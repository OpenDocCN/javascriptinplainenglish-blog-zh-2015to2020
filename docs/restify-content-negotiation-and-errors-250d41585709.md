# 重新定义—内容协商和错误

> 原文：<https://javascript.plainenglish.io/restify-content-negotiation-and-errors-250d41585709?source=collection_archive---------18----------------------->

![](img/4986e42a290adab9ec3992cec8b0bf5e.png)

Photo by [Will Francis](https://unsplash.com/@willfrancis?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Restify 是一个简单的节点后端框架。

在本文中，我们将研究如何使用 Restify 处理内容协商和错误。

# 内容协商和格式化

Restify 将按照从高到低的优先级来确定响应的`content-type`。

如果存在的话，它将使用`res.contentType`。

如果设置了`Content-Type`响应头。

如果 body 是一个对象而不是一个`Buffer`实例，将返回`application/json`。

否则，它通过将可用的格式化程序与请求的`accept`头匹配来协商`content-type`。

如果不能确定一个`content-type`，那么 Restify 将响应一个错误。

如果可以协商一个`content-type`，那么它将决定使用什么格式化程序来格式化响应的内容。

如果找不到与`content-type`匹配的格式化程序，那么 Restify 将覆盖响应的`content-type`到`'application/octet-stream'`，如果找不到该`content-type`的格式化程序，则出错。

创建 Restify 服务器实例时，可以通过传递`strictFormatters: false`属性来更改默认行为。

如果没有为`content-type`找到格式化程序，则刷新响应而不应用任何格式化程序。

用`application/json`、`text/plain`和`application/octet-stream`格式化器对船只进行重新格式化。

在创建服务器时，我们可以通过传递内容类型和解析器的散列来添加额外的格式化程序。

例如，我们可以写:

```
var restify = require('restify');
const util = require('util');function respond(req, res, next) {
  res.send('hello');
  next();
}var server = restify.createServer({
  formatters: {
    ['application/foo'](req, res, body) {
      if (body instanceof Error)
        return body.stack; if (Buffer.isBuffer(body))
        return body.toString('base64'); return util.inspect(body);
    }
  }
});server.get('/hello', respond);
server.head('/hello', respond);server.listen(8080);
```

我们有用于`application/foo`内容类型的`formatters`对象。

如果主体是一个缓冲区，那么我们返回一个 base64 字符串。

我们还可以在我们的格式化程序定义中添加一个`q-value`来改变格式化程序的优先级:

```
var restify = require('restify');
const util = require('util');
function respond(req, res, next) {
  res.send('hello');
  next();
}
var server = restify.createServer({
  formatters: {
    ['application/foo q=0.9'](req, res, body) {
      if (body instanceof Error)
        return body.stack;
      if (Buffer.isBuffer(body))
        return body.toString('base64');
      return util.inspect(body);
    }
  }
});
server.get('/hello', respond);
server.head('/hello', respond);
server.listen(8080);
```

使用默认格式化程序重新格式化。

当用`createServer`传递格式化程序选项时，它可以被覆盖:

```
var restify = require('restify');

function respond(req, res, next) {
  const body = 'hello world';
  res.writeHead(200, {
    'Content-Length': Buffer.byteLength(body),
    'Content-Type': 'text/plain'
  });
  res.write(body);
  res.end();    
}
var server = restify.createServer();
server.get('/hello', respond);
server.head('/hello', respond);
server.listen(8080);
```

我们用一个对象调用了`writeHead`方法，该对象带有我们希望在头中返回的`content-type`选项。

# 错误处理

我们可以通过用`next`函数传递一个错误对象来处理错误情况。

例如，我们可以写:

```
var restify = require('restify');
var errors = require('restify-errors');var server = restify.createServer();server.get('/hello/:foo', function(req, res, next) {
  var err = new errors.NotFoundError('not found');
  return next(err);
});server.on('NotFound', function (req, res, err, cb) {
  return cb();
});server.listen(8080);
```

我们有`NotFound`处理程序来记录日志或收集数据。

`NotFoundError`构造函数在`restify-errors`模块中。

我们不应该在`NotFound`处理程序中调用`res.send`，因为它在不同的错误上下文中。

# 结论

我们可以用同样的方式格式化我们的数据并进行内容协商。

同样，我们用`restify-errors`模块创建错误对象。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**