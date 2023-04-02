# 使用 CORS 中间件进行跨域通信

> 原文：<https://javascript.plainenglish.io/using-the-cors-middleware-for-cross-domain-communication-6e78d17b7eac?source=collection_archive---------3----------------------->

![](img/8855fa7b634dbc615e69aab614d4e3fc.png)

Photo by [Hubert Van den Borre](https://unsplash.com/@trebhu?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

默认情况下，Express 没有提供任何东西来支持跨域通信。

随着单页面 web 应用程序的流行，我们需要在应用程序中启用跨域通信，以便单页面 web 应用程序可以与我们的后端通信。

在本文中，我们将看看如何使用`cors`中间件来实现跨域通信。CORS 站是跨源资源共享，这是让浏览器应用程序与其他领域的应用程序通信的方式。

# 添加中间件

为了安装中间件，我们运行:

```
npm install cors
```

那么我们可以如下使用它:

```
const express = require('express');
const bodyParser = require('body-parser');
const cors = require('cors');
const app = express();
app.use(cors())app.get('/', (req, res, next) => {
  res.json({ foo: 'bar' })
})app.listen(3000);
```

因为我们有:

```
app.use(cors())
```

整个应用程序将启用跨域通信。

# 选择

`cors`中间件接受一个可选的`options`对象。它有以下选项:

## 起源

*   如果这是一个`boolean` —如果我们希望允许所有域的请求，则设置为`true`。`false`如果我们想禁用 CORS。
*   string——将`origin`设置为一个特定的原点，比如`http://example.com`,我们希望从这个原点接受请求
*   Regex——我们可以将其设置为一个 regex，以匹配我们希望接受请求的原始域
*   array——我们可以将它设置为我们希望接受请求的源字符串的数组
*   函数—我们可以将其设置为以`origin`和`callback`为参数的函数。`origin`是来自`Origin`请求头的请求的原始 URL。回调的第一个参数是错误对象，第二个参数是布尔值，表示我们是否希望 CORS 继续。

## 方法

`methods`让我们设置我们想要允许的请求方法，比如 GET、POST 等。我们可以传入一个字符串，比如`'GET,POST'`，或者一个类似`['GET', 'POST']`的数组。

它设置`Access-Control-Allow-Methods` CORS 割台。

## 允许的标题

它设置`Access-Control-Allow-Headers` CORS 标题。

我们可以把它设置成一个类似于`‘Content-Type,Authorization’` 的字符串或者一个数组`[‘Content-Type’, ‘Authorization’]`。

## 暴露的标题

这将设置`Access-Control-Expose-Headers` CORS 割台。

我们可以把它设置成一个类似于`‘Content-Range,X-Content-Range’`的字符串或者一个数组`[‘Content-Range’, ‘X-Content-Range’]`。

如果未指定任何内容，则不会公开任何自定义标头。

## 资格证书

这将设置`Access-Control-Allow-Credentials` CORS 标题。

如果我们想通过割台，将此设置为`true`。否则省略。

## maxAge

`maxAge`落下了`Access-Control-Max-Age` CORS 的头球。

将它设置为一个整数来传递头，否则，它将被忽略。

## 预闪光继续

设置此项，将 CORS 预检响应传递给下一个处理程序

## 选项成功状态

更改成功的选项请求的状态代码。

![](img/9c273cf1154a33355724f693604938a9.png)

Photo by [George Hiles](https://unsplash.com/@hilesy?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 启用 CORS 的单一路由

我们可以通过为单个路由添加 CORS 中间件来为单个路由启用 CORS:

```
const express = require('express');
const bodyParser = require('body-parser');
const cors = require('cors');
const app = express();app.get('/', cors(), (req, res, next) => {
  res.json({ foo: 'bar' })
})app.listen(3000);
```

那么只有`/`路线启用了 CORS。

# 配置 CORS

我们可以配置`cors`只接受不同域的请求。此外，我们可以更改 OPTIONS 请求的成功响应代码，这是针对实际请求的。

例如，我们可以为成功的选项请求配置域和状态代码，如下所示:

```
const express = require('express');
const bodyParser = require('body-parser');
const cors = require('cors');
const app = express();
const corsOptions = {
  origin: '[https://WheatTurboRuntimes--five-nine.repl.co'](https://WheatTurboRuntimes--five-nine.repl.co'),
  optionsSuccessStatus: 200
}app.use(express.static('public'));
app.get('/foo', cors(corsOptions), (req, res, next) => {
  res.json({ foo: 'bar' })
})app.listen(3000);
```

我们将我们的应用程序配置为只接受来自`[https://WheatTurboRuntimes--five-nine.repl.co](https://WheatTurboRuntimes--five-nine.repl.co')`的请求，选项请求的成功状态代码为 200，而不是通常的 204。

然后在`public`文件夹中，我们添加一个`script.js`并添加:

```
(async ()=>{
  const result = await fetch('[https://WheatTurboRuntimes--five-nine.repl.co/foo'](https://WheatTurboRuntimes--five-nine.repl.co/foo'));
  const res = await result.json();
  const p = document.querySelector('p');
  p.innerText = res.foo;
})();
```

然后在`index.html`中，我们添加:

```
<!DOCTYPE html>
<html><head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>repl.it express</title>
  <link href="style.css" rel="stylesheet" type="text/css" />
</head><body>
  <p></p>
  <script src="script.js">
  </script>
</body></html>
```

然后我们在屏幕上看到`bar`。

# 动态起源

通过将`origin`属性的值设置为一个函数，`origin`设置可以是动态的。

该函数以`origin`和`callback`为参数。

`origin`具有请求的发起域，而`callback`是在检查域后调用的函数。

我们可以将其设置如下:

```
const express = require('express');
const bodyParser = require('body-parser');
const cors = require('cors');
const app = express();
const corsOptions = {
  origin: (origin, callback) => {
    if (origin.includes('WheatTurboRuntimes')) {
      callback(null, true)
    }
  },
  optionsSuccessStatus: 200
}app.use(express.static('public'));
app.get('/foo', cors(corsOptions), (req, res, next) => {
  res.json({ foo: 'bar' })
})app.listen(3000);
```

然后，当我们向`/foo`发送 GET 请求，并将`Origin`头设置为`https://WheatTurboRuntimes--five-nine.repl.co`时，我们将返回:

```
{
    "foo": "bar"
}
```

当原点符合我们的条件，也就是包含`WheatTurboRuntimes`时，就会调用`callback`。

回调的第一个参数是错误对象，第二个参数是布尔值，表示我们是否希望 CORS 继续。

# 启用 CORS 预飞行

如果我们想为预检选项请求启用 CORS，我们可以使用`cors`中间件来实现。

例如，我们可以如下使用它:

```
const express = require('express');
const bodyParser = require('body-parser');
const cors = require('cors');
const app = express();app.use(express.static('public'));
app.options('/foo', cors())
app.get('/foo', cors(), (req, res, next) => {
  res.json({ foo: 'bar' })
})app.listen(3000);
```

代码:

```
app.options('/foo', cors())
```

将使选项请求可用于所有域。

我们还可以通过编写以下内容为所有路径启用飞行前请求:

```
const express = require('express');
const bodyParser = require('body-parser');
const cors = require('cors');
const app = express();app.use(express.static('public'));
app.options('*', cors())
app.get('/foo', cors(), (req, res, next) => {
  res.json({ foo: 'bar' })
})app.listen(3000);
```

# 结论

我们可以使用`cors`中间件来实现与不同来源的浏览器应用程序的跨域通信。

它允许我们设置各种选项，如要接受的起点，以及选项成功响应的状态。

此外，我们可以设置各种响应头，并动态检查来源，看看我们是否希望允许它。

`cors`中间件可以用于单条路由，也可以用于所有路由。