# 使用 Koa 获取请求数据

> 原文：<https://javascript.plainenglish.io/getting-request-data-with-koa-6edcd65f8471?source=collection_archive---------6----------------------->

![](img/c2da71c04ff5b4b4ea3ef9decdfd4a6b.png)

Photo by [engin akyurt](https://unsplash.com/@enginakyurt?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Koa 是一个小框架，让我们可以创建在 Node.js 平台上运行的后端应用程序。

在本文中，我们将研究如何用 Koa 接收请求数据。

# 获取请求的内容类型标头

我们可以使用`ctx.request.type`属性来获取请求的`Content-Type`头。

例如，我们可以编写以下代码在屏幕上显示该值:

```
const Koa = require('koa');
const app = new Koa();app.use(async (ctx, next) => {
  ctx.body = ctx.request.type;
});app.listen(3000);
```

然后，如果我们发送带有设置为`application/json`的`Content-Type`头值的请求，那么当我们得到响应时，我们将在屏幕上显示它。

# 获取请求的字符集

`ctx.request.charset`属性具有请求字符集。

# 从请求 URL 获取查询字符串

`ctx.request.query`属性将查询字符串解析成一个键值对。

查询字符串的每个查询参数都被解析为一个对象的键值对。

例如，我们可以如下使用它:

```
const Koa = require('koa');
const app = new Koa();app.use(async (ctx, next) => {
  ctx.body = ctx.request.query;
});app.listen(3000);
```

在上面的代码中，我们只是将`query`属性设置为`ctx.body`的值。

那么当我们向[https://craftygrouchyccables-five-nine.repl.co 发出请求时？a=1 & b=2，](https://CraftyGrouchyCables--five-nine.repl.co?a=1&b=2,)我们得到如下响应:

```
{
    "a": "1",
    "b": "2"
}
```

正如我们所看到的，查询字符串中的键值对被解析为对象的键值对，该对象被设置为`ctx.request.query`的值，而我们自己不做任何事情。

`ctx.request.query`属性也是一个 setter，所以我们可以把它设置成别的。但是，它不支持嵌套对象。

# 检查请求缓存是否是新的，或者其内容是否已经更改

我们可以通过使用`ctx.fresh`属性来检查请求缓存的新鲜度。

例如，我们可以如下使用它:

```
const Koa = require('koa');
const app = new Koa();app.use(async (ctx, next) => {
  ctx.status = 200;
  ctx.set('ETag', 'foo');
  if (ctx.fresh) {
    ctx.status = 304;
    console.log('fresh');
    return;
  }
  ctx.body = 'foo';
});app.listen(3000);
```

在上面的代码中，我们通过使用`ctx.fresg`属性来检查请求缓存是否是新的。如果它是新的，那么我们将看到记录的`fresh`字符串。

否则，我们不会看到它被记录。如果我们向我们的应用程序发出多个请求，那么我们不会看到它被记录，因为缓存已经填充了没有改变的数据，我们总是返回相同的响应。

还有一个`ctx.stale`属性检查`ctx.fresh`的对方状态是`true`。

# 获取我们请求的协议

我们可以使用`ctx.request.protocol`属性来获取请求协议。不是`https`就是`http`。

如果`app.proxy`设置为`true`，它还可以检查协议的`X-Forwarded-Proto`请求报头。

# 获取发出请求的设备的 IP 地址

`ctx.request.ip`和`ctx.request.ips`可以获取发出请求的设备的远程地址。

`ips`属性为我们获取所有设备的 IP 地址，请求通过这些设备到达我们的应用程序。

例如，我们可以如下使用`ip`属性:

```
const Koa = require('koa');
const app = new Koa();app.use(async (ctx, next) => {
  ctx.body = ctx.request.ip;
});app.listen(3000);
```

然后，当我们向我们的应用程序发出请求时，我们会获得发出请求的设备的 IP 地址。

如果`app.proxy`被设置为`true`，我们也可以使用`ips`属性。例如，我们可以如下使用它:

```
const Koa = require('koa');
const app = new Koa();app.proxy = true;app.use(async (ctx, next) => {
  ctx.body = ctx.request.ips;
});app.listen(3000);
```

然后我们会看到一组 IP 地址，请求通过这些地址到达我们的 Koa 应用程序。

![](img/b264d7d50b69b18137fae5d5c0e86bde.png)

Photo by [Carles Rabada](https://unsplash.com/@carlesrgm?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 获取请求子域

我们可以通过使用`subdomain`属性获得请求 URL 的子域。

例如，我们可以如下使用它:

```
const Koa = require('koa');
const app = new Koa();app.use(async (ctx, next) => {
  ctx.body = ctx.request.subdomains;
});app.listen(3000);
```

然后，如果我们的应用程序位于[https://craftygrouchyccables-five-nine.repl.co，](https://CraftyGrouchyCables--five-nine.repl.co,)中，我们将得到`subdomains`属性将返回`[“craftygrouchycables — five-nine”]`，因此我们将得到显示在屏幕上的响应值。

# 结论

我们可以使用`ctx.request`属性来获取所请求 URL 的 IP 和子域。

协议和查询字符串也可用。查询字符串被自动解析为一个对象。

# **简明英语笔记**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页[**plain English . io**](https://plainenglish.io/)上找到所有这些——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**