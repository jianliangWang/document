# koa

## koa是什么？

Koa是一个由Express背后团队设计的新的web框架，它对于web迎来和API来说更小巧，更灵活，更健壮。通过利用异步函数，Koa允许你抛弃回调并大大提高错误处理能力。Koa内核没有绑定任何的中间件，它提供了更简单的方法套件使编写服务端更快和更加有意思。

## 安装koa

Koa 依赖 **node v7.6.0** 或 ES2015及更高版本和 async 方法支持.

你可以使用自己喜欢的版本管理器快速安装支持的 node 版本：

```bash
$ nvm install 7
$ npm i koa
$ node my-koa-app.js
```

## 使用koa

```js
const Koa = require('koa');
const app = new Koa();

app.use(async (ctx) => {
    ctx.type = 'text/html';
    //ctx.response.type = 'text/html';
    ctx.body = '<h2>Hello Koa</h2>';
    //ctx.response.body = '<h2>Hello Koa</h2>';
});

app.listen(3000);
```

## koa洋葱模型

```js
const Koa = require('koa');
const app = new Koa();

// 自定义中间件 洋葱模型
app.use(async, (ctx, next) =>{
    console.log('myFunction started');
    await next();
    console.log('myFunction finished');
});

app.use(async, (ctx, next) =>{
    console.log('myFunction1 started');
    await next();
    console.log('myFunction1 finished');
});

app.use(async, (ctx, next) =>{
    console.log('myFunction2 started');
    await next();
    console.log('myFunction2 finished');
});

app.use(async (ctx) => {
    ctx.type = 'text/html';
    //ctx.response.type = 'text/html';
    ctx.body = '<h2>Hello Koa</h2>';
    //ctx.response.body = '<h2>Hello Koa</h2>';
});

app.listen(3000);
```



代码地址：
