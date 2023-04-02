# Express.js 响应对象—更多发送方式

> 原文：<https://javascript.plainenglish.io/guide-to-the-express-response-object-more-ways-to-send-4091224978e4?source=collection_archive---------3----------------------->

![](img/2eabbefa2ea4fec41dd1dcab6261239b.png)

Photo by [Nick Fewings](https://unsplash.com/@jannerboy62?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Express response 对象允许我们向客户端发送响应。

可以发送各种响应，比如字符串、JSON 和文件。此外，除了主体之外，我们还可以向客户端发送各种标头和状态代码。

在本文中，我们将了解如何设置头和发送状态代码，包括设置头的`set`方法、设置响应状态代码的`status`、设置`Content-Type`头值的`type`方法和设置`Vary`头值的`vary`方法。

# 方法

## 资源集(字段[，值])

我们可以使用`set`方法在发送响应之前设置响应头。

例如，我们可以如下使用它:

```
const express = require('express');
const path = require('path');
const app = express()
app.use(express.json())
app.use(express.urlencoded({ extended: true }))app.get('/', (req, res, next) => {
  res.set({
    'Content-Type': 'text/plain',
    'Content-Length': '100'
  })
    .send();
})
app.listen(3000);
```

然后当我们向`/`发出请求时，我们得到`Content-Length`头被设置为 100，而`Content-Type`被设置为`text/plain`。

注意，我们必须调用`send`来发送响应。

我们可以用 Postman 这样的 HTTP 客户端来验证这一点。

## 资源状态(代码)

我们可以调用`status`方法在发送响应之前添加一个状态代码。

例如，我们可以如下使用它:

```
const express = require('express');
const path = require('path');
const app = express()
app.use(express.json())
app.use(express.urlencoded({ extended: true }))app.get('/', (req, res, next) => {
  res.status(400).send('Bad Request');
})app.listen(3000);
```

然后我们得到显示的`Bad Request`和 400 响应代码。

## 资源类型(类型)

`type`方法将`Content-Type` HTTP 头设置为由`mime.lookup()`方法从指定类型的`node-mime`包中确定的 MIME 类型。

如果`type`包含`/`字符，那么它将`Content-Type`设置为`type`。

此方法不发送响应。

例如，我们可以如下使用它:

```
const express = require('express');
const path = require('path');
const app = express()
app.use(express.json())
app.use(express.urlencoded({ extended: true }))app.get('/', (req, res) => {
  res.type('.html').send();
})
app.listen(3000);
```

然后我们得到`Content-Type`响应头的值是`text/html; charset=utf-8`。

如果我们有:

```
const express = require('express');
const path = require('path');
const app = express()
app.use(express.json())
app.use(express.urlencoded({ extended: true }))app.get('/', (req, res) => {
  res.type('.png').send();
})
app.listen(3000);
```

那么我们得到`Content-Type`的值是`image/png`而不是`text/html`。

![](img/9f1c45efeafbb67af9c7bded5458430d.png)

Photo by [Jametlene Reskp](https://unsplash.com/@reskp?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 分辨率变化(字段)

如果这个字段还不存在的话，`vary`方法会将它添加到`Vary`响应头中。

例如，我们可以如下使用它:

```
const express = require('express');
const path = require('path');
const app = express()
app.use(express.json())
app.use(express.urlencoded({ extended: true }))app.get('/', (req, res) => {
  res.vary('User-Agent').send();
})
app.listen(3000);
```

然后我们将`Vary`响应头设置为`User-Agent`。

`Vary`头让我们可以进行内容协商，这是一种用相同的 URL 提供不同种类数据的机制。

用户代理可以指定什么最适合服务器。

`Vary`头指示哪些头用于内容协商。

由于我们将`Vary`头设置为`User-Agent`，我们的应用程序根据浏览器的用户代理使用`User-Agent`头来提供不同种类的内容。

# 结论

`set`方法让我们在发送响应之前设置头部。它接受一个带有我们想要发送的响应头的对象。

`status`方法用于在发送之前设置响应状态代码。我们可以用响应体来链接它。

`type`方法用于设置`Content-Type`标题值。返回的 MIME-type 由来自`node-mime`的`mime.lookup()`方法确定。

设置`Vary`标题值的`vary`方法。`Vary`头用于内容协商，根据`Vary`头的值指定的字段提供不同的内容。