# 使用 Restify 开始后端开发

> 原文：<https://javascript.plainenglish.io/getting-started-back-end-development-with-restify-136482e123f5?source=collection_archive---------10----------------------->

![](img/814a00e1407e288406d1cd8e12518140.png)

Photo by [Jan Padilla](https://unsplash.com/@janpadilla?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Restify 是一个简单的节点后端框架。

在本文中，我们将看看如何用 Restify 创建一个简单的后端应用程序。

# 快速启动

要开始 Restify，我们可以通过运行以下命令来安装`restify`包:

```
npm i restify
```

然后，我们可以编写第一个应用程序:

```
var restify = require('restify');function respond(req, res, next) {
  res.send(`hello ${req.params.name}`);
  next();
}var server = restify.createServer();
server.get('/hello/:name', respond);
server.head('/hello/:name', respond);server.listen(8080, function() {
  console.log('%s listening at %s', server.name, server.url);
});
```

我们导入`restify`包。

然后我们用`req`、`res`和`next`参数创建我们的`respond`函数。

`req`已经请求数据。

让我们用它提供的方法创建我们的响应。

`next`是调用下一个中间件的函数。

然后我们用`restify.createServer`方法创建了我们的服务器。

然后我们用`get`方法定义了我们的路由，以定义一个 GET 路由。

第一个参数是 URL。

`:name`是 URL 参数的占位符。

我们可以像使用`respond`功能一样使用`req.params.name`来访问参数值。

`server.head`针对 HTTP 谓词在给定路径上安装一个链。

我们需要它来让我们倾听请求。

现在，当我们转到`https://localhost:8000/hello/name`时，我们得到:

```
"hello name"
```

在屏幕上。

URL 参数根据它们的位置自动映射。

# Sinatra 风格的处理器链

我们可以有一系列的路由处理器。

例如，我们可以写:

```
var restify = require('restify');function respond(req, res, next) {
  res.send(`hello ${req.params.name}`);
  next();
}var server = restify.createServer();
server.get('/', function(req, res, next) {
  res.send('home')
  return next();
});server.post('/foo',
  function(req, res, next) {
    req.someData = 'foo';
    return next();
  },
  function(req, res, next) {
    res.send(req.someData);
    return next();
  }
);server.listen(8080, function() {
  console.log('%s listening at %s', server.name, server.url);
});
```

添加获取和发布路由。

邮路有两个中间站。

我们调用`next`以便调用下一个。

因此，当我们向`https://localhost:8080/foo`发出 POST 请求时，我们看到`'foo'`是响应。

有 3 个不同的处理程序链。

`pre`是路由前运行的处理器链。

`use`是一个处理程序链运行后的路由。

而`{httpVerb}`是一个运行在特定路线上的处理程序链。

我们可以通过编写以下代码来运行一个`pre`中间件:

```
var restify = require('restify');function respond(req, res, next) {
  res.send(`hello ${req.params.name}`);
  next();
}var server = restify.createServer();
server.pre(restify.plugins.pre.dedupeSlashes());server.get('/', function(req, res, next) {
  res.send('home')
  return next();
});server.post('/foo',
  function(req, res, next) {
    req.someData = 'foo';
    return next();
  },
  function(req, res, next) {
    res.send(req.someData);
    return next();
  }
);server.listen(8080, function() {
  console.log('%s listening at %s', server.name, server.url);
});
```

我们在运行任何其他东西之前运行`dedupeSlashes`中间件，以去除请求中多余的斜线。

在运行路线处理器之前运行`server.use`处理器。

要使用它，我们可以写:

```
var restify = require('restify');function respond(req, res, next) {
  res.send(`hello ${req.params.name}`);
  next();
}var server = restify.createServer();
server.use(function(req, res, next) {
  console.warn('run for all routes!');
  return next();
});
server.get('/', function(req, res, next) {
  res.send('home')
  return next();
});server.post('/foo',
  function(req, res, next) {
    req.someData = 'foo';
    return next();
  },
  function(req, res, next) {
    res.send(req.someData);
    return next();
  }
);server.listen(8080, function() {
  console.log('%s listening at %s', server.name, server.url);
});
```

那么`use`处理器将在运行路线处理器之前运行。

# 结论

我们可以轻松地用 Restify 创建简单的后端应用程序。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**