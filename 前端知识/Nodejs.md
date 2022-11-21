# Nodejs

## Node介绍

> Node: 在当下的互联网时代，Node的重要性已经不言而喻，诸多国内外的互联网公司都已经有大量的高性能系统运行在Node之上。Node凭借其单线程、异步等举措实现了极高的性能基准。此外，目前最为流行的Web开发模式是前后端分离的形式，即前端开发者和后端开发者在自己喜欢的IDE上独立进行开发，然后通过HTTP或是RPC等方式实现数据与流程等交互。这种开发模式在Node强大功能的引领下变得越来越高效，也越来越受到各个互联网公司的青睐。在GitHub上搜索，你会看到于Node相关的项目是中呈现火爆的态势，这也进一步证明了Node的巨大商业价值与技术价值。因此，无论是前端开发者还是传统的后端开发者，掌握Node已经是刻不容缓的了。而且，Node的完美搭档NPM也提供了异常丰富的工具包与框架供开发者使用。学习Node应该是每一个对前端开发或是后端有追求的人必须要做的事。Node 是基于google的V8引擎来实现的。
> 
> Nodejs是一个框架不是一种语言，依托于javascript
> 
> npm（Node Package Manager），node包管理器
> 
> Nodejs是只有一个线程，依赖于异步处理。
> 
> nodejs官网：nodejs.org
> 
> npm官网：npmjs.com

## Nodejs下载安装

1. 官网下载
   
   > http://node.org 下载安装包直接安装

2. 使用NVM(Node Version Manage)包管理工具来下载
   
   > NVM gitHub地址：<https://github.com/nvm-sh/nvmhttps://github.com/nvm-sh/nvm>
   
   > mac 用户执行命令下载安装
   > 
   > ```bash
   > curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
   > ```
   > 
   > linux客户执行命令安装
   > 
   > ```bash
   > wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
   > ```
   > 
   > `windows系统客户请使用NVM-windows`
   > 
   > 执行日志：
   > 
   > ```bash
   > % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
   >                                  Dload  Upload   Total   Spent    Left  Speed
   > 100 15037  100 15037    0     0   4431      0  0:00:03  0:00:03 --:--:--  4439
   > => Downloading nvm from git to '/Users/jay/.nvm'
   > => Cloning into '/Users/jay/.nvm'...
   > remote: Enumerating objects: 354, done.
   > remote: Counting objects: 100% (354/354), done.
   > remote: Compressing objects: 100% (302/302), done.
   > remote: Total 354 (delta 40), reused 159 (delta 27), pack-reused 0
   > Receiving objects: 100% (354/354), 207.06 KiB | 366.00 KiB/s, done.
   > Resolving deltas: 100% (40/40), done.
   > * (HEAD detached at FETCH_HEAD)
   >   master
   > => Compressing and cleaning up git repository
   > 
   > => Appending nvm source string to /Users/jay/.zshrc
   > => Appending bash_completion source string to /Users/jay/.zshrc
   > => Close and reopen your terminal to start using nvm or run the following to use it now:
   > 
   > export NVM_DIR="$HOME/.nvm"
   > [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
   > [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
   > ```
   > 
   > 看到这个代表安装成功了。因为我们看到命令里面有修改环境变量，所以我们需要重新刷新一下我们的环境变量
   > 
   > ```bash
   > bash: source ~/.bashrc
   > zsh: source ~/.zshrc
   > ksh: . ~/.profile
   > ```
   > 
   > 然后我们执行命令
   > 
   > ```bash
   > nvm -version
   > ```
   
   > 安装Node
   > 
   > ```bash
   > nvm install nodeVersion
   > 
   > 
   > # 例如
   > $ nvm use 16
   > Now using node v16.9.1 (npm v7.21.1)
   > $ node -v
   > v16.9.1
   > $ nvm use 14
   > Now using node v14.18.0 (npm v6.14.15)
   > $ node -v
   > v14.18.0
   > $ nvm install 12
   > Now using node v12.22.6 (npm v6.14.5)
   > $ node -v
   > v12.22.6
   > ```
   
   - 为什么要使用推荐第二种方式安装？
   
   ****
   
   > - Nodejs 有很多的版本，如果我们已经安装了一个版本的话，有了新版本想体验新版本的话需要，再次安装会安装两个版本，之前的版本不会被卸载。这样会有冲突，没办法切换版本，如果我们使用了nvm的话，可以使用vnm来进行版本的切换。
   > 
   > - 如果我们使用第一种方式安装了Nodejs的话，我们想卸载是一个非常麻烦的过程，需要我们找到对应的文件来删除。使用第二种方式的话可以多版本共存。
   
   nvm 命令介绍
   
   > nvm 可以看到使用说明
   > 
   > nvm -version 查看nvm的版本号
   > 
   > nvm use 版本号 切换到对应版本的Nodejs
   > 
   > nvm install 14.7.0 这样就会安装14.7.0版本的Nodejs
   > 
   > nvm install node 会安装最新版本的Nodejs
   > 
   > nvm ls-remote 查看所有版本
   > 
   > nvm which 版本号  查看Nodejs安装位置
   > 
   > ```bash
   > $nvm which 16
   > /Users/jay/.nvm/versions/node/v16.14.0/bin/node
   > ```

