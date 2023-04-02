# 调试 JavaScript 的最佳实践

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-debugging-7581efbd3f2f?source=collection_archive---------13----------------------->

![](img/c7cfbba8e5f076cbef0e8eff88a4463a.png)

Photo by [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。编写可以运行但存在问题的代码很容易。

在本文中，我们将看看如何使调试 JavaScript 程序更容易。

# 调试辅助工具

为了解决程序中出现的问题，我们可以在 JavaScript 程序中引入调试工具，该程序可以在程序的开发版本中运行。

在 JavaScript 中，有`debugger`语句、`console`方法、调试器和`assert`模块。

`debugger`语句暂停一个 JavaScript 程序，以便我们可以检查变量的值。

`console`方法可以以我们喜欢的方式记录变量值，包括颜色、表格、甚至样式。

一些编辑器和 ide 比如 Visual Studio Code 也有内置的 JavaScript 调试器。

`assert` Node.js 模块也非常适合检查断言是否满足，如果不满足就抛出错误。

此外，日志记录对于跟踪错误源也很有帮助。

# 尽早引入调试工具

我们应该尽早引入它们，以便尽可能多地寻找误差源。

# 使用攻击性编程

我们不应该轻易忽略错误。所以我们不应该让开发人员绕过错误，这样他们就可以被检查和修复。

此外，我们可以使用 case 语句或 else 块来关闭我们的程序，这样我们就不会错过遇到的错误值。

错误日志也可以通过电子邮件发送给开发人员，以便他们查看。

错误报告工具可用于检查任何环境中出现的错误。

# 移除调试帮助

在将我们的程序发布到生产环境之前，我们必须移除调试辅助工具。

我们可以通过环境变量来做到这一点，根据环境来切换它们的开关。

只要确保在开发过程中这些辅助工具是打开的，而在生产过程中是关闭的。

# 使用调试存根

我们还可以添加只在开发环境中运行的代码，这样它们有助于更容易地调试我们的代码。

# 确定在生产代码中保留多少防御性编程

当我们的代码发布到生产环境中时，我们可能希望删除一些错误检查代码。

有一些指导方针可以帮助我们决定是否要删除它。

# 留下检查重要错误的代码

如果一个错误太重要而不能忽略，那么我们应该检查它们。

检查无效输入之类的代码应该包含在任何地方，这样我们就不会保存坏数据。

我们必须添加检查以确保我们的输出是正确的。

# 删除检查小错误的代码

如果有检查错误的代码不会产生很多后果，那么我们可以删除它们。

我们可以使用版本控制、环境变量等。来打开和关闭它们。

# 删除导致硬崩溃的代码

硬崩溃并不能给用户提供良好的体验。因此，我们应该清除它们。

我们应该更好地处理它们，以便优雅地处理它们，而不是使我们的程序崩溃。

![](img/9c770c2025c76876c4dd1dd6f093f703.png)

Photo by [Christina @ wocintechchat.com](https://unsplash.com/@wocintechchat?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 留下有助于程序正常崩溃的代码

上一点的推论是，我们应该让代码优雅地处理错误。

他们处理错误的方式不会让用户觉得是不好的体验，所以我们可以把错误保留下来。

# 为我们的技术支持人员记录错误

日志记录很重要，因为我们最终会遇到生产问题。

我们确保我们有可以检查的日志来追踪错误的原始来源。

Morgan 是 Node.js 应用程序的优秀记录者。

# 确保我们留下的错误消息是友好的

含糊不清的错误消息是不好的。

我们应该确保它们是有用的，这样我们就可以用它们来修复我们遇到的错误。

此外，我们可以为用户添加联系电话或电子邮件，以便联系他们寻求帮助。

# 结论

我们应该有辅助工具来帮助我们在开发环境中进行调试，以便我们可以使用它来排除故障。

此外，我们可以取出一些错误处理代码，用于处理小错误或导致用户体验不佳的错误。

日志记录总是有用的，这样我们就可以不费力地追溯错误的源头。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****