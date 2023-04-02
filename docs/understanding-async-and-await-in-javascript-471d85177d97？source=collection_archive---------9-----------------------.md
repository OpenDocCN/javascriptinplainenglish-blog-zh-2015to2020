# 理解 JavaScript 中的异步和等待

> 原文：<https://javascript.plainenglish.io/understanding-async-and-await-in-javascript-471d85177d97?source=collection_archive---------9----------------------->

## 学习如何在 JavaScript 中使用异步和等待

![](img/8921635eb6d03bab8c9680d6f5653877.png)

Photo by [Arnold Francisca](https://unsplash.com/@clark_fransa?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

JavaScript 是一种单线程编程语言。这意味着它有一个调用堆栈和一个内存堆。正如预期，它按顺序执行代码，并且必须在进入下一个代码之前完成一段代码的执行。然而，由于有了 V8 引擎，JavaScript 可以是异步的，这意味着我们可以一次在代码中执行多个任务。异步和等待是使异步代码的编写和工作更简单的一种方式。他们让承诺更容易写。

在本文中，我们将在 JavaScript 中发现异步和等待，以及我们如何使用它们。让我们开始吧。

![](img/a80e8d221a12d5f3539d3f44235557dc.png)

Image Created with ❤️ ️By [Mehdi Aoussiad](https://mehdiouss315.medium.com/).

# 异步语法

函数之前的关键字`async`使函数返回一个承诺。这意味着函数将异步运行。请看下面的例子:

```
**async** function myFunction() {
  return "Hello";
}
```

它与以下相同:

```
**async** function myFunction() {
  return Promise.resolve("Hello");
}
```

以下是您使用承诺的方式:

```
myFunction().then(
  function(value) { /* code if successful */ },
  function(error) { /* code if there is an error */ }
);
```

如果一个函数返回一个承诺，你必须以正确的方式处理这个返回的承诺。这意味着使用方法`then()`获取并处理该承诺的返回值。您也可以使用`catch()`、`finally()`等方法。

# 等待语法

函数前的关键字`await`使函数等待承诺。请看下面的例子:

```
let value = await promise;
```

关键字`await`只能在`async`功能中使用。它告诉 JavaScript 暂停异步函数的执行。然后该功能被暂停，直到承诺返回一些结果。

让我们慢慢来，学习如何使用它:

```
async function myDisplay() {
  let myPromise = new Promise(function(myResolve, myReject) {
    myResolve("I love You !!");
  });
  document.getElementById("element").innerHTML = await myPromise;
}

myDisplay();
```

正如您在上面的例子中所看到的，我们在`async`函数中有一个承诺。关键字`await`等待解析消息的承诺，然后它在我们的 HTML 元素中显示它。

# 等待超时

方法`setTimeout()`是一个异步函数。因此，我们将在下面的示例中使用它，在我们将设置的给定时间之后，在网页上显示文本。看看下面的例子:

```
**async** function myDisplay() {
  let myPromise = new Promise(function(myResolve, myReject) {
    setTimeout(function() { myResolve("I love You !!"); }, 3000);
  });
  document.getElementById("demo").innerHTML = **await** myPromise;
}

myDisplay();
```

如您所见，只有在承诺给出结果后(3000 毫秒后)，消息才会显示在页面上。

# 结论

异步和等待使得不必使用`then`和`catch`就可以更容易地写承诺。它使代码更清晰，易于维护。

谢谢你阅读这篇文章，我希望你觉得它有用。

# 更多阅读

[](https://medium.com/javascript-in-plain-english/5-fun-apis-for-your-next-javascript-projects-1834626864c) [## 您下一个 JavaScript 项目的 5 个有趣的 API

### 您可以在 JavaScript 项目中使用的 5 个有用的 API

medium.com](https://medium.com/javascript-in-plain-english/5-fun-apis-for-your-next-javascript-projects-1834626864c)