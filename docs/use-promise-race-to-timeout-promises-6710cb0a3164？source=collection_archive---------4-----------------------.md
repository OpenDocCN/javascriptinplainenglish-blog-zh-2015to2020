# 使用 Promise.race 超时承诺

> 原文：<https://javascript.plainenglish.io/use-promise-race-to-timeout-promises-6710cb0a3164?source=collection_archive---------4----------------------->

## 解决。拒绝。但是两者都不是呢？

![](img/d2568f80eb099c136eff5db7638abb2c.png)

Race your promise against the clock. Photo by [Jonathan Chng](https://unsplash.com/@jon_chng?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/race?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

承诺是争论异步 JavaScript 的好方法。当他们解决或拒绝，你可以很容易地看到这两个结果是如何处理的承诺。

然而，异步任务有 3 种场景:解决、拒绝、**或者都没有**。后一种情况，加上延迟的解析/拒绝，在我们的 JavaScript 代码中经常没有考虑到，也许是假设底层库足够成熟，最终可以拒绝耗时太长的操作。然而，情况可能并非如此——如果不是这样，所需的处理可能与任何其他错误都不完全相同。

例如，考虑慢速网络请求、数据库操作或加载脚本。根据您的使用情况，您可能希望使用不同的主机重试请求，通知 SLA 跟踪器，或者通知第三方 UI 开发人员他们的 web 组件加载时间过长。

注意，许多成熟的库将超时场景转换为拒绝场景，所以首先要注意遵循它们的文档。下面的代码演示了如何将它添加到您的库中，或者使用它来包装缺少这种处理的库。

# 示例代码

这个例子试图在浏览器中加载一个脚本，但是如果脚本在一段时间内没有加载，就会触发`catch`处理程序，从而将 3 个场景简化为更典型/更明确的 2 个场景`resolve`和`reject`。

该代码块的关键要点是:

*   承诺式的超时可能非常简洁。
*   您可以使用 Promise.race 编写带有超时的承诺，以便有效地对原始承诺设置时间限制。
*   您的超时特定逻辑可以在超时承诺上的一个`.then`中。(上面的例子将超时场景转换为更常见的拒绝场景；然而，你的逻辑可能不同。)

# 警告:清理

超时不会导致另一个承诺被清除或取消。例如，如果一个数据库写承诺是针对超时的`Promise.race`，并且如果超时首先完成，那么数据库写操作**将仍然运行**并且**可能(最终)成功**，但是应用程序的其余部分将认为它失败了。确保引入逻辑来取消操作，以防应用程序启动超时。

处理清理和取消逻辑是一个棘手的问题，因为这个特性没有内置到 JavaScript 中。我已经为这个主题专门写了一篇文章:

[](https://medium.com/javascript-in-plain-english/canceling-promises-in-javascript-31f4b8524dcd) [## 在 JavaScript 中取消承诺

### 或者:如何取消那封尴尬的邮件

medium.com](https://medium.com/javascript-in-plain-english/canceling-promises-in-javascript-31f4b8524dcd) 

# 摘要

电影《公牛达勒姆》中有一段很好的、灵活的引用:

> 有时你会赢，有时你会输，有时会下雨。

你的代码考虑了承诺解决，你的代码可能考虑了拒绝，但是你考虑过“下雨”的可能性吗——既不是赢也不是输？也许你使用的库可以处理这些，但是如果你必须自己做，用 Promise.race 和你的承诺赛跑。如果可以的话，确保清理承诺。