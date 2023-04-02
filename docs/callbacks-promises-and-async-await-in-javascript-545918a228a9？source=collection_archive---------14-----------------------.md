# JavaScript 中的回调、承诺和异步/等待

> 原文：<https://javascript.plainenglish.io/callbacks-promises-and-async-await-in-javascript-545918a228a9?source=collection_archive---------14----------------------->

## 如何编写异步 JavaScript

![](img/92c8e0a08e891a72ea9dbacf2052b4d4.png)

Photo by [Álvaro Bernal](https://unsplash.com/@abn?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是单线程的，这意味着一次只能发生一件事。**同步代码**按照代码编写的顺序从上到下执行。同步代码也是“阻塞”的——每一行代码在运行之前都要等待前一行代码被执行。

相比之下，**异步代码**是“非阻塞”代码，允许长时间运行的请求不会阻塞主 JavaScript 线程。当请求完成时，可以执行额外的代码。这通常通过以下三种方式之一实现:

1.  复试
2.  承诺
3.  异步/等待

让我们看几个例子，看看如何使用这三种方法编写异步代码。

# 复试

回调函数是作为参数传递给异步函数的函数。一旦工作的异步部分完成，回调函数就被执行。

让我们通过使用`setTimeout`方法来模拟等待 API 请求返回响应。回调方法可能如下所示:

```
function myAsyncMethod(callback) {
  console.log('myAsyncMethod was executed')
  setTimeout(callback, 1000)
}function myCallbackMethod() {
  console.log('myCallbackMethod was executed')
}myAsyncMethod(myCallbackMethod)
```

这段代码首先将文本“myAsyncMethod executed”记录到控制台。然后，它将等待一秒钟，然后将文本“myCallbackMethod executed”记录到控制台

# 承诺

承诺是编写异步代码的另一种方式，可以帮助您避免深度嵌套的回调函数，也称为“回调地狱”承诺可以是以下三种状态之一:待定、已解决或已拒绝。一旦承诺得到解决，您就可以使用`promise.then()`方法来处理响应。如果承诺被拒绝，您可以使用`promise.catch()`方法处理错误。

我们可以用这样的承诺重写我们之前的例子:

```
function myAsyncMethod() {
  console.log('myAsyncMethod was executed')

  return new Promise((resolve, reject) => {
    setTimeout(resolve, 1000) 
  }) 
}function myPromiseThenMethod() {
  console.log('myPromiseThenMethod was executed')
}myAsyncMethod().then(myPromiseThenMethod)
```

和以前一样，这段代码首先将文本“myAsyncMethod executed”记录到控制台。然后，它将等待一秒钟，然后将文本“myPromiseThenMethod 已执行”记录到控制台

# 异步/等待

Async/await 是 ES2017 中引入的新语法。它允许你以一种*看起来*同步的方式编写异步代码，即使它不是。这使得代码更容易理解。

让我们再次重写我们的示例，这次使用 async/await:

```
function myAsyncMethod() {
  console.log('myAsyncMethod was executed')

  return new Promise((resolve, reject) => {
    setTimeout(resolve, 1000) 
  })
}function myAwaitMethod() {
  console.log('myAwaitMethod was executed')
}async function init() {
  await myAsyncMethod()
  myAwaitMethod()
}init()
```

同样，这段代码将首先向控制台记录文本“myAsyncMethod executed”然后，它将等待一秒钟，然后将文本“mywaitmethod executed”记录到控制台

注意我们是如何使用关键字`async`定义`init`函数的。然后，在调用`myAsyncMethod`函数之前，我们使用了`await`关键字来告诉我们的代码，在 `myAsyncMethod`完成运行之后的*之前，我们不想运行调用`myAwaitMethod`的下一行代码。*

现在我们有了看起来同步实际上异步运行的代码！Async/await 给了我们两全其美的东西:非阻塞代码仍然易于阅读和推理。