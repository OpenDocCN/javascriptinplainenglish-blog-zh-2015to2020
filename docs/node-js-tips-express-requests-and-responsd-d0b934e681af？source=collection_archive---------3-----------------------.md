# Node.js 提示—快速请求和响应

> 原文：<https://javascript.plainenglish.io/node-js-tips-express-requests-and-responsd-d0b934e681af?source=collection_archive---------3----------------------->

![](img/52ee52311e9438595332c93ac042edc8.png)

Photo by [Nikolay Tchaouchev](https://unsplash.com/@nxn?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 使用 Node 或 Express 返回 JSON

我们可以通过设置`Content-Type`头和`res.end`方法来呈现带有`http`包的 JSON 响应。

例如，我们可以写:

```
const http = require('http');const app = http.createServer((req, res) => {
  res.setHeader('Content-Type', 'application/json');
  res.end(JSON.stringify({
    a: 1
  }));
});app.listen(3000);
```

我们调用`createServer`来创建一个 HTTP 服务器。

在回调中，我们将`Content-Type`响应头设置为`application/json`，以将响应数据设置为 JSON。

然后我们用字符串化的 JSON 调用`res.end`来呈现响应。

使用 Express，我们只需编写:

```
res.json({
  foo: "bar"
});
```

在我们的中间件中。

`res.json`设置头并呈现 JSON。

# 通过快速启用 HTTPS

我们可以通过读取证书数据来启用 HTTPS 和 Express，并使用`http.createServer`来创建服务器。

例如，我们可以写:

```
const fs = require('fs');
const http = require('http');
const https = require('https');
const privateKey = fs.readFileSync('sslcert/server.key', 'utf8');
const certificate = fs.readFileSync('sslcert/server.crt', 'utf8');
const credentials = {
  key: privateKey,
  cert: certificate
};
const express = require('express');
const app = express();

const httpServer = http.createServer(app);
const httpsServer = https.createServer(credentials, app);
httpServer.listen(3000);
httpsServer.listen(3443);
```

我们从服务器读取私钥和证书文件。

然后我们在一个对象中传递它们，这个对象被传递给`https.createServer`来创建服务器。

然后我们调用`httpsServer`上的`listen`来创建服务器。

我们仍然需要创建 HTTP 服务器来监听不安全的 HTTP 请求。

# 修复 Express 中的“错误:请求实体太大”错误

我们可以通过增加允许的请求大小来修复“请求实体太大”的错误。

例如，我们可以写:

```
const bodyParser = require('body-parser');
app.use(bodyParser.json({
  limit: '50mb'
}));
app.use(bodyParser.urlencoded({
  limit: '50mb',
  extended: true
}));
```

我们在作为参数传入的对象中设置了`limit`,以设置请求负载的最大大小。

`'50mb'`是 50 兆。

# 使用 Express 从节点服务器下载文件

我们可以用`res.download`向客户端发送文件下载响应。

例如，我们可以写:

```
app.get('/download', (req, res) => {
  const file =  path.join(__dirname, '/upload-folder/movie.mp4');
  res.download(file);
});
```

`res.download`需要文件的绝对路径，所以我们需要将`__dirname`和路径的其余部分结合起来。

`res.download`会自动设置`Content-Disposition`等头，并将文件发送给客户端。

# 使用 Express 的服务器静态文件

我们可以使用`express.static`中间件通过 Express 提供静态文件。

例如，我们可以写:

```
app.use(express.static(path.join(__dirname, '/public')));
```

我们将我们想要用作静态文件夹的文件夹作为绝对路径。

`express.static`将文件夹暴露给客户端。

# 在 Express 应用程序中获取 JSON POST 数据

我们可以通过使用`req.body`属性从 POST 请求中获取 JSON 数据。

例如，我们可以写:

```
const express = require('express');
const bodyParser = require('body-parser');
const app = express.createServer();app.use(bodyParser.json());app.post('/', (req, res) => {
  console.log(req.body); 
  response.send(req.body);
});app.listen(3000);
```

`req.body`拥有 JSON 请求有效载荷。

如果我们运行了`bodyParser.json()`中间件，那么`req.body`属性是可用的。

我们是这样做的:

```
app.use(bodyParser.json());
```

# 使用 Node.js Mongoose 删除文档

我们可以通过在模型类上使用`remove`方法来删除带有 Mongoose 的文档。

例如，我们可以写:

```
Model.remove({
  _id: id
}, (err) => {
  if (err) {
    console.log(err);
  }
});
```

# 使用 Express 获取上传的文件

要使用 Express 获取上传的文件，我们可以使用`express-fileupload`包。

例如，我们可以写:

```
const fileupload = require("express-fileupload");
app.use(fileupload());app.post("/upload", (req, res) => {
  if (!req.files) {
    res.send("File not found");
    return;
  }
  const file = req.files.photo;
  res.send("File Uploaded");
});
```

我们使用了来自`express-fileupload`包的`fileupload`中间件。

为了使用它，我们写:

```
app.use(fileupload());
```

当我们上传一个或多个文件时,`req.files`应该是可用的。

我们可以通过使用`req.files`对象来获取文件。

`photo`是输入文件的字段名。

我们可以用正确的名字来代替。

![](img/6241d04dd2819ac6c9a6a4adf841b45b.png)

Photo by [Viktor Talashuk](https://unsplash.com/@viktortalashuk?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

有几种方法可以将 JSON 响应呈现给客户端。

我们可以用一个中间件包来上传文件。

同样，我们可以用`res.download`返回一个文件下载响应。

我们必须使用 body-parser 来解析 JSON 请求负载。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**