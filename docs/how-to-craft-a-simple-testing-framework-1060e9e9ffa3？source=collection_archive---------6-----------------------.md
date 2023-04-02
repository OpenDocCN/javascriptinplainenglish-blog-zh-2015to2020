# 如何设计一个简单的测试框架

> 原文：<https://javascript.plainenglish.io/how-to-craft-a-simple-testing-framework-1060e9e9ffa3?source=collection_archive---------6----------------------->

## 开始清理你的垃圾

## 立即测试您的代码！

![](img/94c91dfcfed9b1b8863a7a0912524c88.png)

Photo by [Science in HD](https://unsplash.com/@scienceinhd?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](/s/photos/testing?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

> 质量意味着在没人看的时候做对。
> 
> —亨利·福特

*你可能想知道这个故事如何帮助你……*

**这个故事的目的是帮助你采用正确的心智模式。**你必须理解为什么我们要测试我们的应用程序，为什么自动化测试与手动测试大致相同。重要的一点:**不要把你的 QA 同事视为技术冗余。在这个故事的结尾，你会发现为什么这是错误的一步！**

我不想打击你的士气，但我正试图增加你的信心，让你相信你的测试主题正以应有的方式运行。你必须意识到，不是所有的测试都提供相同程度的信心。如果你没有正确地做事情，你可能已经浪费了你的时间，并给了错误的安全感。那比一点测试都没有更糟糕。

我不再通过谈论无用的知识来产生废料。我们开门见山吧！

# 一个关于虫子的故事

![](img/64304df2c48b358bb16f81e8621dd5ae.png)

The first actual case of a bug being found (source: [Wikipedia](https://en.wikipedia.org/wiki/Software_bug#/media/File:H96566k.jpg))

对上面的图片有一个很大的误解。这是第一例窃听器。准确地说，这是有史以来第一次报道这种病毒。信是来自[海军](https://www.nrl.navy.mil/)实验室的！

这并不是第一次使用术语“bug”来描述系统中的问题。托马斯·爱迪生甚至在 19 世纪就使用了这个术语。他用“bug”这个词来定义这封信中的一个技术故障，这封信是寄给西方联合公司的总裁威廉·奥顿的，信中向他介绍了他们就一种新的电话设计当面交谈的最新情况:

> “你说对了一部分，我的确在我的仪器里发现了一个‘窃听器’，但不是在电话本身。它属于“callbellum”这种昆虫似乎能在所有的电话呼叫装置中找到生存的条件。"

今天，**这个 bug 与系统中表达的一个问题**有关。我们为我们的业务逻辑写了很多代码，我们一直在软件中发现 bug。令人着迷的是，每个软件都很流畅，所以 bug 可以在我们背后偷偷摸摸。

技术每天都变得越来越复杂，因此 bug 也越来越常见。我们今天发现的虫子比以前在真空管或 T2 继电器中发现的虫子(飞蛾和其他昆虫)多得多。

# 我们如何区分虫子？

随着系统复杂性的增加，各种各样的错误也在增加。我将列出最常见的错误:

*   安全漏洞，
*   内存泄漏，
*   回归 bug，
*   业务逻辑错误，
*   可访问性缺陷，
*   逻辑错误，
*   集成错误，
*   一个误差，
*   竞争条件脆弱性，
*   空指针异常(运行时异常)等。

这些是一些最常见的错误，但我不认为你可以创建一个完整的错误列表。**错误会出现在我们软件的所有领域。**使用 JavaScript 时，你有两种策略可以选择:

1.  深度理解 JavaScript 引擎，这是我推荐的。
2.  使用静态类型系统，如[类型脚本](https://www.typescriptlang.org/)或[流程](https://flow.org/)。如果你利用静态类型，它将会带走你的软件中可能出现的一整类错误。

给你的 JavaScript 代码添加静态风格也会**让你花更多的时间来写 JavaScript** ，因为有时候给 JavaScript 添加类型会令人沮丧。但总的来说，它让你的软件变得更好。

我个人不使用类型检查器，因为我选择了解系统。你可以很酷，用棉绒。ESLint 是一个很好的工具。如果你正在使用林挺格式化你的代码，请停止它！有一个很好的工具叫做[更漂亮的](https://www.npmjs.com/package/prettier)。

**ESLint 的主要目标是捕捉那些偷偷通过静态类型检查器的常见错误。**大量使用 linter 来解决特定领域的事情。林挺是另一种形式的测试。ESLint 可以捕捉最危险的 bug——**逻辑 bug**。

## 机翼试验类型

最后，我们可以定义我们在应用程序上执行的自动化测试的综合列表:

*   [单元测试](https://en.wikipedia.org/wiki/Unit_testing)，
*   [集成测试](https://en.wikipedia.org/wiki/Integration_testing)，
*   [端到端测试](https://www.guru99.com/end-to-end-testing.html)，
*   [回归测试](https://en.wikipedia.org/wiki/Regression_testing)，
*   [验收测试](https://en.wikipedia.org/wiki/Acceptance_testing)，
*   [性能测试](https://en.wikipedia.org/wiki/Software_performance_testing)，
*   [无障碍测试](https://www.guru99.com/accessibility-testing.html)，
*   [压力测试](https://en.wikipedia.org/wiki/Stress_testing_(software))，
*   [渗透测试](https://en.wikipedia.org/wiki/Penetration_test)，
*   [国际化(本地化)测试](https://www.softwaretestinghelp.com/localization-and-internationalization-testing/)。

# 编写你的第一个简单的测试框架！

我们使用测试框架有两个主要原因:

1.  加快我们的工作流程，
2.  我们在进行更改时不会破坏现有的代码。

*注意:如果你想跟着编码，你得已经安装了*[*NodeJS*](https://nodejs.org/)*(服务器环境)和*[*Jest*](https://jestjs.io/)*(*[*NPM*](https://www.npmjs.com/package/jest)*或者*[*Yarn*](https://classic.yarnpkg.com/en/package/jest)*)。*

至少我们开始建立我们的极简框架。我们从简单开始。我将在测试用例中使用严格相等，因为我想防止强制和严格。现在我要写一个简单的测试:

`**actual !== expected**` **叫做断言。**是用代码说一件事应该是某个值或者通过某个测试的一种方式。

注意:我在测试中使用 Jest，除了最后两个测试。

在我们继续之前，让我们考虑几件事情。**测试是一个代码，当某件事情的实际结果与预期结果不匹配时，它会抛出一个错误。**在下一个例子中，我将展示`tf_first.test.js`测试的输出:

这是一次热身练习。在接下来的几个例子中，我们将使用一个非常简单的模块`tf_math.js`:

测试像我们的`tf_math.js`模块中的那些**纯函数**相对容易。对于那些不知道的人来说，纯函数是一个对于给定的输入连续返回相同输出的函数。更重要的是，它不会改变周围世界的状态。

我们将开始测试`tf_math.js`模块。让我们编写第一个测试:

考验会过去的。但是如果我们破坏 sum 函数，用星号(*)改变加号(+)，测试将会失败:

每个框架的关键特性是控制有用的错误消息。当测试失败时，我们首先检查的是错误消息。如果错误消息很糟糕，我们将花 4 到 7 倍的时间来查看代码，以了解测试失败的原因。

当你选择一个测试框架(或者断言库)的时候，选择一个有好的全面的错误消息的框架。我提到了断言库。NodeJS 有自己的(我会在故事的最后展示用法)，但我打算自己动手:

测试失败是因为我们中断了函数(在前面的示例中):

我们这里有个问题。如果我也破坏了第二个函数，系统将只记录我破坏的第一个函数。我们不会知道第二个功能`subtract(34, 22)`也坏了。控制台与上一个示例中的相同。我们需要对助手函数做一点重构:

我将运行`node __tests__/tf_fourth.test.js`，因为我不能用我的自定义测试函数运行 Jest(因为 Jest 有它的测试函数实现):

现在，我将注释掉`test()`函数并运行 Jest:

如果你想的话，最好使用 Jest CLI，你可以构建自己的 CLI。选择四个步骤来构建简单的测试框架。正如我所承诺的，我将向您展示如何使用 [NodeJS 断言库](https://nodejs.org/api/assert.html):

CLI 很简单:

## *结论*

*当我们处理依赖于其他状态的函数时，会变得格外复杂，我们需要先设置状态。例如，在 React 中，组件需要首先被渲染，然后我们可以使用它们的状态。*

你可以在[这个链接](https://github.com/alenvlahovljak/simple-testing-framework)克隆这个库。

## 坦白地说

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**