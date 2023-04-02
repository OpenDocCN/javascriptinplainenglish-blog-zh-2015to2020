# 使用 Cookie 解析器解析 Express 应用程序中请求的 Cookie

> 原文：<https://javascript.plainenglish.io/parse-cookies-in-requests-in-express-apps-with-cookie-parser-4a94af5af620?source=collection_archive---------2----------------------->

![](img/5b2457e44d3cd37d42ce64ac81297f92.png)

Photo by [Christina Branco](https://unsplash.com/@starvingartistfoodphotography?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

默认情况下，Express 4.x 或更高版本没有提供任何中间件来解析 cookies。

为此，我们可以添加`cookie-parser`中间件。

在本文中，我们将研究如何使用它来解析随请求发送的 cookies。

# 添加包

我们可以通过运行以下命令来安装该软件包:

```
npm install cookie-parser
```

然后我们可以写:

```
const express = require('express')
const cookieParser = require('cookie-parser')

const app = express()
app.use(cookieParser())
```

将中间件添加到我们的应用程序中。

# 方法

## cookieParser(机密，选项)

`cookie-parser`中间件的`cookieParser`函数将一个`secret`字符串或字符串数组作为第一个参数，将一个`options`对象作为第二个参数。

`secret`是可选的。如果没有指定，它将不会解析签名的 cookies。如果提供了一个字符串，它将被用作秘密。如果提供了一个字符串数组，那么每个秘密都将被尝试用于解码签名的 cookie

`options`是作为第二个选项传入`cookie.parse`的对象。

## cookieParser。JSONCookie(str)

`JSONCookie`解析一个 JSON cookie。如果它是一个 JSON cookie，那么它将返回缩减的 JSON 值。否则它将返回传递的值。

## cookieParser。JSON cookies(cookie)

该方法将遍历这些键，对每个值调用`JSONCookie` ，并用解析后的值替换原始值。这将返回传入的同一个对象。

## cookieParser.signedCookie(str，secret)

`signedCookie`将 cookie 解析为签名 cookie。如果它是一个签名的 cookie 并且签名有效，它将返回解析的无符号值。如果该值没有符号，则返回原始值。如果值是有符号的，但是 cookie 不能用`secret`验证，那么就返回`false`。

## cookieparser . signed cookies(cookie，secret)

该方法将遍历这些键，并检查是否有任何值是签名的 cookie。如果它已经签名并且签名有效，那么密钥将从对象中删除并添加到返回的新对象中。

`secret`可以是数组或字符串。如果它是一个字符串，那么它将检查字符串的秘密。否则，将使用数组中的每个字符串检查每个 cookie。

根据客户端发送的 cookie 类型，将自动调用`JSONCookie`、`JSONCookies`、`signedCookie` 和`signedCookies`方法。

![](img/7f04455fa28e93fd5e933b6ef88e50bb.png)

Photo by [Oriol Portell](https://unsplash.com/@oriol_portell?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 解析未签名的 Cookies

一个简单的例子是解析未签名的 cookies，如下所示:

```
const express = require('express');
const bodyParser = require('body-parser');
const cookieParser = require('cookie-parser');
const app = express();app.use(bodyParser.json());
app.use(cookieParser());app.get('/', (req, res) => {
  res.send(req.cookies);
});app.listen(3000);
```

然后，当我们发送一个 GET 请求，将`Cookie`头的值设置为`foo=bar`时，我们得到:

```
{
    "foo": "bar"
}
```

作为回应，因为`req.cookies`已经解析了 cookie。

# 解析签名的 Cookies

我们可以发送一个签名的 cookie 并解析它，如下所示:

```
const express = require('express');
const bodyParser = require('body-parser');
const cookieParser = require('cookie-parser');
const app = express();
const secret = 'secret';app.use(bodyParser.json());
app.use(cookieParser(secret));app.get('/cookie', (req, res) => {
  res
    .cookie('foo', 'bar', { signed: true })
    .send();
});app.get('/', (req, res) => {  
  res.send(req.signedCookies);
});app.listen(3000);
```

代码有一个`/cookie`端点将签名的 cookie 发送给客户机。

```
{ signed: true }
```

会让快递在饼干上签名。

然后从客户端，当我们向`/`发出请求时，我们通过`req.signedCookies`得到签名的 cookie，因此我们得到:

```
{
    "foo": "bar"
}
```

作为`/`的回应。

# 结论

我们可以使用`cookie-parser`中间件来解析 cookies。

它可以解析已签名和未签名的 cookies。为了解析一个签名的 cookie，我们只需要用一个秘密来签名我们的 cookie，然后如果我们把这个秘密传递给`cookieParser`中间件函数，`cookie-parser`就可以解码这个 cookie。

根据客户端发送的 cookie 类型，将自动调用`JSONCookie`、`JSONCookies`、`signedCookie` 和`signedCookies`方法。