# 理解 JavaScript 中的承诺

> 原文：<https://javascript.plainenglish.io/understanding-promises-in-javascript-10b59cd32776?source=collection_archive---------4----------------------->

## 通过实例了解 JavaScript 中的承诺

![](img/2f91670952ae5c6d39264edda2f728cd.png)

Photo by [Danial RiCaRoS](https://unsplash.com/@ricaros?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# JavaScript 中的承诺是什么？

在 JavaScript 中，承诺就是它听起来的样子。你用它来承诺做某事，通常是异步的。当任务完成时，你要么履行你的承诺，要么不履行。承诺是一个链接生产代码和消费代码的 JavaScript 对象。

在本文中，我们将学习 JavaScript 中的承诺。让我们开始吧。

![](img/1a629f36f0b9851747f999e25a3c49db.png)

Image Created with ❤️️ By Mehdi Aoussiad.

# 创建一个 JavaScript 承诺

承诺是一个构造函数，所以您需要使用`**new**`关键字来创建一个。它以一个函数作为它的自变量，有两个参数:`**resolve**`和`**reject**`。这些是用来确定承诺结果的方法。

看看下面的例子:

```
const myPromise = **new** **Promise**((resolve, reject) => {

});
```

如你所见，我们创建了一个名为`**myPromise**`的承诺，然后我们将带有`**resolve**`和`**reject**`参数的函数传递给构造函数。

# 坚定地完成承诺，拒绝

承诺有三种状态:`pending`、`fulfilled`和`rejected`。

我们在前一个例子中创建的承诺永远停留在`**pending**`状态，因为您没有添加完成承诺的方法。当你希望你的承诺成功时使用`**resolve**`，当你希望它失败时使用`**reject**`。

看看下面的例子:

```
const myPromise = new Promise((resolve, reject) => {
  if(condition here) {
    **resolve**("Promise was fulfilled");
  } else {
    **reject**("Promise was rejected");
  }
});
```

# 履行诺言

当您发出一个服务器请求时，需要花费一些时间，在请求完成后，您通常希望对来自服务器的响应做一些事情。这可以通过使用`**then**`方法来实现。

方法`**then**`在您的承诺通过`**resolve**`兑现后立即执行。这里有一个例子:

```
myPromise.then(result => {
  // do something with the result.
});
```

# 处理被拒绝的承诺

当你的承诺被拒绝时，使用`**catch**`方法。在调用了 promise 的`**reject**`方法后，立即执行。

这里有一个例子:

```
myPromise.catch(error => {
  // do something with the error.
});
```

`error`是传递给`reject`方法的参数。

# 结论

承诺用于处理 JavaScript 中的异步操作。在处理多个异步操作时，它们很容易管理，因为回调可能会创建回调地狱，导致无法管理的代码。

感谢您阅读这篇短文，希望您觉得有用。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**

## 更多阅读

[](https://medium.com/javascript-in-plain-english/5-fun-apis-for-your-next-javascript-projects-1834626864c) [## 为您的下一个 JavaScript 项目准备的 5 个有趣的 API

### 您可以在 JavaScript 项目中使用的 5 个有用的 API

medium.com](https://medium.com/javascript-in-plain-english/5-fun-apis-for-your-next-javascript-projects-1834626864c)