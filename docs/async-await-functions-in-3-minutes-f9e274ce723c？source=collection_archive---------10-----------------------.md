# 一个 3 分钟的 JavaScript 异步/等待速成班

> 原文：<https://javascript.plainenglish.io/async-await-functions-in-3-minutes-f9e274ce723c?source=collection_archive---------10----------------------->

![](img/f83a65cb29b9033d38c7a0f8b076521f.png)

Photo by [Goran Ivos](https://unsplash.com/@goran_ivos?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 先决条件

为了让我在几分钟内解释如何编写 async/await 函数，您需要很好地理解 JavaScript 的异步特性以及如何使用承诺。这里的目标受众是那些习惯于承诺的开发人员，他们已经准备好通过使用 async/await 来提升他们的代码。或者只是重构他们的代码以获得更整洁的外观。已经有大量关于 promises 和 async/await 的深入文章，所以我就长话短说。

# 为什么使用 Async/Await？

在我看来，Promises 和 Async/Await 都很好。无论你选择哪种方法，结果都是一样的。我也更喜欢 async/await 的原因是因为它使代码更干净，更容易阅读，尤其是当你有多个链接的时候。当我写代码时，可读性和可维护性总是在我的脑海中浮现。你的同事会欣赏你干净的代码，即使他们从来没有提到过！

# **需要记住的关键事项**

*   Async/Await 只不过是承诺周围的语法糖
*   异步函数是返回承诺的函数
*   Await 只能在标记为 ***async*** 的函数中使用
*   使用带有异步函数的 **try/catch** 来捕捉异常

# 通过例子学习

对我个人来说，最好的学习方法之一就是通过例子。这就是为什么我将使我的解释简短。我还打算让我的例子简单易懂。

## 使用 Axios 的简单 HTTP 请求

让我们来看一个使用 promises 从 jsonplaceholder 获取帖子的简单函数。

我们可以像这样使用 async/await 重写它:

## 一个更真实的 HTTP 请求示例

这是我最近参与的一个项目的片段。Superagent 是类似于 Axios 的轻量级 http 库。 *getCustomer* 函数正在发出 HTTP 请求并设置授权头。一旦我收到响应，我就在我的数据变量中设置它们(我在这个项目中使用 Vue.js 作为前端框架)。

现在让我们重写这个代码以使用异步/等待:

## **异步/等待一个简单的 MongoDB 请求**

下面是一个用 Mongoose 使用 promises 对 MongoDB 进行数据库调用的例子。

异步/等待！

# 结论

我在这里的意图是保持文章简短和甜蜜。人们可以用作参考或指南的东西。如前所述，如果您正在寻找关于 Promises 和 Async/Await 内部工作方式的更深入的解释，有大量关于这个主题的资源。

我在业余时间写这些文章是为了娱乐。如果你喜欢这篇文章，请在下面留下你的喜欢和评论！可以关注我上 [*中*](https://medium.com/@this.kevinluu) *和* [*推特*](https://twitter.com/kluu_10) *。感谢支持！*