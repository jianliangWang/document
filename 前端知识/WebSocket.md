# WebSocket

> socket.io安装
> 
> 可以直接使用npm安装，npm install socket.io:version

## 1. 编写一个服务端

```js
const io = require('socket.io');
const fs = require('fs');
const http = require('http');

const server = http.createServer((request, response) => {
    response.writeHead(200, { "Conetent-type": "text/html" });
    if (request.url === '/') {
        fs.readFile('./index.html', (error, data) => {
            if (error) {
                console.log(error);
                return;
            }
            console.log('client comming');
            response.end(data.toString());
        });
    } else {
        response.end("<html><body>404</body></html>");
    }
});
server.listen(3000, 'localhost')
const sio = new io.Server(server);
sio.on('connection', (socket) => {
    socket.on('message', (message) => {
        console.log('message:' + message);
    });
    socket.on('disconnect', () => {
        console.log("connection has lost");
    });

    //自定义一个事件
    socket.emit('serverEvent', 'server message');

    socket.on('clientEvent', (data) => {
        console.log(data);
    });

    socket.on('broadcastEventClient', (message) => {
        console.log("收到客户上线消息"+message+"要广播给其他客户端");
        socket.broadcast.emit('broadcastEventServer', 'you are good');
    });

    socket.send('hello client');
});
```

## 2. 编写一个客户端

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    </meta>
    <title>index</title>

    <script src="/socket.io/socket.io.js"></script>
    <script>
        const socket = io('localhost:3000');
        socket.on('message', (message) => {
            console.log(message);
        });
        socket.on('disconnect', () => {
            console.log('disconnect');
        });

        socket.on('serverEvent', (data) => {
            console.log('this is ' + data);

            socket.emit('clientEvent', 'client event');
        });

        socket.emit('broadcastEventClient', 'I am comming');

        socket.on('broadcastEventServer', (data) => {
            console.log("收到广播消息" + data);
        });


    </script>
</head>

</html>
```

实现的功能：当启动服务端，打开一个网页访问http://localhost:3000 服务端就会收到客户端连接，会将html内容返回给客户端。然后客户端会与服务端建立socket连接，连接成功之后收到服务端的发送内容。客户端自定义事件：clientEvent，服务端监听这个事件就能获得客户端触发这个事件的时候发送的数据。同理，服务端也可以自定义事件serverEvent，客户端监听。服务端自定义广播事件。
