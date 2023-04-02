# Restify 版本化的路由和升级请求

> 原文：<https://javascript.plainenglish.io/restify-versioned-routes-and-upgrade-requests-458865add47?source=collection_archive---------16----------------------->

![](img/cd599108a99de9709c9ae150276168f3.png)

Photo by [Angelina Kichukova](https://unsplash.com/@anynieel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Restify 是一个简单的节点后端框架。

在本文中，我们将了解如何使用 Restify 添加路由。

# 版本化路由

Restify 路由可以进行版本控制。

版本号应该遵循语义版本控制。

例如，我们可以写:

```
var restify = require('restify');var server = restify.createServer();function sendV1(req, res, next) {
  res.send(`hello: ${req.params.name}`);
  return next();
}function sendV2(req, res, next) {
  res.send({ hello: req.params.name });
  return next();
}server.get('/hello/:name', restify.plugins.conditionalHandler([
  { version: '1.1.3', handler: sendV1 },
  { version: '2.0.0', handler: sendV2 }
]));server.listen(8080);
```

拥有不同版本的路由。

然后我们可以用版本号作为值的`accept-version`头进行请求。

如果没有提供这个头，那么最新版本的处理程序将运行。

例如，如果我们运行:

```
curl -s localhost:8080/hello/foo
```

我们得到:

```
{
    "hello": "foo"
}
```

如果我们逃跑:

```
curl -s -H 'accept-version: ~1' localhost:8080/hello/foo
```

我们得到:

```
"hello: foo"
```

如果我们逃跑:

```
curl -s -H 'accept-version: ~2' localhost:8080/hello/foo
```

我们得到:

```
{
    "hello": "foo"
}
```

如果我们的标题中有无效的版本号，比如:

```
curl -s -H 'accept-version: ~3' localhost:8080/hello/foo
```

我们得到:

```
{
    "code": "InvalidVersion",
    "message": "~3 is not supported by GET /hello/foo"
}
```

通过传入一个数组，我们可以对多个版本使用相同的路由处理程序:

```
var restify = require('restify');var server = restify.createServer();function sendV1(req, res, next) {
  res.send(`hello: ${req.params.name}`);
  return next();
}function sendV2(req, res, next) {
  res.send({ hello: req.params.name });
  return next();
}server.get('/hello/:name', restify.plugins.conditionalHandler([
  { version: '1.1.3', handler: sendV1 },
  { version: ['2.0.0', '2.1.0', '2.2.0'], handler: sendV2 }
]));server.listen(8080);
```

同样，我们也可以通过`req.version`方法得到最初请求的版本。

我们可以得到 Restify 与`matchedVersion`方法相匹配的版本。

例如，我们可以写:

```
var restify = require('restify');var server = restify.createServer();server.get('/hello/:name', restify.plugins.conditionalHandler([
 {
    version: ['2.0.0', '2.1.0', '2.2.0'],
    handler: function (req, res, next) {
      res.send(200, {
        requestedVersion: req.version(),
        matchedVersion: req.matchedVersion()
      });
      return next();
    }
  }
]));server.listen(8080);
```

如果我们跑了:

```
curl -s -H 'accept-version: ~2' localhost:8080/hello/foo
```

我们得到:

```
{
    "requestedVersion": "~2",
    "matchedVersion": "2.2.0"
}
```

# 升级请求

如果传入的 HTTP 请求有`Connection: Upgrade`头，那么节点服务器会以不同的方式处理它。

默认情况下，它不会被推送到 Restify。

我们可以通过`handleUpgrades`方案实现这一目标。

例如，我们可以写:

```
var restify = require('restify');
var Watershed = require('Watershed');var server = restify.createServer();var ws = new Watershed();
server.get('/websocket/attach', function(req, res, next) {
  if (!res.claimUpgrade) {
    next(new Error('Connection Must Upgrade For WebSockets'));
    return;
  }var upgrade = res.claimUpgrade();
  var shed = ws.accept(req, upgrade.socket, upgrade.head);
  shed.on('text', function(msg) {
    console.log('Received message from websocket client: ' + msg);
  });
  shed.send('hello there!'); next(false);
});server.listen(8080);
```

我们创建`Watershed`实例来监听网络套接字连接。

# 结论

我们可以用 Restify 版本化我们的路线。

此外，我们可以使用`Watershed`来处理网站请求。

喜欢这篇文章吗？如果是这样的话，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获得更多类似的内容吧！**