# Node.js 最佳实践—头盔和 Cookie

> 原文：<https://javascript.plainenglish.io/node-js-best-practices-helmet-and-cookie-2db298854d00?source=collection_archive---------4----------------------->

![](img/e0bce6ea4011164d1230f8fed8c8330d.png)

Photo by [Dwidiyo Hanung](https://unsplash.com/@dwidiyohanung?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# 添加头盔以设置 Sane 默认值

快速应用程序的某些默认设置不太安全。

因此，头盔中间件可用于设置一些更合理的默认设置。

为了使用它，我们写:

```
const express = require('express');
const helmet = require('helmet');const app = express();app.use(helmet());
```

当我们创建快速应用程序时。

它做了几件事来提高我们的应用程序的安全性。

它启用了内容安全策略 HTTP 头。

这定义了允许在我们的网页上加载的内容(如脚本、图像等)的可信来源。

DNS 预取有助于加快加载速度。

但是，禁用预取将限制所使用的外部服务类型的潜在数据泄漏。

它还可以减少与 DNS 查询查找相关的流量和成本。

X-Frame-Options HTTP 头也被启用。

这通过禁用在另一个站点上呈现的网页的选项来阻止点击劫持尝试。

X-Powered-By HTTP 头也是隐藏的。

这样，攻击者就无法识别我们用什么来创建我们的应用程序。

还启用了公钥固定标头。

这可以防止使用伪造证书的中间人攻击。

还启用了严格传输安全标头。

一旦客户端最初与 HTTPS 连接，这将强制后续的服务器连接使用 HTTPS。

它还使用默认值启用 Cache-Control、Pragma、Expires 和 Surrogate-Control，阻止客户端缓存旧版本的站点资源。

X-Content-Type-Options HTTP 头阻止客户端嗅探声明的内容类型之外的响应的 MIME 类型。

我们的应用程序的响应头中引用的 HTTP 头也可以被控制来包含某些信息。

防止浏览器中某些 XSS 攻击的 X-XSS 保护 HTTP 头。

# 收紧会话 Cookies

我们应该加强安全性不高的会话 cookies。

我们可以用`express-session`包设置各种设置。

`secret`属性是用来加盐的 cookie 的秘密字符串。

`key`是饼干的名字。

`httpOnly`将 cookies 标记为只能由发布 web 服务器访问。

`secure`应该设置为`true`，需要 SSL/TLS。

这将强制 cookies 仅用于 HTTPS 请求。

`domain`表示可以访问 cookie 的域。

`path`具有 cookie 在应用程序域内被接受的路径。

`expires`已经设置了 cookie 的失效日期。

默认情况下，这将持续一个会话。

要使用这些选项，我们可以编写:

```
const express = require('express');
const session = require('express-session');const app = express();app.use(session({  
  secret: 'secret',
  key: 'someKey', 
  cookie: {
    httpOnly: true,
    secure: true,
    domain: 'example.com',
    path: '/foo/bar',
    expires: new Date( Date.now() + 60 * 60 * 1000 )
  }
}));
```

我们可以设置所有这些选项来创建和发送我们的 cookie。

![](img/6ac4eee4c30ff44f0bec4c871ee0254a.png)

Photo by [andrew welch](https://unsplash.com/@andrewwelch3?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

快速头盔和快速会话包对于保护我们的快速应用程序非常有用。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**