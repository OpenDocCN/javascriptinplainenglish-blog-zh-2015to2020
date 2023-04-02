# JavaScript 基础—生成器和网络

> 原文：<https://javascript.plainenglish.io/javascript-basics-generators-and-the-web-f30f0a1d2602?source=collection_archive---------9----------------------->

![](img/2ffebef1eb69c735b2a76a70e4457ebf.png)

Photo by [Krzysztof Niewolny](https://unsplash.com/@epan5?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是世界上最流行的编程语言之一。为了有效地使用它，我们必须了解它的基本知识。

在本文中，我们将研究如何定义和使用生成器和 web 通信。

# 发电机

生成器函数让我们创建可以暂停和恢复的函数。

它由关键字`functuion*`表示。

例如，我们可以写:

```
function* random() {
  while (true) {
    yield Math.random();
  }
}
```

我们定义了一个`random`函数，只要我们调用它，它就会不断生成随机数。

`yield`运行时暂停。

我们可以有一个无限循环，因为只有当我们显式运行返回的生成器时，循环才会运行。

要使用它，我们可以写:

```
const ran = random();
console.log(ran.next());
console.log(ran.next());
```

然后我们会得到这样的结果:

```
{value: 0.6168149388512836, done: false}
{value: 0.7733643240483823, done: false}
```

我们调用了我们的生成器函数，并调用了返回的生成器`ran`上的`next`。

同样，我们可以创建一个迭代器对象来顺序返回一个数字数组。

例如，我们可以写:

```
const obj = {
  *[Symbol.iterator]() {
    const arr = [1, 2, 3];
    for (const a of arr) {
      yield a;
    }
  }
}
```

我们有`Symbol.iterator`方法，这是一个生成函数，它产生`arr`的每一项。

运行`yield`时暂停。

然后，我们可以通过运行以下命令将它与 for-of 循环一起使用:

```
for (const o of obj) {
  console.log(o);
}
```

`async`功能是一种特殊类型的发生器。

当它被召唤时，它产生一个承诺，这个承诺被解决或拒绝。

当它产生或等待一个承诺时，承诺的结果或异常值就是`await`表达式的结果。

# 事件循环

异步程序是一部分一部分运行的。

每一部分都可以启动一些动作，并在动作完成或失败时调度要运行的代码。

程序闲置在这两部分之间。

异步操作发生在它自己的调用堆栈上。

这就是为什么我们需要承诺来管理异步动作，以便它们一个接一个地运行。

# JavaScript 和浏览器

JavaScript 用于构建几乎所有的浏览器应用。

浏览器用户使用 HTTP 协议与互联网通信。

HTTP 代表超文本传输协议。

每当我们在浏览器中运行一些东西时，我们就发出一个 HTTP 请求。

我们让他们下载网页，向服务器发送数据，等等。

传输控制协议(TCP)是互联网的通用协议。

它通过让一台计算机监听数据，一台计算机发送数据，让计算机相互通信。

每个侦听器都有一个与之关联的端口号。

如果我们想发送电子邮件，我们使用 SMTP 协议，通过端口 25 进行通信。

我们有一个双向通信管道。一段数据可以从一台计算机流向另一台计算机。

# 网络

浏览器通过万维网(简称 web)进行通信。

我们可以通过把我们的电脑放在互联网上并监听 80 端口来进行网络交流，这样我们就可以通过 HTTP 进行交流。

例如，我们可以向`http://example.com`发出请求，这是一个告诉我们向哪里发出请求的 URL。

URL 只是一段固定格式的文本，让我们找到我们需要的资源。

URL 首先有协议，即`http`，然后是服务器名，即`example.com`。

它也可以有路径，像`http://example.com/foo.html`。`foo.html`是 URL 中的路径。

![](img/cde23fdd1f22ed2b0e69561927cc1167.png)

Photo by [Nicolas Picard](https://unsplash.com/@artnok?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 超文本标记语言

HTML 代表超文本标记语言。

这是存储网页的文档格式。

它包含文本和标签，我们可以用它们来构建文本。

我们用它来描述链接、段落、标题等等。

例如，我们可以写:

```
<!doctype html>
<html> <head>
    <meta charset="utf-8">
    <title>hello</title>
  </head> <body>
    <h1>Hello</h1>
    <p>Read my work
      <a href="http://example.com">here</a>.</p>
  </body></html>
```

所有 HTML 文档都以`<!doctype html>`开头，表示这是一个 HTML 文档。

然后，他们有一些元数据如标题的`head`标签和额外元数据的`meta`标签。

`body`标签有主要内容。

它有标题标签，如用于段落的`h1`和`p`标签，以及用于其他页面链接的`a`标签。

我们可以用 JavaScript 动态改变 HTML。

# 结论

我们可以创建生成器来创建可以暂停和恢复的函数。

JavaScript 对于创建 web 应用程序很有用。它们通过 HTTP 进行通信，HTTP 通过 TCP 进行通信。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**