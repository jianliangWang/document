# Process(进程)模块

## Process

### Process属性

- version

```js
// 当前node版本
console.log(process.version);
```

- versions

```js
// node和依赖的库版本
console.log(process.versions);
```

- platform

```js
// 平台版本
console.log(process.platform);
```

- execPath

```js
// 安装地址
console.log(process.execPath);
```

- config

```js
// 配置信息
console.log(process.config);
```

- pid

```js
// 当前运行的node进程id
console.log(process.pid);
```

- title

```js
// 当前正在运行的node窗口的标题
console.log(process.title);
```

- arch

```js
//当前操作系统的架构
console.log(process.arch);
```

- env

```js
// 环境属性
console.log(process.env);
```

### 方法

- memoryUsage

```js
// 内存情况
console.log(process.memoryUsage());
```

- cwd

```js
// 当前工作目录 Current Work Directory
console.log(process.cwd());
```

- chdir

```js
// 修改当前工作目录
process.chdir('../');
```

- uptime

```js
// 执行时间
console.log(process.uptime());
```

### 事件

#### 退出

```js
// 退出事件
process.on('exit', () => {
    console.log('process exited');
});


// 执行退出
process.exit(0);
```

#### 退出前执行

```js
process.on('beforeExit', () => {
    console.log('before process exited');
});
```

#### 未捕获异常

```js
process.on('uncaughtException', () => {
    console.log('uncaughtException');
});
```

#### 信号

```js
process.on('SIGINT', () => {
    console.log("received SIGINT info");
});

setTimeout(() => {
    console.log('timeout');
}, 10000);
```

### nextTick

> 在同步方法执行之后执行，在异步方法执行之前执行

> 例子：

```js
const fs = require('fs');

const myFunction = ()=>{
    console.log('myFunction invoked');
}

// 在同步方法执行之后执行，在异步方法执行之前执行
process.nextTick(myFunction);


const data = fs.readFileSync('./app.js');
console.log(data.toString());

console.log('这里是分割线=========================');
fs.readFile('./app.js',(error, data)=>{
    console.log(data.toString());
});
```

## 子进程child_process

1. ### spawn

```js
const childProcess = require('child_process');

const nodeChildProcess = childProcess.spawn('node',['app3']);

nodeChildProcess.stdout.on('data', (data)=>{
    console.log(data.toString());
    console.log(`child process id ${nodeChildProcess.pid}`);
});

nodeChildProcess.on('exit', (code, signal)=>{
    console.log(code);
});
```

2. ### fork

```js
// fork是spawn的一种特殊形式，只能生成node的子进程
const childProcess = require('child_process');

const forkProcess = childProcess.fork('./app6',{silent:true});

// 接收子进程发送的消息
forkProcess.on('message', (message) =>{
    console.log(message);
});

// 向子进程发送消息
forkProcess.send('hello world');
```

app6.js

```js
[1,2,3,4,5,6].forEach(i => {
    console.log(i);
});

process.on('message', (message)=>{
    console.log(message);
    process.send('welcome');
});
```

3. ### exec

```js
const childProcess = require('child_process');
childProcess.exec('node app6',(error,stdout,stderr)=>{
    if(error){
        console.log(error);
        return;
    }
    console.log(stdout);
});
```

4. ### execFile

```js
const childProcess = require('child_process');
childProcess.exec('node app6',(error,stdout,stderr)=>{
    if(error){
        console.log(error);
        return;
    }
    console.log(stdout);
});
```

> **主线程和子线程通讯是通过ipc(Inter-Process Communication)来完成的。**

## master与cluster

> 主进程负责发送到不同的cluster，之所以能够都监听3000端口，是因为本质只是主线程进行监听，将请求转发到不同的cluster。

```js
const cluster = require('cluster');
const http = require('http');
const os = require('os');

const cpuCount = os.cpus().length;

if (cluster.isMaster) {
    for (let i = 0; i < cpuCount; i++) {
        cluster.fork();
    }

    cluster.on('exit', (worker, code, signal) => {
        console.log(worker.process.pid);
    });
} else {
    const httpServer = http.createServer((request, response) => {
        let data = '';
        request.on('data', (chunk) => {
            data += chunk;
        });
        request.on('end', () => {
            response.writeHead(200, { 'ContentType': 'text/plain' });
            response.end(`${process.pid}`);
        });
    });
    httpServer.listen(3000, () => {
        console.log('listening to port 3000');
    });
}
```

cluster一旦fork之后子进程就会从头到尾进行执行当前文件，所以需要判断是主进程还是子进程。主进程和子进程是通过socket进行通讯。

使用了经典的Master-Worker模式
