# Express.js 应用程序对象-渲染和设置

> 原文：<https://javascript.plainenglish.io/guide-to-the-express-application-object-rendering-and-setting-10187e49fdaa?source=collection_archive---------5----------------------->

![](img/360204f517a26f5c3d980952660d3014.png)

Photo by [Belle Hunt](https://unsplash.com/@bellehunt?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

快速应用程序的核心部分是应用程序对象。是应用本身。

在本文中，我们将看看`app`对象的方法以及我们可以用它做什么，包括呈现 HTML 和更改设置。

# app.render(视图，[局部变量]，回调)

我们可以使用`app.render`方法通过它的`callback`函数来呈现视图的 HTML。它接受一个可选参数，该参数是一个包含视图变量的对象。

例如，我们可以如下使用它:

```
const express = require('express');
const bodyParser = require('body-parser');
const app = express();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));app.engine('ejs', require('ejs').renderFile);
app.set('view engine', 'ejs');app.render('index', { people: ['geddy', 'neil', 'alex'] }, (err, html) => {
  console.log(html);
});app.listen(3000);
```

然后，如果我们在`views/index.ejs`中有以下内容:

```
<%= people.join(", "); %>
```

然后我们得到:

```
geddy, neil, alex
```

从`console.log(html);`输出

# app.route(路径)

我们可以使用`app.route`来定义带有给定`path`的路由处理程序。

例如，我们可以如下使用它:

```
const express = require('express');
const bodyParser = require('body-parser');
const app = express();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));app.route('/')
  .get((req, res, next) => {
    res.send('GET request called');
  })
  .post((req, res, next) => {
    res.send('POST request called');
  })
  .all((req, res, next) => {
    res.send('Other requests called');
  })app.listen(3000);
```

然后，当对`/`发出 GET 请求时，我们得到`GET request called`。如果向`/`发出 POST 请求，那么我们得到`POST request called`。

对`/`的任何其他类型的请求都会得到`Other requests called`。

顺序很重要，因为`all`将处理各种请求。因此，如果我们除了其他类型的请求之外，还想监听特定类型的请求，`all`应该放在最后。

![](img/92abd89a5acd79908fa6c58336340d78.png)

Photo by [Shlag](https://unsplash.com/@shlagance?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# app.set(名称，值)

我们可以使用`set`来设置给定`name`到给定`value`的设置。

例如，我们可以如下使用它:

```
const express = require('express');
const bodyParser = require('body-parser');
const app = express();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));
app.set('title', 'Foo');app.get('/', (req, res) => {
  res.send(app.get('title'));
})app.listen(3000);
```

因为我们调用了`app.set(‘title’, ‘Foo’);`来将`title`设置为`Foo`，所以当我们向`/`发出 GET 请求时，我们应该会看到显示的`Foo`。

有些设置是 Express 专用的。它们包括:

*   `case sensitive routing` —用于启用或禁用区分大小写路由的布尔值(例如，如果是`true`，则`/Foo`将被视为与`/foo`不同)
*   `env` —环境模式的字符串
*   `etag` — ETag 响应头
*   `jsonp callback name`—JSONP 回调名称的字符串
*   `json escape` —启用或禁用来自`res.json`、`res.jsonp`或`res.send`的转义 JSON 响应的布尔选项。如果是`true`，则`<`、`>`和`&`将被转义
*   `json replacer` —替换`JSON.stringify`的回调
*   `json spaces`—`JSON.stringify`的空格参数
*   `query parser` —如果设置为`false`，则禁用查询解析，或将查询解析设置为使用`'simple'`或`'extended'`或自定义查询字符串解析功能。
*   `strict routing` —启用/禁用严格路由的布尔设置。如果这是`true`，那么`/foo`将被认为不同于`/foo/`
*   `subdomain offset` —要删除以访问子域的主机的点分隔部分的数量。默认为 2。
*   `trust proxy` —如果是`true`，则表示应用程序在代理后面。`X-Forwarded-*`头将决定客户端的连接和 IP 地址。
*   `views` —查找视图模板的目录字符串或数组。如果它是一个数组，那么视图将按照它们被列出的顺序被搜索
*   `view cache` —用于启用视图模板编译缓存的布尔值
*   `view engine` —用于设置渲染模板的视图引擎的字符串。
*   `x-powered-by` —启用`'X-Powered-By: Express` HTTP 头

## “信任代理”设置的选项

它可以采取以下选项:

*   `true` —客户端的 IP 地址被认为是`X-Forwarded-*`报头最左边的条目
*   `false` —假设应用程序直接面向互联网
*   字符串、逗号分隔的字符串或字符串数组—一个或多个要信任的子网或 IP 地址
*   Number —将前端代理服务器的第 n 跳作为客户端。
*   函数—自定义信任实现

## “etag”设置的选项

它可以采取以下选项:

*   布尔— `true`启用弱 ETag，`false`禁用 ETag
*   String — `'strong'`启用强 ETag，`‘weak’`启用弱 ETag
*   函数—自定义实现。

# 结论

我们可以用`app.render`方法呈现一个 HTML 字符串。它需要一个视图文件名、一个变量对象和一个带有`html`参数的回调来呈现 HTML。

`app.route`让我们定义路线处理程序。

`app.set`让我们为应用程序设置所需的选项。有些设置是 Express 专用的，如果设置了这些设置，将由 Express 处理。