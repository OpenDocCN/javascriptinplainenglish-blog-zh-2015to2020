# JavaScript 中回调函数的力量

> 原文：<https://javascript.plainenglish.io/the-power-of-callback-functions-in-javascript-c5a9582897fb?source=collection_archive---------15----------------------->

## 用实例理解回调函数

![](img/e484208ba962738a7154597e4afba446.png)

Photo by [Sean Lim](https://unsplash.com/@seanlimm?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

作为一名 JavaScript 开发人员，甚至是初学者，您将需要了解 JavaScript 中的回调函数。因为一旦你理解了回调是如何工作的，你就会在 JavaScript 中变得更好。回调函数在高级 JavaScript 主题列表中，尽管它们看起来很容易理解。

# 什么是回调函数？

从根本上说，函数式编程指定将函数用作参数。**回调函数**也被称为高阶函数，是作为参数传递给另一个函数的函数。回调函数的使用也被称为回调模式。

![](img/766fa1b11dc3e921705edfa861de979b.png)

Photo by [Shahadat Rahman](https://unsplash.com/@hishahadat?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 回调函数是如何工作的？

当我们将回调作为参数传递给另一个函数时，我们只是传递函数定义，而不是执行它。因为另一个函数的参数中有回调函数作为函数定义，所以它可以随时执行回调。回调确保函数不会在任务完成前运行，而是在任务完成后立即运行。它使我们的代码异步，并使我们免于错误和 bug。看看下面的例子。

# 创建回调函数:

在下面的例子中，`**setTimeout**` 函数接受消息函数作为回调函数。考虑以下示例:

Callback Function.

# 带有事件的回调函数:

在 JavaScript 中，有必要对事件使用回调函数。假设我们想通过点击一个按钮来进行点击事件。

Button in HTML.

下面的示例将一条消息记录到单击了**按钮的控制台上，但是在首先单击该按钮之后，因为回调是这样工作的，所以它们在任务完成后异步执行。**

Callback function inside a click event.

# 结论:

回调函数非常有用，它们使我们的代码异步运行，并使我们免受臭虫的侵害。每个 JavaScript 开发人员都应该理解回调是如何工作的。这篇文章到此为止，我希望你今天学到了一些新东西。

喜欢这篇文章吗？如果是这样的话，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获得更多类似的内容吧！**