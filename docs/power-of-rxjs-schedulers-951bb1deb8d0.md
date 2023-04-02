# RxJS 调度程序的能力

> 原文：<https://javascript.plainenglish.io/power-of-rxjs-schedulers-951bb1deb8d0?source=collection_archive---------1----------------------->

![](img/53553e4c4e877da337a5c29ef4f95101.png)

Photo by [Content Pixie](https://unsplash.com/@contentpixie?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

你对 RxJS 中的调度器了解多少？他们对开发人员隐藏了 Observable 的执行上下文。就像《哈利·波特》中的家养小精灵，他们在霍格沃茨做所有的粗活，甚至没有人听说过他们。让我们解决这个问题，并进一步了解他们。

# 什么是调度程序？

> *一个* `*Scheduler*` *让你定义一个* `*Observable*` *在什么执行上下文中向它的* `*Observer*` *发送通知。*

换句话说，一个`Scheduler`在`Observable`中管理操作的顺序和执行时间。下面的例子将帮助我们理解它是如何工作的。

如您所见，代码是同步执行的。但是如果我们希望`Observable`异步运行，我们需要添加带有特殊`Scheduler`的`observeOn`操作符。

现在`Observable`异步提供数据。酷毙了。就好像我们把它包装在`setTimeout`函数里，不是吗？顺便说一下，我们在代码中使用了`asyncScheduler`。这是传说中的调度之一。

# 调度程序类型

您需要知道 JavaScript 中的事件循环是如何工作的，以便理解调度器的强大功能。可以用[这个视频](https://youtu.be/8aGhZQkoFbQ)来刷新一下你的知识。

总之，浏览器有自己的代码执行顺序:

1.  同步代码(调用堆栈)
2.  微任务队列(承诺)
3.  宏任务队列(setTimeout、setInterval、XMLHttpRequest 等。).
4.  在下一次浏览器重画之前要执行的代码队列(requestAnimationFrame)

RxJS 对这些项目都有一个`Scheduler`:

*   `queueScheduler`同步执行任务
*   `asapScheduler`微任务队列中的计划
*   `asyncScheduler`宏任务队列中的调度
*   `animationFrameScheduler`在下一次浏览器重画之前，在要执行的代码队列上进行调度

还有`VirtualTimeScheduler`和`TestScheduler`，用于测试。点击阅读相关内容[。](https://medium.com/angular-in-depth/so-what-is-rxjs-virtualtimescheduler-796e92ed722f)

看一下下面的代码。

如你所见，`“queueScheduler”`同步工作，因为它在`“synchronous code”`之前。并且`“asapScheduler”`出现在`“asyncScheduler“`之前，因为它使用了微任务队列。

# 如何使用调度程序？

您可以使用带有`**observeOn**`和`**subscribeOn**`操作符的调度程序。两者都接受第一个参数`Scheduler`，第二个参数`delay`，默认为零。

> *使用* `*subscribeOn*` *来调度* `*subscribe()*` *调用会在什么上下文中发生。
> 使用* `*observeOn*` *来安排在什么上下文中发送通知。*

有趣的事实——无论您使用什么，`Scheduler`都会以大于零的延迟回落到`**asyncScheduler**`。遵循代码是没有用的。

在 RxJS 6.5.0 之前，您可以为`of`、`from`、`merge`、`range`等添加一个调度器作为第二个参数。在 RxJS 的新版本中，这种行为已被否决，您应该使用`**scheduled**`函数来实现这一点。

# 调度程序用例

在处理 RxJS 时，我们通常不会考虑调度程序，因为库作者在抽象这种逻辑方面做得很好。但是有些时候使用`Scheduler`会有帮助，你应该为此做好准备。

首先，如果我们需要用`Observable`制作流畅的动画，我们应该使用`**animationFrameScheduler**`。

我还有一个来自我的实践的例子。我不得不为不同的`*id*`请求开发缓存。我写了这样一个函数:

这个代码是有效的。但是有一个问题。第一次使用我的函数时，它异步返回值，第二次同步返回值。

对于消费者来说，一个功能的行为如此不可预测，这是一个悲剧。不幸的是，我发布了 [Zalgo](https://medium.com/@bluepnume/intentionally-unleashing-zalgo-with-promises-ab3f63ead2fd) 🔥。

让我们添加带有`asyncScheduler`的`scheduled`函数来修复这个问题。

伟大的扎尔戈被放逐了。是啊。

# 结论

调度器控制可观察操作的时间和顺序执行。四分之一的 RxJS 操作员在引擎盖下使用它们。最有可能的是，关于它们的知识在实践中没有帮助。但是你永远不知道调度程序什么时候会派上用场。

我很想听听你对调度的经验。感谢关注！