# HTTP模块

## 服务端程序编写

### 第一个Nodejs程序

> 写一个服务器启动程序，创建一个js文件，内容如下：

```js
var http = require('http');

var server = http.createServer(function(request, response) {
    response.writeHead(200, {'Content-Type': 'text/plain'});
    response.end('Hello Node.js');

});

// 监听3000端口
server.listen(3000, 'localhost');
console.log('Node Server started on port 3000');
```

进入到刚刚创建的app.js的目录，我们执行node app.js。这时候控制台会打印

```log
$ node app.js
Node Server started on port 3000
```

浏览器访问http://localhost:3000。这个代码的意思就是创建一个服务器，然后server监听本地3000这个端口，3000端口被访问就会执行server里面的回调函数，返回给客户端Hello Node.js

<img src="file:///Users/jay/Library/Application%20Support/marktext/images/2022-03-11-20-53-02-image.png" title="" alt="" width="234">

> 再增加两个回调事件

```js
var http = require('http');

var server = http.createServer(function(request, response) {
    response.writeHead(200, {'Content-Type': 'text/plain'});
    response.end('Hello Node.js');

});

// 监听3000端口
server.listen(3000, 'localhost');

// 当服务器启动监听后会被调用
server.on('listening', function(){
    console.log('Server is listening');
    server.close();
});

// 当客户端与服务器建立好连接之后就会触发
server.on('connection', function(){
    console.log('Client is connected');
});

// 当服务器发生关闭事件的时候会回调
server.on('close', function(){
    console.log('Server is closed');
});

console.log('Node Server started on port 3000');
```

### 第二种写法

```js
const http = require('http');
const httpServer = new http.Server();

httpServer.on('request', function (request, response) {
    response.writeHead(200, { "Content-Type": 'text/plain' });
    response.end('Helo Node.js');
});

httpServer.listen(3000, function () {
    console.log('Node Server started on port 3000');
});
```

### 实现获取request中的信息

> 获取request的信息打印

```js
require('http').createServer(function (request, response) {

    let data = '';

    request.on('data', function (chunk) {
        data += chunk;
    });

    request.on('end', function () {
        let method = request.method;
        let headers = JSON.stringify(request.headers);
        let httpVersion = request.httpVersion;
        let requestUrl = request.url;

        response.writeHead(200, { 'Content-Type': 'text/html' });
        let responseData = method + "," + headers + "," + httpVersion + "," + requestUrl;
        response.end(responseData);
    });

}).listen('3000', function (request, response) {
    console.log('Node Server started on port 3000');
});
```

> 结果： curl localhost:3000

```log
$curl localhost:3000
GET,{"host":"localhost:3000","user-agent":"curl/7.77.0","accept":"*/*"},1.1,/% 
```

## 客户端代码

### 写法一

```js
const http = require('http');

let responseData = '';

http.request({
    'host': '127.0.0.1',
    'port': '3000',
    'method': 'get'
}, function (response) {
    response.on('data', function (chunk) {
        responseData += chunk;
    });
    response.on('end', function () {
        console.log(responseData);
    });

}).end();
```

> 说明：启动一个客户端，请求地址：127.0.0.1，端口3000，方法是get。我们启动app.js。然后再使用这个客户端去连接，没有问题。

### 写法二 GET方法

```js
const http = require('http');

let responseData = "";

http.get({
    "host": "127.0.0.1",
    "port": "3000"
}, function (response) {
    response.on('data', function (chunk) {
        responseData += chunk;
    });
    response.on('end', function () {
        console.log(responseData);
    });
}).end();
```

### 写法三 option

```js
const http = require('http');

let responseData = "";

let option = {
    "host":"127.0.0.1",
    "port":"3000"
};

http.request(option, function(response){
    response.on('data', function(chunk){
        responseData += chunk;
    });
    response.on('end', function(){
        console.log(responseData);
    });
}).end();
```

# URL 模块

## URL解析

