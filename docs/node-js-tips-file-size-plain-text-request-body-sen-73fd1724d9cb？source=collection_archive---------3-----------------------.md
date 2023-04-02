# Node.js 提示—文件大小、纯文本请求正文、发送消息

> 原文：<https://javascript.plainenglish.io/node-js-tips-file-size-plain-text-request-body-sen-73fd1724d9cb?source=collection_archive---------3----------------------->

![](img/17a6f42bf1528c695289fee6a5ccd377.png)

Photo by [Nikolay Tchaouchev](https://unsplash.com/@nxn?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 强制将请求正文解析为纯文本，而不是 Express 中的 JSON

我们可以移除主体解析器中间件，将请求主体作为原始文本进行解析。

例如，我们可以写:

```
const express = require('express');const app = express();const parseRawBody = (req, res, next) => {
  req.setEncoding('utf8');
  req.rawBody = '';
  req.on('data', (chunk) => {
    req.rawBody += chunk;
  });
  req.on('end', () => {
    next();
  });
}app.use(parseRawBody);app.post('/test', (req, res) => {
  res.send(req.rawBody);
});app.listen(3000);
```

我们创建了自己的`parseRawBody`中间件，接受请求、响应对象和`next`函数。

`req`是请求，是读流。

我们可以通过监听`data`事件从中获取数据。

在`data`事件的处理程序中，我们将文本块连接到`req.rawBody`属性。

然后，当发出`end`事件时，流就不再有数据要发出，所以我们调用`next`来移动到下一个中间件。

然后我们可以在随后的路线中访问`req.rawBody`。

如果我们使用 body-parser 包，那么我们可以使用`bodyParser.text()`中间件将请求体保持为纯文本。

例如，我们可以写:

```
app.use(bodyParser.text());
```

然后，我们可以通过编写以下内容来访问请求体:

```
app.get('/', (req, res) => {
  res.send(req.body);
})
```

`req.body`具有带请求体的字符串。

# 使用 Socket.io 和空消息队列向特定客户端发送消息

要向特定的客户端发送消息，并使用 socket.io 清空消息队列，我们可以监听`connection`事件，然后将`socket`对象设置为一个对象的属性。

例如，我们可以写:

```
const socketio = require('socket.io');
const clients = {};
const io = socketio.listen(app);io.sockets.on('connection', (socket) => {
  clients[socket.id] = socket;
});
```

属性名是套接字的 ID，由`socket.id`属性表示。

我们保存了每个客户端的套接字，以便我们稍后可以调用`emit`向客户端发送消息。

然后我们可以写下一条信息:

```
const socket = clients[socketId];
socket.emit('show', {});
```

其中`socketId`是我们之前设置的套接字的 ID。

我们用自己的事件和想要发送的数据调用`emit`。

# 修改快速请求对象

要修改 Express 的请求对象，我们可以在请求对象中分配我们的属性。

例如，我们可以写:

```
app.use((req, res, next) => {
  req.root = `${req.protocol}://${req.get('host')}`;
  next();
});
```

我们将`req.root`属性设置为 URL 的协议和主机的组合。

回调是一个我们可以在任何地方使用的中间件。

`next`是调用链中下一个中间件的函数。

# 在 Node.js 中查找文件的大小

我们可以用`stateSync`找到一个文件的大小。

例如，我们可以写:

```
function getFileSize(filename) {
  const stats = fs.statSync(filename);
  const fileSizeInBytes = stats.size;
  return fileSizeInBytes;
}
```

我们在`filename`参数上调用`statsSync`，这是一个文件的字符串路径。

然后我们可以用`size`属性得到字节大小。

我们也可以使用`filesize`包来获取文件大小。

例如，我们可以写:

```
const filesize = require("filesize");
const stats = fs.statSync("foo.txt");
const fileSizeInMb = filesize(stats.size, {round: 0});
```

这个包的好处是我们可以把尺寸转换成我们喜欢的任何格式。

例如，我们可以改变基底，将其转换为对象，等等。

上面的示例将大小转换为以兆字节为单位的文件大小。

我们可以通过编写以下内容进行其他转换:

```
const filesize = require("filesize");
const stats = fs.statSync("foo.txt");
const fileSizeMb = filesize(stats.size, {round: 0});         
const fileSizeArray = filesize(stats.size, {output: "array"});  
const fileSizeObj = filesize(stats.size, {output: "object"});
```

数组将大小和单位作为数组的单独条目。

`object`把大小、并集、指数放在自己的属性里。

![](img/5dbacb16a8b00ff113d761e9ca89c38a.png)

Photo by [Cristina Anne Costello](https://unsplash.com/@lightupphotos?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用各种方式保存请求正文。

有一些库可以帮助我们得到我们想要的格式的文件大小。

我们可以用 Socket.io 向特定的客户端发送消息。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**