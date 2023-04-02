# 快速请求对象指南—正文、Cookies 等

> 原文：<https://javascript.plainenglish.io/guide-to-the-express-request-object-body-cookies-and-more-f0cbdb834c8c?source=collection_archive---------2----------------------->

![](img/0ed79dada7f538c187a3896a902e4b13.png)

Photo by [Nick Karvounis](https://unsplash.com/@nickkarvounis?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

request 对象让我们在中间件和路由处理程序中获得来自客户端的请求信息。

在本文中，我们将详细研究 Express 的请求对象的属性，包括获取请求体和 cookies。

# 请求对象

我们在上面的路由处理程序中的`req`参数是`req`对象。

它有一些属性，我们可以使用这些属性来获取客户端发出的请求的相关数据。下面列出了比较重要的几个。

## 请求应用程序

`req.app`属性保存了对使用中间件的 Express 应用实例的引用。

例如，我们可以如下使用它:

```
const express = require('express');
const bodyParser = require('body-parser');
const path = require('path');
const app = express();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));app.set('foo', 'bar');app.get('/', (req, res) => {
  res.send(req.app.get('foo'));
})app.listen(3000);
```

我们运行`app.set(‘foo’, ‘bar’);`将`foo`设置为值`'bar'`，然后我们可以用`req.app`的`get`方法获得该值。

## req.baseUrl

`req.baseUrl`属性保存已挂载的路由器实例的基本 URL。

例如，如果我们有:

```
const express = require('express');const app = express();
const greet = express.Router();
greet.get('/', (req, res) => {
  console.log(req.baseUrl);
  res.send('Hello World');
})app.use('/greet', greet);app.listen(3000, () => console.log('server started'));
```

然后我们从`console.log`得到`/greet`。

## 请求体

`req.body`有请求体。我们可以用`express.json()`解析 JSON 主体，用`express.urlencoded()`解析 URL 编码的请求。

例如，如果我们有:

```
const express = require('express')
const app = express()app.use(express.json())
app.use(express.urlencoded({ extended: true }))app.post('/', (req, res) => {
  res.json(req.body)
})app.listen(3000, () => console.log('server started'));
```

然后，当我们用 JSON 主体发出 POST 请求时，我们得到的是我们在请求中发送的内容。

## req.cookies

我们可以获得带有`req.cookies`属性的请求所发送的 cookies。

例如，我们可以如下使用它:

```
const express = require('express');
const bodyParser = require('body-parser');
const cookieParser = require('cookie-parser');
const app = express();
app.use(cookieParser());app.get('/', (req, res) => {
  res.send(req.cookies.name);
})app.listen(3000);
```

然后，当我们像发送`name=foo`一样发送以`name`为关键字的`Cookie`请求头时，就会显示`foo`。

## 请求新鲜

`fresh`属性表示该应用程序是“新鲜的”。如果`cache-control`请求头没有`no-cache`指令，且以下任何一项为`true`，则 App app 为“新的”:

*   `if-modified-since`请求头被指定，`last-modified`请求头等于或早于`modified`请求头。
*   `if-none-match`请求头是`*`
*   `if-none-match`请求头在被解析成指令后，与`etag`响应头不匹配。

我们可以如下使用它:

```
const express = require('express');
const bodyParser = require('body-parser');
const path = require('path');
const app = express();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));app.get('/', (req, res) => {
  res.send(req.fresh);
})app.listen(3000);
```

如果这些条件不满足，我们就应该得到`false`。

![](img/18a1ae4939008c069302a17baa25fcf0.png)

Photo by [Aniket Bhattacharya](https://unsplash.com/@aniket940518?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 请求主机名

我们可以用`req.hostname`从 HTTP 头中获取主机名。

当信任代理设置未评估为`false`时，Express 将从`X-Forwarded-Host`报头字段获取值。报头可以由客户端或代理设置。

如果有多个`X-Forwarded-Host`表头，将使用第一个。

例如，如果我们有:

```
const express = require('express')
const app = express()app.use(express.json())
app.use(express.urlencoded({ extended: true }))app.get('/', (req, res) => {
  res.json(req.hostname)
})app.listen(3000, () => console.log('server started'));
```

然后，如果没有`X-Forwarded-Host`头，并且信任代理的评估结果不是`false`，我们就可以得到应用所在的域名。

## 请求. ip

我们可以用这个属性获取发出请求的 IP 地址。

例如，我们可以如下使用它:

```
const express = require('express');
const bodyParser = require('body-parser');
const cookieParser = require('cookie-parser');
const app = express();
app.use(cookieParser());app.get('/', (req, res) => {
  res.send(req.ip);
})app.listen(3000);
```

然后我们得到类似`::ffff:172.18.0.1`的显示。

## 请求. ips

当`trust proxy`设置不是`false`时，该属性包含由`X-Forwarded-For`请求头指定的 IP 地址数组。

否则，它就是一个空数组。`X-Forwarded-For`可以由客户端或代理设置。

例如，我们可以如下使用它:

```
const express = require('express');
const bodyParser = require('body-parser');
const app = express();
app.set('trust proxy', true);app.get('/', (req, res) => {
  res.send(req.ips);
})app.listen(3000);
```

然后我们从客户端或`X-Forwarded-For`头获取远程 IPs。

## 请求方法

`method`属性具有请求的请求方法，如 GET、POST、PUT 或 DELETE。

# 结论

request 对象有许多属性，用于获取有关接收到的 HTTP 请求的各种信息。

像`cookie-parser`这样的第三方中间件向请求对象添加了新的属性，以获取 cookies 数据等内容。