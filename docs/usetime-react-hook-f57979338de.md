# useTime() React 挂钩

> 原文：<https://javascript.plainenglish.io/usetime-react-hook-f57979338de?source=collection_archive---------1----------------------->

## React 中的倒计时很复杂/很难。让我们使用钩子来创建一个 useTime 钩子。

![](img/55250e1a349d25bf12bedd13e420237c.png)

Time keeps on ticking… Photo by [Djim Loic](https://unsplash.com/@loic?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/clock?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 钩子(代码优先！)

复制粘贴这个要点([https://gist . github . com/jamesfulford/7f 3311 BD 918982 e 68d 911 a9 c 70 b 27415](https://gist.github.com/jamesfulford/7f3311bd918982e68d911a9c70b27415))并在您的代码库中尝试一下。别担心，你回来的时候这篇文章还在。

useTime, getTime, and sample usage in a Countdown component.

这个要点目前使用 Luxon 处理日期，但是注释显示了如何将其转换为使用 Moment 或原生 JS 日期。如果时区复杂，纪元整数也是明智之举。

PSA:如果你正在处理时区，使用一个库。否则，这将发生在你和你所有的朋友身上(我警告过你)

# 问题是

渲染 React 组件时使用当前时间可能会很棘手。目前的文献(媒体文章、各种博客和 StackOverflow 答案)通常有 3 个问题:

# 实现是错误的

在 React 中使用当前时间很困难。由于对 Javascript 中的`setInterval`的误解，以及对 React 中的`state`转换的误解，`setInterval`回调递增/递减状态的常见解决方案会很快失去同步。

在`render`方法中直接访问当前时间不会在需要时触发重新渲染，并且通过一个属性/上下文只会将问题委托给不同的组件。因此，使用状态的解决方案是正确的解决方案。

博客、媒体文章和 StackOverFlow 回答中的一些实现错误地更新了状态。一些倒计时实现将存储剩余秒数，然后**将使用`setInterval`每 1000 毫秒递减**剩余秒数。这将很快与实际时间*不同步，具体取决于:*

*   选项卡可见多长时间(出于性能原因，浏览器会降低`setInterval`调用非焦点页面的优先级)
*   应用程序的其余部分有多复杂(如果有大量工作要做，线程可能无法足够快地到达`setInterval`回调)
*   查看设备的性能规格(例如，移动设备执行 JS 会更慢)

如果你想了解更多，研究一下“Javascript 事件循环”——几个好的面试问题都围绕着这个概念。这不是特异反应。

具体到 React，一些实现没有考虑到`this.setState`不会立即更新状态的事实。这意味着在第二状态更新被排队之前，第一状态更新可能还没有被执行。*如果实现依赖于前一状态来决定下一状态*(即时间递增或递减)，这是一个问题，因为时间将失去同步(因为两个重复的更新被排队)。

解决方案是将一个函数传递给`this.setState` [，如 React 文档](https://reactjs.org/docs/state-and-lifecycle.html#state-updates-may-be-asynchronous)所示，或者(更好的解决方案)不依赖于之前的状态来获取当前状态。在`setInterval`回调函数中，用最近的时间更新状态。

# 没有清理工作完成

在一个类组件中，`componentWillUnmount`方法可以使用类似于`clearInterval(this.state.intervalId)`的东西来清理区间 id。然而，一些实现忽略了这个重要的细节，这可能会导致复制实现的人的代码中出现内存泄漏。

# 我的生命周期方法是巨大的(“它不是钩子”)！

如果一个 React 组件变得很大(也许您继承了坏代码)，那么从`setInterval`调用到`clearInterval`调用以及从`this.state`访问当前时间可能需要相当多的滚动。这违反了我的一条个人经验法则:

**有关联的事物，应该是彼此接近的。**

顺便说一下，这是 React 中切换到钩子的动机。

我还没有找到一个钩子来消耗和更新当前时间。所以，请享受我的实现。

顺便说一句，这个“相关的事物应该彼此靠近”的经验法则是我个人最喜欢的法则之一的特例(如果完全采用，它会有生命/道德的含义):

**让做正确的事情变得容易；让做错事变得困难。**

要了解这种哲学的更多内容，请查阅我的一篇早期文章(嘿，你已经读到这里了！):

[](https://medium.com/@james.patrick.fulford/what-to-do-when-things-go-wrong-4eaec952d157) [## 出问题时该怎么办

### 错误是自然而然的。改善不会。

medium.com](https://medium.com/@james.patrick.fulford/what-to-do-when-things-go-wrong-4eaec952d157)