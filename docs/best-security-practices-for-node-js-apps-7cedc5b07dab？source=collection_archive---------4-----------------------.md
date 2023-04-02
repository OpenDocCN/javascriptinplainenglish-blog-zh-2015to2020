# Node.js 应用程序的最佳安全实践

> 原文：<https://javascript.plainenglish.io/best-security-practices-for-node-js-apps-7cedc5b07dab?source=collection_archive---------4----------------------->

![](img/f9c833156b5ff674bf3a5049047ac704.png)

Photo by [Erik Mclean](https://unsplash.com/@introspectivedsgn?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究一些避免在 Node.js 应用程序中产生安全漏洞的最佳实践。

# 缓冲区中的 noAssert 标志

我们不应该将`noAsserr`设置为`true`来跳过`offset`的验证。

否则，我们也许可以将`offset`设置在`Buffer`的末尾。

# exec 中的动态代码

我们不应该在`exec`中动态编码。

这样，我们避免了攻击者能够插入他们想要在我们的服务器上运行的命令。

例如，我们应该写:

```
child_process.exec('ls', (err, data) => {
  console.log(data);
});
```

但是我们不应该写:

```
const path = "user input";
child_process.exec(`ls -l ${path}`, (err, data) => {
  console.log(data);
});
```

我们不应该在第一个论点中有任何动态的表述。

这边，那边；攻击者不可能在我们的服务器上运行命令。

要运行带参数的命令，我们可以用`execFile`来完成:

```
const child_process = require('child_process');

const path = "./foo/bar"
child_process.execFile('/bin/ls', ['-l', path], (err, result) => {
  console.log(result)
});
```

我们只在我们想要的地方注入我们想要添加的参数，而不是让任何人输入任何东西。

同样，我们也可以使用`spawn`:

```
const child_process = require('child_process');

const path = "."
const ls = child_process.spawn('/bin/ls', ['-l', path])
ls.stdout.on('data', (data) => {
  console.log(data.toString());
});
```

和`execFile`一样。

用户不能在 shell 中运行子命令。

但是，运行`/bin/find`这样的东西，可能还是会让用户接管我们的系统。

# 没有胡子逃脱

我们不应该把`object.escapeMarkup`设置成`false`。

这可以让一些模板引擎在 HTML 实体中禁用转义。

因此，这个值可能允许攻击者在我们的应用程序上进行跨站点脚本编写。

我们不应该让用户输入类似这样的内容:

```
<body onload=alert('foo')>
```

直接结束运行。

# 不要使用 eval

`eval`绝对是我们不该用的烂函数。

它让我们从字符串中运行表达式。

因此，这导致攻击者也能够运行他们想要的代码。

允许任何人运行他们想要的代码，如果肯定不好的话。

# 使用 CSRF 中间件来防止 CSRF 攻击

跨站点请求伪造我们的 CSRF 是一种攻击，攻击者可以在未经授权的情况下在站点上运行请求。

当用户从合法网站转到恶意网站，但仍然拥有合法网站的凭据时，就会发生这种情况。

我们应该使用`csrf` 中间件来防止 CSRF 攻击。

当我们使用它时，我们不应该在我们的 Express 应用程序中使用`methodOverride`中间件方法来覆盖使用 post 键或`x-http-method-override`请求的方法。

方法覆盖功能可以绕过由`csrf`中间件提供的 CSRF 令牌检查。

因此，我们不应该在我们的 Express app 中调用`express.methodOverride`。

然而，我们应该注意，对于 Ajax 请求的同源策略，使用`x-http-method-override`头会困难得多。

# 使用 fs 方法时没有动态文件路径

当使用`fs`方法时，我们不应该使用动态文件路径。

这样，攻击者就无法访问我们系统中我们不希望人们访问的不同路径。

我们不希望人们穿越带有`../`或`./`的路径。

# 没有非文字正则表达式

正则表达式不应该是动态的。这样，没人能改变他们。

一些正则表达式可能会挂起我们的节点应用程序。

它们包括有许多组的正则表达式，也检查长字符串。

# 没有非文字要求

在我们的`require`函数调用中不应该有动态参数。

这样，攻击者就无法传入自己的字符串来查找和使用模块。

`require`通过路径查找模块，也在`node_modules`文件夹中查找。

# 没有不安全的比较

我们不应该使用顺序检查输入的不安全的比较。

它们包括`==`、`!=`、`!==`和`===`。

这可能会导致计时攻击。

一次检查一个字符。

所以这些运算符会遍历每个字符，并对它们进行比较。

攻击者可以利用比较的时间倒推，查看有多少字符匹配，这使得破解密码变得更加容易。

![](img/c6ab328c7a8d795562879b01147887fe.png)

Photo by [Zdeněk Macháček](https://unsplash.com/@zmachacek?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

除了让应用程序正常工作，我们还应该确保它们是安全的。

在比较字符串和计算动态表达式时，我们必须考虑安全性。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)和 [**找到它们订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**