# 向快速应用添加基本身份验证

> 原文：<https://javascript.plainenglish.io/add-basic-authentication-to-an-express-app-9536f5095e88?source=collection_archive---------1----------------------->

![](img/9f762fcdf491c5aa4e3e122cb2652f7c.png)

Photo by [Patrick Brinksma](https://unsplash.com/@patrickbrinksma?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

要向 Express 应用程序添加基本的身份验证功能，我们可以使用`express-basic-auth`包。

有了它，我们可以在我们的受保护路由的 URL 中检查给定的用户名和密码。

在这篇文章中，我们将看看如何使用它。

# 如何安装

`express-basic-auth`以节点包的形式提供，我们可以通过运行以下命令来安装它:

```
npm install express-basic-auth
```

# 基本用法

我们可以如下使用它:

```
const express = require('express');
const basicAuth = require('express-basic-auth')
const app = express();app.use(basicAuth({
    users: { 'admin': 'supersecret' }
}))app.get('/', (req, res) => {
  res.send('authorized');
});app.listen(3000, () => console.log('server started'));
```

在上面的代码中，我们在应用中包含了来自`express-basic-auth`的`basicAuth`中间件。

然后我们可以用一个具有`users`属性的对象调用`app.use`，用户名作为键，相应的密码作为值。

然后，如果我们向一个 URL 发出 GET 请求，比如:

```
https://admin:supersecret@ThistleDarkExponents--five-nine.repl.co
```

然后我们将得到`authorized`响应，因为用户名和密码与我们在应用程序的对象中列出的相匹配。

否则，我们将得到一个 401 响应和一个可配置的主体，默认情况下是空的。

# 自定义授权

我们也可以传入我们自己的`authorizer` 函数来检查我们想要的凭证。

当比较用户输入和 sercet 凭证时，我们不应该使用带有`==`或`===`的标准字符串比较，因为这将使我们的应用程序容易受到计时攻击。

计时攻击是一种旁道攻击，攻击者试图通过分析运行加密算法所需的时间来危害系统。

我们应该使用软件包提供的`safeCompare`方法。

同样，出于同样的原因，我们应该使用按位逻辑运算符而不是标准运算符。

例如，我们可以使用以下内容:

```
const express = require('express');
const basicAuth = require('express-basic-auth')
const app = express();app.use(basicAuth({
  authorizer: (username, password) => {
    const userMatches = basicAuth.safeCompare(username, 'admin')
    const passwordMatches = basicAuth.safeCompare(password, 'supersecret')
    return userMatches & passwordMatches
  }
}))app.get('/', (req, res) => {
  res.send('authorized');
});app.listen(3000, () => console.log('server started'));
```

在上面的代码中，我们有一个函数，它调用`safeCompare`来分别匹配用户名和密码，然后使用 and 位运算符来组合 2，以确保两者都是`true`。

# 异步授权

我们可以将`authorizeAsync`选项设置为`true`，并将带有`cb`参数的回调函数设置为`authorizer`函数来异步检查凭证:

```
const express = require('express');
const basicAuth = require('express-basic-auth')
const app = express();app.use(basicAuth({
  authorizer: (username, password, cb) => {
    const userMatches = basicAuth.safeCompare(username, 'admin')
    const passwordMatches = basicAuth.safeCompare(password, 'supersecret')
    if (userMatches & passwordMatches)
      return cb(null, true)
    else
      return cb(null, false)
  },
  authorizeAsync: true,
}))app.get('/', (req, res) => {
  res.send('authorized');
});app.listen(3000, () => console.log('server started'));
```

我们返回`cb`回调调用，而不是在我们的`authorizer`函数中直接返回结果。

# 未经授权的响应主体

我们可以将`unauthorizedResponse`属性设置为一个函数，该函数返回一个我们希望在凭证检查失败时显示的响应。

例如，我们可以写:

```
const express = require('express');
const basicAuth = require('express-basic-auth')
const app = express();
app.use(basicAuth({
  users: { 'admin': 'supersecret' },
  unauthorizedResponse: (req) => {
    return `unauthorized. ip: ${req.ip}`
  }
}))
app.get('/', (req, res) => {
  res.send('authorized');
});
app.listen(3000, () => console.log('server started'));
```

返回类似这样的内容:

```
unauthorized. ip: ::ffff:172.18.0.1
```

当基本身份验证失败时。

我们将`unauthorizedResponse`设置为的函数接受 Express request 对象，因此我们可以在函数中使用该对象的任何属性。

![](img/2fd579ee32a6d98c5d62a6aa94fa36c8.png)

Photo by [Danka & Peter](https://unsplash.com/@dankapeter?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 挑战

我们可以让浏览器显示一个弹出窗口，这样用户可以通过向对象添加一个`challenge: true`选项来输入身份验证凭证。

此外，我们设置了`realm`来标识要进行身份验证的系统，并可用于保存质询的凭证，方法是传递一个静态或函数，该函数传递请求对象并预期返回质询:

```
const express = require('express');
const basicAuth = require('express-basic-auth')
const app = express();app.use(basicAuth({
  users: { 'admin': 'supersecret' },
  challenge: true,
  realm: 'foo',
}))app.get('/', (req, res) => {
  res.send('authorized');
});app.listen(3000, () => console.log('server started'));
```

有了上面的代码，当我们加载`/`页面时，我们应该会得到一个对话框来输入用户名和密码。

# 结论

使用`express-basic-auth`包可以很容易地将基本认证添加到 Express 应用中。

它允许我们添加有效凭证的列表，或者使用一个函数来验证凭证。

我们还可以让用户输入用户名和密码，并在用户未经授权时显示自定义内容。

## **用简单的英语写的 JavaScript 的注释:**

我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到[submissions@javascriptinplainenglish.com](mailto:submissions@javascriptinplainenglish.com)给我们，我们会把你添加为作者。