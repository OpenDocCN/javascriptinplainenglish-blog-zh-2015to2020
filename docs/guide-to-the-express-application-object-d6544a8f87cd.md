# 如何在 Express.js 中使用 app 对象

> 原文：<https://javascript.plainenglish.io/guide-to-the-express-application-object-d6544a8f87cd?source=collection_archive---------3----------------------->

![](img/e4d41e588e9206ce3ebcef2c193de8b2.png)

Photo by [bruce mars](https://unsplash.com/@brucemars?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

快速应用程序的核心部分是应用程序对象。是应用本身。

在本文中，我们将看看`app`对象的属性和方法，以及我们可以用它做什么。

# 它能做什么？

`app`对象有以下方法:

*   路由 HTTP 请求
*   配置中间件
*   呈现 HTML 视图
*   注册模板引擎

# 性能

## app.locals

`app.locals`对象的属性是应用程序中的局部变量。

我们可以将它用作 getter 或 setter。

例如，我们可以写:

```
console.dir(app.locals.title);
```

这些属性在整个应用程序中都是持久的，而不仅仅是请求的生命周期。

我们还可以根据需要设置属性:

```
app.locals.title = 'foo';
```

## app.mountpath

`app.mountpath`属性有一个或多个安装了子应用程序的路径模式。

子应用程序是`express`的一个实例，用于处理路由请求。

例如，我们可以如下使用它:

```
const express = require('express');
const bodyParser = require('body-parser');const app = express();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));
const foo = express();foo.get('/', (req, res) => {
  res.send(foo.mountpath);
})app.use('/foo', foo);app.listen(3000, () => console.log('server started'));
```

那么我们应该得到`/foo`，因为我们有:

```
app.use('/foo', foo);
```

定义子应用路径`/foo`。

像常规路径一样，子应用程序的路径也可以有通配符或特殊字符来匹配模式。

例如，如果我们有:

```
const express = require('express');
const bodyParser = require('body-parser');const app = express();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));
const foo = express();foo.get('/', (req, res) => {
  res.send(foo.mountpath);
})app.use('/ab*d', foo);app.listen(3000, () => console.log('server started'));
```

然后当我们转到`/abcd`时，我们会看到`/ab*d`，因为在`b`和`d`之间有通配符`*`。

![](img/e24dc8daea5fd19239f625d291fa4e5a.png)

Photo by [CoinView App](https://unsplash.com/@coinviewapp?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 事件

## app.on('mount '，回调(父))

当子应用程序装载到父应用程序上时，在子应用程序上触发`mount`事件。父应用程序被传递给回调。

子应用程序不会继承有默认值的值，也不会继承没有默认值的设置值。

例如，我们可以如下使用它:

```
const express = require('express');
const bodyParser = require('body-parser');const app = express();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));
const foo = express();foo.on('mount', (parent) => {
  console.log(parent);
})foo.get('/', (req, res) => {
  res.send('foo');
})app.use('/foo', foo);app.listen(3000, () => console.log('server started'));
```

我们应该看到父应用程序对象被记录。

# 方法

## app.all(路径，回调[，回调…])

这个应用程序允许我们传递路由处理程序来处理所有的请求方法。

它采用以下参数:

*   `path` —它可以是表示路径或路径模式的字符串或正则表达式。默认为`/`。
*   `callback` —处理请求的功能。它可以是一个中间件功能、一系列中间件功能、一系列中间件功能或以上所有功能的组合

例如，我们可以如下使用它:

```
const express = require('express');
const bodyParser = require('body-parser');const app = express();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));app.all('/', function (req, res, next) {
  res.send('hi');
})app.listen(3000, () => console.log('server started'));
```

那么无论我们对`/`提出什么样的要求，都要看到`hi`。

对于像检查身份验证或加载用户数据这样的全局逻辑来说，这很方便。

## app.delete(路径，回调[，回调…])

这个应用程序允许我们传递路由处理程序来处理删除请求。

它采用以下参数:

*   `path` —可以是表示路径或路径模式的字符串或正则表达式。默认为`/`。
*   `callback` —处理请求的功能。它可以是一个中间件功能、一系列中间件功能、一系列中间件功能或以上所有功能的组合

例如，如果我们有:

```
const express = require('express');
const bodyParser = require('body-parser');const app = express();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));app.delete('/', function (req, res, next) {
  res.send('DELETE request made');
})app.listen(3000, () => console.log('server started'));
```

那么当我们用 Postman 这样的 HTTP 客户端发出删除请求时，应该会看到`DELETE request made`。

## app.disable(名称)

我们可以使用`disable`方法来设置给定的`name`到`false.`的设置，这与调用`app.set('setting', false)`是一样的；

例如，如果我们有:

```
const express = require('express');
const bodyParser = require('body-parser');const app = express();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));app.disable('foo');app.get('/', function (req, res, next) {
  res.json({ foo: app.get('foo') });
})app.listen(3000, () => console.log('server started'));
```

那么我们应该看到:

```
{"foo":false}
```

因为我们叫`app.disable(‘foo’);`

# 结论

`app`对象是 Express app 的核心对象。它有让我们添加中间件和路由处理器的方法。

当子应用程序装载到父应用程序上时，它还允许我们处理`mount`事件。

我们还可以从`app`对象中获取子应用的`mountpath`。