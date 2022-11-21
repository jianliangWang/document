# Nodejs连接数据库

## 连接Mysql数据库

1. ### npm 安装mysql包

```bash
npm install mysql
```

2. ### 代码

```js
const mysql = require('mysql');

const uuid = require('uuid');

const connection = mysql.createConnection({
    host: 'localhost',
    user: 'root',
    password: '123456',
    database: 'test'
});

connection.connect((error) => {
    if (error) {
        console.log(error);
    } else {
        console.log('connection successful');
        connection.query('insert into users set ?', {
            id: uuid.v1(),
            name: "zhangsan",
            age: 25
        }, (error, result) => {

            if (error) {
                console.log(error);
            } else {
                console.log(result);
                connection.query('select * from users', (error, result) => {
                    if (error) {
                        console.log('select error occured' + error);
                    } else {
                        console.log(result);
                        connection.end((error) => {
                            if (error) {
                                console.log('end error occured');
                            } else {
                                console.log('end success');
                            }
                        });
                    }
                });
            }
        });
    }
});
```

## 连接mongoDB数据库

### 1. 安装mongoose包

```js
npm install mongoose
```

### 2. 连接代码

```js
const mongoose = require('mongoose');

const uri = 'mongodb://admin:123456@localhost:27017/admin';

mongoose.connect(uri, { useNewUrlParser: true, useUnifiedtopology: true }, (error) => {
    if (error) {
        console.log(error);
    } else {
        console.log('connection successful');
        const parentSchema = new mongoose.Schema({
            name: String,
            age: Number,
            address: String
        });

        const studentSchema = new mongoose.Schema({
            name: String,
            age: Number,
            address: String,
            married: Boolean,
            parents: parentSchema
        });

        mongoose.model('student', studentSchema);


        const Student = mongoose.model('student');
        const student = new Student({
            name: 'zhangsan',
            age: 20,
            address: 'tianjin',
            married: false,
            parents: {
                name: 'lisi',
                age: 68,
                address: 'dalian'
            }
        });

        student.save(error => {
            if (error) {
                console.log(error);
            } else {
                console.log('save successful');
                Student.find({}, (error, docs) => {
                    if (error) {
                        console.log(error);
                    } else {
                        console.log(docs);
                        docs.forEach(doc => {
                            doc.remove();
                        });

                    }
                });
            }
        });

    }
});
```

## 连接redis数据库

### 1. 安装ioredis

```js
npm install ioredis
```

### 2. 代码

```js
const Redis = require('ioredis');
const redisKeyPrefix = 'myRedis:info:user:';

class UserRedisDao {
    getRedisConnection() {
        return new Redis({
            host: 'localhost',
            port: 6379
        });
    }
      async sotreUserId(userSessionId, userId) {
        const redis = this.getRedisConnection();
        redis.set(redisKeyPrefix + userSessionId, userId, 'ex', 1800, (error, result) => {
            redis.quit();// 关闭连接
        });
    }
}
```

这样就连上了redis，可以往里面存储值。

### 3. 一个例子

> 用户登录以后，将用户信息保存到redis中，Key为用户的sessionId。值是用户的name。需要查询到时候从redis中查询用户信息，通过用户的id获取用户信息。更新用户信息的过期时间。用户退出将用户信息从redis中删除。

1. dao层代码

```js
const Redis = require('ioredis');
const redisKeyPrefix = 'myRedis:info:user:';

class UserRedisDao {
    getRedisConnection() {
        return new Redis({
            host: 'localhost',
            port: 6379
        });
    }

    async sotreUserId(userSessionId, userId) {
        const redis = this.getRedisConnection();
        redis.set(redisKeyPrefix + userSessionId, userId, 'ex', 1800, (error, result) => {
            redis.quit();// 关闭连接
        });
    }

    async getUserIdFromUIserSessionByUserSessionId(userSessionId) {
        const redis = this.getRedisConnection();
        return redis.get(redisKeyPrefix + userSessionId, (error, userId) => {
            redis.quit;
            return userId;
        });
    }

    // 重置过期时间
    async resetUserSessionExpirationTime(userSessionId) {
        const redis = this.getRedisConnection();
        redis.expire(redisKeyPrefix + userSessionId, 1800, (error, result) => {
            redis.quit();
        });
    }

    // 删除key
    async removeUserSessionByUserSessionId(userSessionId){
        const redis = this.getRedisConnection();
        redis.del(redisKeyPrefix+userSessionId, (error, result) => {
            redis.quit();
        });
    }
}
module.exports = new UserRedisDao();
```

2. service层代码

```js
const userRedisDao = require('../db/redis/userRedisDao');

class UserService{

    async sotreUserId(userSessionId, userId){
        await userRedisDao.sotreUserId(userSessionId, userId);
    }

    async getUserIdFromUIserSessionByUserSessionId(userSessionId){
        return await userRedisDao.getUserIdFromUIserSessionByUserSessionId(userSessionId);
    }

    async resetUserSessionExpirationTime(userSessionId){
        await userRedisDao.resetUserSessionExpirationTime(userSessionId);
    }

    async removeUserSessionByUserSessionId(userSessionId){
        await userRedisDao.removeUserSessionByUserSessionId(userSessionId);
    }
}

module.exports = new UserService();
```

3. controller层代码

```js
const userService = require('../service/userService');

const uuid = require('uuid');

class UserController {

    async userLogin(userName, password){
        const userId = userName;
        const userSessionId = uuid.v1();

        await userService.sotreUserId(userSessionId, userId);
    }

    async userLogout(userSessionId){
        await userService.removeUserSessionByUserSessionId(userSessionId);
    }

    async userOtherOperation(userSessionId) {
        const userId = await userService.getUserIdFromUIserSessionByUserSessionId(userSessionId);

        console.log('userId from UserController: ' + userId);
        await userService.resetUserSessionExpirationTime(userSessionId);
    }
}

module.exports = new UserController();
```

4. 服务器层代码

```js
const http = require('http');
const querystring = require('querystring');
const url = require('url');
const { userLogin } = require('./controller/userController');
const usercontroller = require('./controller/userController');

const server = http.createServer((request, response) => {
    let data = '';

    request.on('data', (chunk) => {
        data += chunk;
    });

    request.on('end', () => {
        const requestUrl = request.url;
        const requestMethod = request.method;

        console.log(requestUrl);

        if (requestUrl.includes('login') && requestMethod === 'GET') {
            const requestParams = url.parse(requestUrl);
            const queryObject = querystring.parse(requestParams.query);
            console.log(queryObject);

            usercontroller.userLogin(queryObject.username, queryObject.password);

            response.writeHead(200, { 'Content-Type': 'text/plain' })
            response.end('username: ' + queryObject.username + " password:" + queryObject.password);
        } else if (requestUrl.includes('logout') && requestMethod === 'GET') {
            const requestParams = url.parse(requestUrl);
            const queryObject = querystring.parse(requestParams.query);
            usercontroller.userLogout(queryObject.userSessionId);
            response.writeHead(200, { 'Content-Type': 'text/plain' });
            response.end('user logout');
        } else if (!requestUrl.includes('/favicon.ico')) {
            const requestParams = url.parse(requestUrl);
            const queryObject = querystring.parse(requestParams.query);
            usercontroller.userOtherOperation(queryObject.userSessionId);
            response.writeHead(200, { 'Content-Type': 'text/plain' });
            response.end('user other operation, expiration time');
        }
    });
}).listen(3000, () => {
    console.log("listening start");
});
```


