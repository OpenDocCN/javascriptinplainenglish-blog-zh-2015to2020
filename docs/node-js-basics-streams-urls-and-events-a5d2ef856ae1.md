# Node.js 基础——流、URL 和事件

> 原文：<https://javascript.plainenglish.io/node-js-basics-streams-urls-and-events-a5d2ef856ae1?source=collection_archive---------5----------------------->

![](img/a15bd61d83979012319f72b0a9ed6839.png)

Photo by [Annie Spratt](https://unsplash.com/@anniespratt?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是世界上最流行的编程语言之一。为了有效地使用它，我们必须了解它的基础知识。

在本文中，我们将了解 Node.js 的基础知识，包括创建流。

# 流

可写流在 Node 中的许多地方被使用。

这些流是具有`write`方法的对象，可以通过字符串或`Buffer`对象向流中写入内容。

`end`方法关闭流，并可选地在关闭前取一个值写入流。

我们可以从`fs`模块创建一个指向具有`createWriteStream`功能的文件的可写流。

然后我们可以使用`write`方法一次写一个文件，而不是像`writeFile`一样一次写完。

可读流可以从各种来源读取数据。

使用事件处理程序读取数据。

在节点中发出事件的对象有一个`on`方法来监听事件。

可读流发出`data`和`end`事件。

`data`每当数据进入时被触发，

`end`在流结束时调用。

它适用于可以立即处理的流式数据。

例如，我们可以将 HTTP 服务器发送的响应更改如下:

```
const { createServer } = require("http");createServer((request, response) => {
  response.writeHead(200, { "Content-Type": "text/plain" });
  request.on("data", chunk =>
    response.write(chunk.toString().toLowerCase()));
  request.on("end", () => response.end());
}).listen(8000);
```

然后，如果我们用纯文本正文进行请求，则纯文本将被转换为小写。

这是因为`chunk`有部分请求体，我们把它们写到读流中。

读取流以`end`事件的发射结束，我们结束响应。

# 节点. js 文件服务器

我们可以使用`fs`模块和`url`模块从请求中解析路径，为我们的服务器提供文件。

例如，我们可以写:

```
const http = require('http');
const url = require('url');
const fs = require('fs');http.createServer((req, res) => {
  const q = url.parse(req.url, true);
  const filename = `.${q.pathname}`;
  fs.readFile(filename, (err, data) => {
    if (err) {
      res.writeHead(404, {'Content-Type': 'text/html'});
      return res.end("404 Not Found");
    } 
    res.writeHead(200, {'Content-Type': 'text/html'});
    res.write(data);
    return res.end();
  });
}).listen(8080);
```

在上面的代码中，我们从文件名中获取文件的路径。

`url`模块有一个`parse`方法，通过`pathname`属性获取文件的路径。

然后使用`readFile`读取给定相对路径的文件。

在回调中，如果文件存在，我们就编写它。

如果有错误，我们会返回 404 响应。

所以当我们向`http://localhost:8080/foo.txt`提出请求时，如果它存储在服务器上，我们会得到`foo.txt`的内容。

# 事件

我们可以用`events`模块发出并监听事件。

例如，我们可以写:

```
const events = require('events');
const eventEmitter = new events.EventEmitter();
```

创建一个新的`EventEmitter`对象。

然后我们可以使用`emit`方法来发出一个事件:

```
eventEmitter.emit('hello');
```

我们可以通过书写来听:

```
eventEmitter.on('hello', () => {
  console.log('got hello');
});
```

我们可以向事件发送数据。例如，我们可以写:

```
eventEmitter.emit('hello', 'james');
```

然后`'james'`就是我们的数据。

那么我们可以通过写下来获得数据:

```
eventEmitter.on('hello', (data) => {
  console.log(`hello ${data}`);
});
```

`data`有我们发送的数据。

![](img/ec1c0e46c3e646f24999fdedc8bf2e00.png)

Photo by [Free To Use Sounds](https://unsplash.com/@freetousesoundscom?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

流是可以传递和读写的数据。

可读流是可以成块读取数据的流。

可写流可以分块写入数据。

我们可以使用`url`模块将 URL 解析成我们可以使用的东西。

`EventEmitter`构造函数可以用来创建事件发射器和监听事件。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**