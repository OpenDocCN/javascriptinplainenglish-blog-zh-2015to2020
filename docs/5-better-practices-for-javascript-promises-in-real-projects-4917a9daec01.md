# 真实项目中 JavaScript 承诺的 5 个更好实践

> 原文：<https://javascript.plainenglish.io/5-better-practices-for-javascript-promises-in-real-projects-4917a9daec01?source=collection_archive---------4----------------------->

## 使用 Promise.all、Promise.race 和 Promise.prototype.then 来提高代码质量。

![](img/c6e6672de0c054c3f078209ad5c42b7d.png)

Photo by [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在学习了 Promise 的基本用法后，本文希望能帮助你在实际项目中更好地使用 Promise。

# 承诺。所有

Promise.all 实际上是一个承诺，它接受一个承诺数组(或一个可迭代的)作为输入。然后，当所有的承诺都得到解决，或者其中任何一个被拒绝时，问题就解决了。

例如，假设您有十个承诺(执行网络调用或数据库连接的异步操作)。你必须知道什么时候所有的承诺都解决了，或者你必须等到所有的承诺都解决了。所以你把所有的十个承诺传递给了 promise.all，那么，Promise.all 本身作为一个承诺将会得到解决，一旦所有的十个承诺都得到解决，或者十个承诺中的任何一个被错误地拒绝。

**让我们用代码来看看:**

```
Promise.all([promise1, promise2, promise3])
 .then(result) => {
   console.log(result)
 })
 .catch(error => console.log(`Error in promises ${error}`))
```

如您所见，我们将一个数组传递给 promise.all。当三个承诺都被解析时，promise.all 被解析，输出得到安慰。

**我们来看一个例子:**

在上面的示例中，Promise.all 在 2000 ms 后解析，输出以数组的形式进行控制。

关于承诺，有趣的一点是承诺的顺序是保持不变的。数组中的第一个承诺将被解析为输出数组的第一个元素，第二个承诺将是输出数组中的第二个元素，依此类推。

好了，以上是 promise.all 的基本用法，下面我来介绍一下它在真实项目中的应用。

## 1.同步多个异步请求

在实际项目中，一个页面往往需要向后台发送多个异步请求。等到后台返回结果后，我们才开始呈现页面。

一些程序员可能会编写这样的代码:

上面的代码确实有效，但是在这个代码中有两个缺陷:

*   每次我们从服务器请求数据时，我们需要编写一个单独的函数来处理数据。这样会导致代码冗余，也不方便以后的升级和扩展。
*   每个请求花费不同的时间，导致函数呈现页面三次不同步，用户感觉页面卡住了。

现在我们可以使用 Promise.all 来优化我们的代码。

当所有请求完成后，我们统一处理数据。

## 2.处理异常

在上面的例子中，我们非常直接地采用这种方法来处理异常:

```
Promise.all([p1, p2]).then(res => {
  // ...
}).catch(error => {
  // handle error
})
```

我们知道，Promise.all 机制是，只有当 Promise 数组中的任何 promise 实例作为参数抛出异常时，整个 Promise.all 函数才会直接进入 catch 方法，而不管其他 promise 实例是成功还是失败。

但在实践中，我们经常需要的是这样的:即使一个或多个 promise 实例抛出异常，我们仍然希望 Promise.all 继续正常执行。比如上面的例子，即使`getBannerList()`发生异常，只要`getStoreList()`或者`getCategoryList()`没有发生异常，我们就继续想执行程序。

为了满足这一需求，我们可以使用一个技巧来增强 Promise.all 特性。我们可以这样写代码:

这样，即使一个 promise 实例出现异常，也不会中断 Promise.all 的其他实例。

应用到前面的例子，这是结果。

## 3.让多个 promise 实例一起工作

当用户试图上传或发布某些内容时，我们可能需要验证用户提供的内容。比如检查内容是否包含血腥暴力、色情、假新闻等。在许多情况下，这些检测行为是由后端提供的不同 API 或 SaaS 服务提供商提供的不同云功能来执行的。

一些程序员可能会编写这样的代码:

但是有了 Promise.all，我们可以让不同的承诺任务协同工作:

# 承诺.比赛

`promise.race`的参数和`promise.all`一样，可以是 promise 数组，也可以是 iterable 对象。

`Promise.race()`方法返回一个承诺，只要 iterable 中的一个承诺满足或拒绝，该承诺就会满足或拒绝，并带有该承诺的值或原因。

## 4.计时功能

当我们从后端服务器异步请求资源时，我们通常会限制一个时间。如果在指定的时间内没有收到数据，将引发异常。

考虑一下你将如何实现这个特性？Promise.race 可以帮助我们解决这个问题。

# 答应我，然后

我们知道`promise.then()`总是返回一个 promise 对象，所以`promise.then`支持链式调用。

```
Promise.then().then().then()
```

## 5.承诺链

因此，如果接口返回的数据量很大，并且一个 then 中的处理看起来很臃肿，我们可以考虑访问处理逻辑，并在多个 then 方法中轮流执行它:

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****