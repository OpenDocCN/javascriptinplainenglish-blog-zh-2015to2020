# 在 JavaScript 中取消承诺

> 原文：<https://javascript.plainenglish.io/canceling-promises-in-javascript-31f4b8524dcd?source=collection_archive---------0----------------------->

## 或者:如何取消那封尴尬的邮件

![](img/467a092f4673f4a5d895413efbdbbc3b.png)

Photo by [Andrej Lišakov](https://unsplash.com/@lishakov?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/red-x?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

你有没有发过一封让你立刻后悔的邮件，或者在发现有问题的时候点击提交？“红 X 在哪里？我能快速停止吗？”，你哭吧！

# 承诺方法

在 Javascript 中，异步行为是用承诺捕获的。虽然它们比回调更干净，并且与语言(async 和 await)深度集成，但 Promises 的方法很少:`then`接受行为成功时运行的回调，`catch`失败时运行回调，`finally`在两种情况下都运行。`cancel` **不在这些标准的许诺方式**之列。

让我们看看如何打破承诺界面，取消行为。

# 必须是可取消的行为

某些操作无法取消。虽然 xhr 可以被`.abort()`d，但是用 Javascript 加载脚本是无法停止的。显而易见，如果没有提供接口来取消您的承诺正在包装的行为，那么您不能扩展您的承诺包装器来允许取消该行为。

如果你不能让火车停下来，你的停车按钮就不会——不能——起作用。这是物理。

# 设计可取消的承诺

我们将返回一个`cancel`回调，而不是仅仅返回一个承诺。调用我们的承诺构建函数的代码可以像正常情况一样处理返回的承诺，但也可以在超时、错误处理或用户调解的情况下调用`cancel`回调。

(你也可以在返回之前在你的承诺上设置一个`cancel`方法，比如`promise.cancel = cancel;`，但是 TypeScript/IDE IntelliSense 可能不理解这种方法。运用你的判断力。)

当构建`cancel`回调行为时，有 4 个场景需要考虑:

1.  `cancel`马上就叫，之前还有什么要清理的。(如果您启动了底层效果，请确保立即取消并清理它。)
2.  `cancel`在执行中间被调用。(清理效果。考虑拒绝或解决承诺，或者让它处于不完整的状态**，如果它适合你的用例**
3.  `cancel`在完成、拒绝或解决承诺后调用。(什么都不做)。
4.  `cancel`在执行中的任意点被多次调用。(后续调用应该什么也不做。)

记住这些场景，让我们来看一个示例实现。

# 可取消承诺示例

尝试将这段代码粘贴到您的浏览器控制台中。(右键单击“检查”，然后单击“控制台”选项卡。如果你用其他方式到达那里，请告诉我！)

捉迷藏是一种游戏，其中一个孩子数数，而其他孩子藏起来。当计数完成后,"寻找者"去找其他的孩子……假设其他的孩子都没有取消上厕所的倒计时。哪些孩子容易这样做。

# 让我们来看看这个例子

*   在有机会开始之前，试着取消**的承诺。您应该看不到任何倒计时数字:`startCountingForHideAndSeek().cancel()`**
*   试着在倒计时的时候**取消承诺。你应该早点看到计数停止，承诺会拒绝:**

```
const during = startCountingForHideAndSeek();during.promise.then(() => console.log('Promise resolved.'));
during.promise.catch(() => console.error('Promise rejected!'));// Wait a bit, but not too long!during.cancel();
```

*   倒计时结束后，尝试取消承诺**。您应该看不到任何事情发生(因为计数已经完成)。**

```
const after = startCountingForHideAndSeek();after.promise.then(() => console.log('Promise resolved.'));
after.promise.catch(() => console.error('Promise rejected!'));// Let the counting finish (10 seconds)after.cancel();
```

*   尝试在任何时候多次调用`cancel`——在执行之前、期间或之后。您应该看到第一次调用导致该行为被取消，但是接下来的调用应该安全地什么都不做。

# 摘要

承诺没有用于中途取消的 API。要构建这个接口，如果您的异步行为支持它，您需要考虑多种场景:执行之前、期间和之后的取消，以及是否重复调用取消。

## **用简单英语写的 JavaScript 笔记**

我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到[**submissions@javascriptinplainenglish.com**](mailto:submissions@javascriptinplainenglish.com)给我们，我们会把你添加为作者。

我们还推出了三种新的出版物！为我们的新出版物献上一点爱心吧，请跟随他们:[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**