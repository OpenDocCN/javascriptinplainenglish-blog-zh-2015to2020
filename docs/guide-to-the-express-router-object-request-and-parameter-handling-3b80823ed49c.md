# 快速路由器对象指南—请求和参数处理

> 原文：<https://javascript.plainenglish.io/guide-to-the-express-router-object-request-and-parameter-handling-3b80823ed49c?source=collection_archive---------5----------------------->

![](img/5d85faf5bc80fcb800352e591145646b.png)

Photo by [Jon Flobrant](https://unsplash.com/@jonflobrant?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Express `router`对象是中间件和路线的集合。它是主应用程序中的一个迷你应用程序。

它只能执行中间件和路由功能，不能独立运行。

它本身也像中间件一样，所以我们可以将它与`app.use`一起使用，或者作为另一个路由的`use`方法的参数。

在本文中，我们将看看`router`对象的方法，包括`all`、`param`和监听特定类型请求的方法。

# 方法

## router.all(路径，[回调，...]回调)

`router.all`方法使用回调来处理各种请求。

我们可以传入一个常量路径，或者一个带有路径模式的字符串或者一个正则表达式。

例如，我们可以传递为所有连接到`router`的路由运行的中间件，如下所示:

```
const express = require('express');
const bodyParser = require('body-parser');const app = express();
const fooRouter = express.Router();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));const mw1 = (req, res, next) => {
  console.log('middleware 1 called');
  next();
}const mw2 = (req, res, next) => {
  console.log('middleware 2 called');
  next();
}fooRouter.all('*', mw1, mw2);fooRouter.get('/', (req, res) => {
  res.send('foo');
})app.use('/foo', fooRouter);app.listen(3000, () => console.log('server started'));
```

然后我们得到:

```
middleware 1 called
middleware 2 called
```

如果我们向`/foo`发出一个请求，因为任何以`/foo`开头的内容都会通过`fooRouter`发送，并且我们有一个带有传入中间件的`fooRouter.all`方法调用。

等价地，我们可以写:

```
const express = require('express');
const bodyParser = require('body-parser');const app = express();
const fooRouter = express.Router();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));const mw1 = (req, res, next) => {
  console.log('middleware 1 called');
  next();
}const mw2 = (req, res, next) => {
  console.log('middleware 2 called');
  next();
}fooRouter.all('*', mw1);
fooRouter.all('*', mw2);fooRouter.get('/', (req, res) => {
  res.send('foo');
})app.use('/foo', fooRouter);app.listen(3000, () => console.log('server started'));
```

只要调用`fooRouter.all`的顺序与回调的传入顺序相同，它们就是相同的。

![](img/74eb6085aa8f207f089281f4b420b573.png)

Photo by [Sebastien Gabriel](https://unsplash.com/@sgabriel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 路由器。方法(路径，[回调，...]回调)

`router.METHOD`是用给定的方法处理请求。例如，`router.get`用于处理 GET 请求，`router.post`用于处理 POST 请求等。

如果没有调用`router.head`，除了 GET 方法之外`router.get`还会自动调用`HTTP HEAD`。

我们可以提供多次回访，并且一视同仁。这些回调可以调用`next('route')`调用来绕过剩余的路由回调。

例如，我们可以如下使用它:

```
const express = require('express');
const bodyParser = require('body-parser');const app = express();
const fooRouter = express.Router();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));fooRouter.get('/', (req, res) => {
  res.send('foo');
})app.use('/foo', fooRouter);app.listen(3000, () => console.log('server started'));
```

然后当我们向`/foo`发出请求时，我们得到`foo`。

我们还可以为路线`path`传入一个正则表达式。例如，我们可以写:

```
const express = require('express');
const bodyParser = require('body-parser');const app = express();
const fooRouter = express.Router();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));fooRouter.get('/ab+c/', (req, res) => {
  res.send('foo');
})app.use('/foo', fooRouter);app.listen(3000, () => console.log('server started'));
```

监听对路径`/foo/abc`、`/foo/abbc`、`/foo/abbbc`等的请求。，因为我们在正则表达式中指定了在路径中查找任意数量的字符`b`。

## router.param(名称，回调)

`router.param`让我们在客户端发出请求并传入特定参数时触发`callback`函数调用。

`name`是我们寻找的参数占位符名称。

`callback`功能的参数有:

*   `req`，请求对象。
*   `res`，应答对象。
*   `next`，表示下一个中间件功能。
*   `name`参数的值。
*   参数的名称。

例如，我们可以如下使用它:

```
const express = require('express');
const bodyParser = require('body-parser');const app = express();
const fooRouter = express.Router();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));fooRouter.param('name', (req, res, next, name) => {
  req.name = name;
  next();
})fooRouter.get('/:name', (req, res) => {
  res.send(req.name);
})app.use('/foo', fooRouter);app.listen(3000, () => console.log('server started'));
```

然后我们向`/foo/abc`发出请求，然后我们得到`abc`，因为`fooRouter.param`发现`name`参数是通过 URL 传入的。

`name`参数的值为`'abc'`，因为它抓取了`/foo/`之后的部分，然后我们将`name`赋给`req.name`并调用`next`。

之后，调用我们传递给`foorRouter.get`的路由处理程序，然后我们将`req.name`传递给`res.send`并将其作为响应发送。

# 结论

Express `router`让我们可以创建一个 Express 应用程序的子应用程序，这样我们就不必在主应用程序中添加所有的路线处理器和中间件。

用这种方法，我们可以倾听各种各样的要求。我们还可以用相应的方法监听特定类型的请求，比如 GET 或 POST 请求。它们都接受一个字符串或正则表达式路径和一个路由处理程序回调。

最后，我们有`param`方法来获取路线参数，并使用它做我们想做的事情。