# 快速请求对象指南—查询和 Cookies

> 原文：<https://javascript.plainenglish.io/guide-to-the-express-request-object-queries-and-cookies-c2224592ca7f?source=collection_archive---------4----------------------->

![](img/0757b4caf2cbebd4872fc8679c9c6115.png)

Photo by [nikhil laddha](https://unsplash.com/@laddhanikh?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

request 对象让我们在中间件和路由处理程序中获得来自客户端的请求信息。

在本文中，我们将详细研究 Express 的请求对象的属性，包括查询字符串、URL 参数和签名的 cookies。

# 请求对象

我们在上面的路由处理程序中的`req`参数是`req`对象。

它有一些属性，我们可以使用这些属性来获取客户端发出的请求的相关数据。下面列出了比较重要的几个。

## 请求.原始 Url

`originalUrl`属性有原始的请求 URL。

例如，我们可以如下使用它:

```
const express = require('express');
const bodyParser = require('body-parser');
const app = express();app.get('/', (req, res) => {
  res.send(req.originalUrl);
})app.listen(3000);
```

然后，当我们得到带有查询字符串`/?foo=bar`的请求时，我们得到`/?foo=bar`。

## 请求参数

`params`属性有来自 URL 的请求参数。

例如，如果我们有:

```
const express = require('express')
const app = express()app.use(express.json())
app.use(express.urlencoded({ extended: true }))app.get('/:name/:age', (req, res) => {
  res.json(req.params)
})app.listen(3000, () => console.log('server started'));
```

然后，当我们将`/john/1`作为 URL 的参数部分传入时，我们得到:

```
{
    "name": "john",
    "age": "1"
}
```

作为上面路线的回应。

## req.path

属性包含请求 URL 的路径部分。

例如，如果我们有:

```
const express = require('express');
const bodyParser = require('body-parser');
const app = express();app.get('/:name/:age', (req, res) => {
  res.send(req.path);
})app.listen(3000);
```

并向`/foo/1`发出请求，然后我们在响应中得到同样的东西。

## 请求协议

我们可以用`protocol`属性获得协议字符串。

例如，如果我们有:

```
const express = require('express');
const bodyParser = require('body-parser');
const app = express();app.get('/', (req, res) => {
  res.send(req.protocol);
})app.listen(3000);
```

然后，当我们通过 HTTP 发出请求时，我们返回`http`。

## req.query

`query`属性从解析成对象的请求 URL 中获取查询字符串。

例如，如果我们有:

```
const express = require('express')
const app = express()app.use(express.json())
app.use(express.urlencoded({ extended: true }))app.get('/', (req, res) => {
  res.json(req.query)
})app.listen(3000, () => console.log('server started'));
```

然后，当我们将`?name=john&age=1`附加到主机名的末尾时，我们得到:

```
{
    "name": "john",
    "age": "1"
}
```

从回应来看。

![](img/bccd747d6d8daab68cd252ed72fafa21.png)

Photo by [Anna Demianenko](https://unsplash.com/@annademy?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 请求路由

我们用`route`属性得到当前匹配的路线。

例如，我们可以如下使用它:

```
const express = require('express');
const bodyParser = require('body-parser');
const app = express();app.get('/', (req, res) => {
  console.log(req.route);
  res.json(req.route);
})app.listen(3000);
```

然后我们从`console.log`中得到类似下面的东西:

```
Route {
  path: '/',
  stack:
   [ Layer {
       handle: [Function],
       name: '<anonymous>',
       params: undefined,
       path: undefined,
       keys: [],
       regexp: /^\/?$/i,
       method: 'get' } ],
  methods: { get: true } }
```

## 请求安全

一个布尔属性，指示请求是否通过 HTTPS 发出。

例如，假设我们有:

```
const express = require('express');
const bodyParser = require('body-parser');
const app = express();app.get('/', (req, res) => {  
  res.json(req.secure);
})app.listen(3000);
```

如果请求是通过 HTTP 发出的，那么我们得到`false`。

## req.signedCookies

`signedCookies`对象有一个 cookie，它的值是从散列值中解密出来的。

例如，我们可以如下使用它:

```
const express = require('express');
const bodyParser = require('body-parser');
const cookieParser = require('cookie-parser');
const app = express();
app.use(cookieParser('secret'));app.get('/cookie', (req, res, next) => {
  res.cookie('name', 'foo', { signed: true })
  res.send();
})app.get('/', (req, res) => {
  res.send(req.signedCookies.name);
})app.listen(3000);
```

在上面的代码中，我们使用了`/cookie`路径将 cookie 从我们的应用程序发送到客户端。然后，我们可以从客户端获取值，然后向`/`路由发出请求，请求的标题是`Cookie`，关键字是`name`，值是来自`/cookie`路由的签名值。

例如，它看起来像这样:

```
name=s%3Afoo.dzukRpPHVT1u4g9h6l0nV6mk9KRNKEGuTpW1LkzWLbQ
```

然后，在签名的 cookie 被解码后，我们从`/`路径返回`foo`。

## req.stale

`stale`是`fresh`的反义词。更多详情见`[req.fresh](https://expressjs.com/en/4x/api.html#req.fresh)`。

## 请求子域名

我们可以在带有`subdomains`属性的请求的域名中获得一个子域数组。

例如，我们可以如下使用它:

```
const express = require('express');
const bodyParser = require('body-parser');
const app = express();app.get('/', (req, res) => {
  res.send(req.subdomains);
})app.listen(3000);
```

然后，当我们向`/`路由发出请求时，我们会得到类似这样的结果:

```
["beautifulanxiousflatassembler--five-nine"]
```

## req.xhr

`true`为`X-Requested-With`的布尔属性请求头字段为`XMLHttpRequest`。

例如，我们可以如下使用它:

```
const express = require('express');
const bodyParser = require('body-parser');
const app = express();app.get('/', (req, res) => {
  res.send(req.xhr);
})app.listen(3000);
```

然后，当我们在`X-Requested-With`报头设置为`XMLHttpRequest`的情况下向`/`路由发出请求时，我们返回`true`。

# 结论

请求对象中有许多属性可以获取许多东西。

我们可以用 Cookie 解析器中间件获得签名的 Cookie。查询字符串和 URL 参数可以通过`query`、`params`、`route`属性以各种形式访问。

同样，我们可以用`subdomains`属性获取子域的一部分。