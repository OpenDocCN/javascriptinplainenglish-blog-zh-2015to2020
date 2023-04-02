# 关于快速应用程序对象的更多信息

> 原文：<https://javascript.plainenglish.io/more-about-the-express-application-object-a8ea38f04c93?source=collection_archive---------6----------------------->

![](img/75a821f6ad2c12c64d0095ed5245e21a.png)

Photo by [Mimi Thian](https://unsplash.com/@mimithian?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

快速应用程序的核心部分是应用程序对象。是应用本身。

在本文中，我们将看看`app`对象的方法以及我们可以用它做什么。

# 方法

## app.disabled(名称)

如果给定的设置被禁用，该方法返回`true`。

例如，如果我们有:

```
const express = require('express');
const bodyParser = require('body-parser');const app = express();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));app.disable('foo');app.get('/', function (req, res, next) {
  res.json({ fooDisabled: app.disabled('foo') });
})app.listen(3000, () => console.log('server started'));
```

然后我们得到:

```
{"fooDisabled":true}
```

因为我们叫`app.disable(‘foo’);`

另一方面，如果我们调用`app.enable(‘foo’);`，那么我们得到:

```
{"fooDisabled":false}
```

## app.enable(名称)

我们可以使用`enable`将指定名称的设置设置为`true`。

例如，我们可以如下使用它:

```
const express = require('express');
const bodyParser = require('body-parser');const app = express();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));app.enable('foo');app.get('/', function (req, res, next) {
  res.json({ fooDisabled: app.disabled('foo') });
})app.listen(3000, () => console.log('server started'));
```

然后我们得到:

```
{"fooDisabled":false}
```

因为我们调用了传入了`'foo'`的`enable`。

## app.enabled(名称)

如果启用了具有给定名称的设置，则`enabled`方法返回。例如，我们可以如下使用它:

```
const express = require('express');
const bodyParser = require('body-parser');const app = express();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));app.enable('foo');app.get('/', function (req, res, next) {
  res.json({ fooDisabled: app.enabled('foo') });
})app.listen(3000, () => console.log('server started'));
```

然后我们得到了`{“fooDisabled”:true}`，因为我们调用了`app.enable(‘foo’);`。

## app.engine(外部，回调)

我们调用`engine`方法来设置用于呈现 HTML 输出的模板引擎。

例如，我们可以如下使用它:

```
const express = require('express');
const bodyParser = require('body-parser');
const app = express();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));
app.set('views', './views');app.get('/', (req, res, next) => {
  res.render('index', { people: ['geddy', 'neil', 'alex'] })
})app.engine('html', require('ejs').renderFile);
app.set('view engine', 'ejs');
app.listen(3000, () => console.log('server started'));
```

然后我们可以将我们的模板添加到`views/index.ejs`中，如下所示:

```
<%= people.join(", "); %>
```

我们得到了:

```
geddy, neil, alex
```

已显示。

`callback`应具有参数`path`、`options`和`callback`。

![](img/e849b29bba26b691e1f482be38063a6b.png)

Photo by [Andrew Neel](https://unsplash.com/@andrewtneel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## app.get(名称)

我们可以使用`get`方法获得给定名称的设置值。

例如，如果我们有:

```
const express = require('express');
const bodyParser = require('body-parser');
const app = express();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));
app.set('title', 'Foo');app.get('/', (req, res, next) => {
  res.send(app.get('title'));
})app.listen(3000, () => console.log('server started'));
```

然后我们显示`Foo`,因为我们调用了:

```
app.set('title', 'Foo');
```

## app.get(路径，回调[，回调…])

`get`方法让我们传入一个路由处理程序回调或一系列回调来处理带有给定`path`的 get 请求。

它采用以下参数:

*   `path` —它可以是表示路径或路径模式的字符串或正则表达式。默认为`/`。
*   `callback` —处理请求的功能。它可以是一个中间件功能、一系列中间件功能、一系列中间件功能或以上所有功能的组合

例如，我们可以如下使用它:

```
const express = require('express');
const bodyParser = require('body-parser');
const app = express();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));
app.set('title', 'Foo')app.get('/', (req, res, next) => {
  res.send('GET request made');
})app.listen(3000, () => console.log('server started'));
```

## app.listen(路径，[回调])

它启动一个 UNIX 套接字并监听给定路径上的连接。

例如，我们可以如下使用它:

```
const express = require('express');
const bodyParser = require('body-parser');
const app = express();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));
app.set('title', 'Foo')app.get('/', (req, res, next) => {
  res.send('hi');
})app.listen('/tmp/sock');
```

然后当`/tmp/sock`套接字不使用时，我们可以通过监听这个套接字来启动我们的应用程序。

## app . listen([端口[，主机[，积压]]][，回调])

这个`listen`方法监听给定主机和端口上的连接。

如果端口被省略或者是 0，那么操作系统将分配一个任意的端口给它监听。

我们可以将 Express `app`对象传递给`http.createServer`来监听如下连接:

```
const express = require('express');
const bodyParser = require('body-parser');
const http = require('http');
const app = express();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));app.get('/', (req, res, next) => {
  res.send('hi');
})http.createServer(app).listen(3000);
```

这让我们可以很容易地用相同的代码库提供 HTTP 和 HTTPS 版本的应用程序。

`app`实际上是一个函数，所以我们可以把它作为回调传递给`http.createServer`。

`app.listen()`返回一个`http.Server`对象，这是下面的一个方便的方法:

```
app.listen = function () {
  const server = http.createServer(this)
  return server.listen.apply(server, arguments)
}
```

所以我们可以这样使用它:

```
const express = require('express');
const bodyParser = require('body-parser');
const http = require('http');
const app = express();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));app.get('/', (req, res, next) => {
  res.send('hi');
})app.listen = function () {
  const server = http.createServer(this)
  return server.listen.apply(server, arguments)
}app.listen(3000);
```

# 结论

我们可以用`disabled`方法检查设置是否被设置为`false`。

为了处理 GET 请求，我们可以使用`get`方法。它接受一个路由路径和一个或多个路由处理程序回调。

最后，我们有`listen`方法来监听来自 UNIX 套接字或给定主机和端口的连接。因为`app`实际上是一个函数，所以`app`可以作为一个回调函数传递给 Node.js' `http.createServer`方法。