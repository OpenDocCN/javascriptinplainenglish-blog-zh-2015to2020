# 排除使用 Express 调用 Express 中间件的路由-除非

> 原文：<https://javascript.plainenglish.io/excluding-routes-from-calling-express-middleware-with-express-unless-3389ab4117ef?source=collection_archive---------3----------------------->

![](img/bea26b5eda41d33844203c041a2a320c.png)

Photo by [Zach Miles](https://unsplash.com/@zachmiles?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了在满足条件时有条件地跳过中间件，我们可以使用`express-unless`包。

在这篇文章中，我们将看看如何使用它。

# 装置

我们可以通过运行以下命令来安装它:

```
npm i express-unless --save
```

# 使用

安装后，我们可以分配由`express-unless`公开的`unless`函数，并将其分配给我们的中间件函数的`unless`属性。

例如，我们可以写:

```
const express = require('express');
const unless = require('express-unless');

const static = express.static(__dirname + '/public');
static.unless = unless;const app = express();
app.use(static.unless({ method: 'OPTIONS' }));
app.listen(3000, () => console.log('server started'));
```

在上面的代码中，我们将`static`的`unless`属性设置为`express-unless`包的`unless`函数。

然后我们写道:

```
app.use(static.unless({ method: 'OPTIONS' }));
```

排除`static`路线运行选项请求。

如果我们正在编写自己的中间件，我们可以如下分配中间件函数的`unless`属性:

```
const express = require('express');
const unless = require('express-unless');

const static = express.static(__dirname + '/public');const logHostname = (req, res, next) =>{
  console.log(req.hostname);
  next();
}
logHostname.unless = unless;const app = express();
app.use(logHostname.unless({ method: 'OPTIONS' }));
app.listen(3000, () => console.log('server started'));
```

在上面的代码中，我们定义了`logHostman`中间件来记录主机名。然后我们将函数的`unless`属性设置为`express-unless`包中公开的`unless`函数。

# 选择

在我们传入`unless`的对象中，我们可以传入以下选项:

*   `method` —这可以是一个字符串或字符串数组。如果请求方法匹配，那么中间件不会运行。
*   `path` —这可以是字符串、正则表达式或字符串或正则表达式的数组。它也可以是 URL 和方法键值对的对象数组。如果请求路径或者路径和方法匹配，那么中间件不会运行。
*   `ext` —这是一个字符串或字符串数组。如果路径以这些扩展结束，那么中间件就不会运行。
*   `custom` —接受`req`并返回布尔值的函数。如果函数为给定的请求返回`true`，那么中间件将不会运行。
*   `useOriginalUrl` —这应该是一个布尔值。默认为`true`。如果是`false`，则`path`将与`req.url`而不是`req.originalUrl`匹配。

# 高级示例

当我们有`jpg`、`html`、`css`或`.js`扩展时，我们可以排除调用`express.static`中间件，如下所示:

```
const express = require('express');
const unless = require('express-unless');
const url = require('url');
const static = express.static(__dirname + '/public');static.unless = unless;const app = express();
app.use(static.unless((req) => {
  const ext = url.parse(req.originalUrl).pathname.substr(-4);
  return !['.jpg', '.html', '.css', '.js'].includes(ext);
}));
app.listen(3000, () => console.log('server started'));
```

在上面的代码中，我们检查了`req.originalUrl`属性的扩展，以查看请求是否在数组中列出了扩展，如果有，则跳过`static`中间件。

我们还可以将路径和请求方法混合在一起，如下所示:

```
const express = require('express');
const unless = require('express-unless');const static = express.static(__dirname + '/public');
const logHostname = (req, res, next) => {
  console.log(req.hostname);
  next();
}
logHostname.unless = unless;
const app = express();
app.use(logHostname.unless({
  path: [
    '/index.html',
    { url: '/', methods: ['OPTIONS', 'PUT'] }
  ]
}));
app.listen(3000, () => console.log('server started'));
```

然后，当我们使用 PUT 或 OPTION 请求方法转到`index.html`或`/`时，我们停止`logHostname`中间件的运行。

![](img/d4081282130b03cf9a48cdda6a88a4a5.png)

Photo by [Diego Jimenez](https://unsplash.com/@diegojimenez?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以通过使用`express-unless`包来阻止中间件在某些路由或请求方法上运行。

要使用它，我们只需设置从包中暴露的`unless`函数，并将其设置为我们的中间件函数的`unless`属性。

然后，我们可以通过检查 URL 和/或请求方法，使用各种条件来排除路由。

【JavaScript 用简单英语写的一句话:我们总是对帮助推广优质内容感兴趣。如果你有一篇文章想用简单的英语提交给 JavaScript，请用你的 Medium 用户名给我们发一封电子邮件到[submissions@javascriptinplainenglish.com](mailto:submissions@javascriptinplainenglish.com)，我们会把你添加为作者。