```js
const url = require('url');

const httpUrl = "http://localhost:8080?param=1";

const urlObject = url.parse(httpUrl);

console.log(urlObject);
```

## 将字符串转换成URL

```js
const url = require('url');

const urlObject = {
    href: 'http://localhost:8080/?param=1',
    origin: 'http://localhost:8080',
    protocol: 'http:',
    host: 'localhost:8080',
    hostname: 'localhost',
    port: '8080',
    pathname: '/',
    search: '?param=1',
    hash: ''
};

let realAddress = url.format(urlObject);

console.log(realAddress);
```

# querystring模块

## ~~querystring~~

querystring已经过期了，不推荐使用了，代替它的是URLSearchParams。这里还是写一下这个过期的写法

```js
const queryStr = require('querystring');

let paramJson = {
    name: 'zhangsan',
    age: 23
};

let paramStr = queryStr.stringify(paramJson);

console.log(paramStr);
```

代替后的写法

```js
let paramJson = {
    name: 'zhangsan',
    age: 23
};

const params = new URLSearchParams(paramJson);

console.log(params.toString());
```

## 获取指定参数名字的值

```js
let param = new URLSearchParams("name=zhangsan&age=23");
console.log(param.get("name"));
```

# util模块

## 将对象转换成字符串

util.inspect

```js
const util = require('util');
const obj = {
    name: 'zhangsan',
    address: 'shanghai',
    age: 25,
    marrided:false
}

const str=util.inspect(obj,{
    colors: true
});

console.log(str);
```

```log
{ name: 'zhangsan', address: 'shanghai', age: 25, marrided: false }
nodejs/src/app-util.js:13
```

# path模块

## 字符串拼接构建路径

```js
const path = require('path');
const outputPath = path.join(__dirname, "mydir", "hello.js");

console.log(outputPath);
# __dirname是当前文件所在磁盘路径
```

## 扩展名

```js
const path = require('path');

const extInfo = path.extname(path.join(__dirname, 'myDir', 'hello.js'));

console.log(extInfo);
```

## 转对象

```js
const filePath = '/Users/helloworld/node/test.js';

const obj = path.parse(filePath);

console.log(obj);
```

<mark>输出结果</mark>

```json
{
  root: '/',
  dir: '/Users/helloworld/node',
  base: 'test.js',
  ext: '.js',
  name: 'test'
}
```

# DNS模块

## 解析域名

```js
const dns = require('dns');

const domain = 'baidu.com';

dns.resolve(domain, function(error, realAddress){
    if(error){
        console.log(error);
        returnl;
    }
    console.log(realAddress);
});
```

## 通过ip地址获取域名

```js
//通过ip地址获取域名
dns.reverse('114.114.114.114',function(erro, domain){
    console.log(domain);
});
```

# 自定义模版

## 编写模块

```js
let myInfo = {
    name: 'zhangsan',
    age: 20
};

let myFunction = function (inputNumber){
    return inputNumber +=5;
};
```

## 模块的导出

```js
exports.myInfo = myInfo;

exports.myFunction = myFunction;let myInfo = {
    name: 'zhangsan',
    age: 20
};

let myFunction = function (inputNumber){
    return inputNumber +=5;
};

exports.myInfo = myInfo;

exports.myFunction = myFunction;
```

## 模块导入使用

```js
const myModule = require('./myInfo');

console.log(myModule.myInfo);

console.log(myModule.myFunction(2));
```

## 第二种导出方式

```js
const myModule2 = {
    myInfo:{
        name: 'zhangsan',
        age: 23
    },
    myFunction: function (inputNumber) {
        return inputNumber + 5;
    }
};
module.exports = myModule2;
```

# 一个综合例子

<mark> 定义一个Service</mark>

```js
class Service {

    login(username, password){
        console.log('entered UserService login method');

        console.log('info from Service.login:'+ username+","+password);

        return true;
    }
}

module.exports = new Service();
```

<mark> 定义一个服务端接受请求 </mark>

```js
const http = require('http');
const querystring = require('querystring');
const url = require('url');

const service = require('./Service');

http.createServer(function(request,response){
    let data = '';

    request.on('data', function(chunk){
        data+=chunk;
    });

    request.on('end', function(){
        const requestUrl = request.url;
        console.log(requestUrl);

        if(requestUrl.includes('login') && request.method==='GET'){
            const urlParam = url.parse(requestUrl);
            const queryObject = querystring.parse(urlParam.query);
            console.log(queryObject);
            service.login(queryObject.username, queryObject.password);
            response.writeHead(200, { 'Content-Type': 'text/plain' });
            response.end("success");
        }
    });
}).listen(3000,function(){
    console.log('server start');
});
```
