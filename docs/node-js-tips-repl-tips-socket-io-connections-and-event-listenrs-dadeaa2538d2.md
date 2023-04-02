# Node.js Tips — REPL 技巧、Socket.io 连接和事件侦听器

> 原文：<https://javascript.plainenglish.io/node-js-tips-repl-tips-socket-io-connections-and-event-listenrs-dadeaa2538d2?source=collection_archive---------6----------------------->

![](img/06881de7c280c2b94e990eb907090ec1.png)

Photo by [Ana Silva](https://unsplash.com/@noqas?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究编写节点应用程序时常见问题的一些解决方案。

# 删除 Node.js 事件发射器中的事件侦听器

我们可以通过使用`removeListener`方法移除带有节点事件发射器的侦听器。

例如，我们可以写:

```
const events = require('events');
const EventEmitter = events.EventEmitter;
const rr = new EventEmitter();
const refreshHandler = () => {
  console.log('refresh');
}
rr.on("refresh", refreshHandler);
rr.removeListener("refresh", refreshHandler);
```

我们创建了我们的`EventEmitter`实例。

然后我们将`refreshHandler`事件监听器附加到`refresh`事件。

如果我们不再需要收听`refresh`事件，那么我们可以调用`removeListener`将事件收听者从记忆中清除。

我们传入事件名称作为`removeListener`的第一个参数。

第二个是我们要移除的事件处理程序。

# node . js . REPL 中“_”(下划线)符号的含义

`_`符号返回 REPL 中最后一个记录表达式的结果，

例如，如果我们键入:

```
> 2 * 3
6
```

然后`_`返回 6:

```
> _
6
```

# Node.js 中执行回调函数的访问变量

`exec`接受一个回调函数，其结果是运行一个 shell 命令。

例如，我们可以写:

```
const execChild = (callback) => {
  const child = exec(cmd, (error, stdout, stderr) => {
    callback(stdout);
  });
}
```

我们从回调中得到`stdout`，回调中有我们的`cmd`命令的结果。

然后我们调用`callback`来获得`exec`回调之外的结果。

我们必须这样做，因为`exec`是异步的，所以我们不知道什么时候会得到结果。

现在我们可以通过书写来使用`execChild`功能:

```
execChild((result) => console.log(result));
```

我们进入`callback`的`stdout`将是`result`。

# socket . io—侦听连接事件

我们可以监听服务器端的连接。

然后我们从客户端建立连接。

在服务器端代码中，我们写道:

```
const io = require('socket.io').listen(8001);
io.sockets.on('connection', (socket) => {
  console.log('user connected');
});
```

收听`connection`事件，如果我们调用`io.connect`，该事件将从客户端发出。

然后在我们的客户端代码中，我们写道:

```
const socket = io.connect('http://localhost:8001');
```

连接到我们创建的服务器。

# 获取 Node.js 中的完整文件路径

我们可以通过`__dirname`获取当前目录的完整路径。

然后 w 可以写:

```
const path = require("path");
const fileName = "file.txt";
const fullPath = path.join(__dirname, "/uploads/", fileName);
```

我们使用`path.join`以平台无关的方式将所有路径段连接在一起。

同样，我们可以使用`path.resolve`方法返回文件的完整路径。

例如，我们可以写:

```
const path = require("path");
const fullPath = path.resolve("./uploads/file.csv");
```

我们用相对路径调用`resolve`来获取文件的完整路径。

# 使用 Express 获取客户的 IP

如果我们正在编写一个 Express 应用程序，那么我们可以使用`req.ip`来获取客户端的 IP 地址。

我们还可以通过编写以下内容来获得`x-forward-for`头的值:

```
req.headers['x-forwarded-for']
```

来获取报头的值，其中包含 IP 地址。

我们也可以检查`req.connection.remoteAddress`来做同样的事情。

# 连接到插座。具有特定路径和名称空间的 Io 服务器

通过使用`of`方法和`path`属性，我们可以连接到具有特定路径和名称空间的 socket.io 服务器。

例如，我们可以写:

```
const io = require('socket.io')(http, {
  path: '/foo/bar'
});io
  .of('/namespace')
  .on('connection', (socket) => {
    socket.on('message', (data) => {
      io.of('namespace').emit('message', data);
    });
  });
```

我们将`http`服务器实例传递给`socket.io`函数。

第二个参数让我们用`path`属性指定路径。

然后在`io.of`方法中，我们指定名称空间。

`on`方法让我们观察来自客户端的连接。

我们可以用`socket.on`监听来自客户端的事件。

第一个参数是事件名称。

第二个是一个回调，它包含随事件发出的数据。

![](img/6069917af20f01d6a4009a6c022c2b6f.png)

Photo by [Adam Grabek](https://unsplash.com/@agmakonts?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用`removeListener`来清除事件监听器。

我们可以用 socket.io 连接到特定的路径和名称空间。

用 Node 很容易获得各种文件和网络信息。

我们可以使用`_`来获取节点 REPL 中返回的最后一个结果。

# 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**