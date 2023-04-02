# Restify-使用 Restify-Errors 进行错误处理

> 原文：<https://javascript.plainenglish.io/restify-error-handling-with-restify-errors-ce3770526802?source=collection_archive---------6----------------------->

![](img/25641245f625560f701a4fb219c2ca7c.png)

Photo by [Michael Geiger](https://unsplash.com/@jackson_893?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Restify 是一个简单的节点后端框架。

在本文中，我们将看看如何用 Restify 格式化错误消息。

# 格式错误

我们可以用错误处理程序格式化错误。

例如，我们可以写:

```
var restify = require('restify');
var errors = require('restify-errors');var server = restify.createServer();server.get('/hello/:foo', function(req, res, next) {
  var err = new errors.InternalServerError('not found');
  return next(err);
});server.on('InternalServer', function (req, res, err, cb) {
  err.toString = function() {
    return 'an internal server error occurred!';
  }; err.toJSON = function() {
    return {
      message: 'an internal server error occurred!',
      code: 'error'
    }
  }; return cb();
});server.on('restifyError', function (req, res, err, cb) {
  return cb();
});server.listen(8080);
```

我们有带`toString`和`toJSON`方法的`InternalServer`处理程序来格式化字符串和 JSON 错误。

`restifyError`错误处理器有错误处理器。

此外，我们可以为其他内容类型创建格式化程序。

例如，我们可以写:

```
var restify = require('restify');
var errors = require('restify-errors');const server = restify.createServer({
  formatters: {
    ['text/html'](req, res, body) {
      if (body instanceof Error) {        
        return `<html><body>${body.message}</body></html>`;
      }
    }
  }
});server.get('/', function(req, res, next) {
  res.header('content-type', 'text/html');
  return next(new errors.InternalServerError('error!'));
});server.listen(8080);
```

为`text/html`内容类型创建一个内容格式化程序。

# 重新定义-错误

`restify-errors`模块为许多常见的 HTTP 请求和 REST 相关错误公开了一套错误压缩程序。

例如，我们可以通过编写以下内容来创建错误:

```
var restify = require('restify');
var errors = require('restify-errors');const server = restify.createServer();server.get('/', function(req, res, next) {
  return next(new errors.ConflictError("conflict"));
});server.listen(8080);
```

然后当我们转到`http://localhost:8080`时，我们得到:

```
{"code":"Conflict","message":"conflict"}
```

我们也可以将 Restify 错误作为参数`res.send`传入。

例如，我们可以写:

```
var restify = require('restify');
var errors = require('restify-errors');const server = restify.createServer();server.get('/', function(req, res, next) {
  res.send(new errors.GoneError('gone'));
  return next();
});server.listen(8080);
```

我们将`GoneError`实例传递给`res.send`方法。

然后当我们转到`http://localhost:8080`时，我们得到:

```
{"code":"Gone","message":"gone girl"}
```

JSON 的自动序列化是通过调用`Error`对象上的`JSON.stringify`来完成的。

它们都定义了`toJSON`方法。

如果对象没有定义`toJSON`方法，那么我们将得到一个空对象。

## HttpError

`HttpError`是一个普通的 HTTP 错误，我们可以用`statusCode`和`body`属性返回。

`statusCode`将自动设置 HTTP 状态码，`body`默认设置为消息。

400 错误 500 之间的所有状态代码被自动转换成一个`HttpError`，名称为 PascalCase 格式，空格被删除。

## RestError

Restify 为我们提供了带有`code`和`message`属性的内置错误/

例如，如果我们有:

```
var restify = require('restify');
var errors = require('restify-errors');const server = restify.createServer();server.get('/', function(req, res, next) {
  return next(new errors.InvalidArgumentError("error"));
});server.listen(8080);
```

使用`restify-errors`可以创建任何 400 或 500 系列错误。

# 结论

我们可以用`restify-errors`创建错误对象。

如果我们在响应中使用 JSON，它们会自动序列化成 JSON。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**