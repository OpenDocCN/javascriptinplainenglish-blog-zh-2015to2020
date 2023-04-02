# 如何摆脱前端 bug

> 原文：<https://javascript.plainenglish.io/how-to-get-rid-of-frontend-bugs-c09a64ceee9a?source=collection_archive---------12----------------------->

## 自动化测试是不够的

作为专业的软件开发人员，我们的工作是交付高质量、无错误的软件，但遗憾的是这不太可能。除非你正在开发一个非常简单的软件，否则你会遇到错误。

然而，这并不意味着我们可以忽视 bug 对业务和用户的巨大影响。在这张[信息图](https://www.globalapptesting.com/blog/how-bugs-impact-your-company-infographic)中，你可以找到更多关于软件漏洞平均成本的数据。

> 48%的用户如果对应用的性能不满意，就不太可能再次使用该应用

在这篇文章中，我们将探讨发生在前端的几种类型的错误，以及如何最小化错误数量的方法。

![](img/f6d8bc1b39e05f3f432057ba51967369.png)

Photo by [Gabriel Gabriel](https://unsplash.com/@osomax?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 前端 bug 有多严重

随着用户期望的提高，前端系统不再被视为虚拟接口。

相反，前端支持不同的设备、屏幕尺寸，为用户提供愉快的 UX，与后端系统集成，并处理诸如内部化、验证、转换等功能。

由于所有这些需求，出现了一些技术挑战，例如，状态管理、组件通信以及维护数十或数百个组件。

这导致复杂性不断增加，因此出现了各种各样的错误，包括但不限于此。

**内容漏洞:**如果处理不当，缺失或无效的数据可能会导致错误，如图像损坏、链接到 404、空工具提示和错别字。

**功能缺陷:**不提交的表单，无所作为的按钮，没有实现的需求，500 个页面阻止用户达到目标。

视觉错误:当界面看起来有问题时，有时*(溢出)*也被认为是功能错误。文本溢出、对齐和裁剪界面都对用户体验有很大影响。

**易访问性缺陷**:经常被忽视的、流行的缺陷包括键盘导航问题、缺少替代文本和颜色对比问题。

**国际化错误**:众所周知的 RTL 问题、错误的货币、缺失的内容、截断和日期格式。

**性能缺陷:**图像加载时间太长，或者更糟糕的页面直到用户离开后才呈现。

浏览器兼容性错误:所有的 IE 迷因和笑话都不是无缘无故的。有时候，某些功能在某些浏览器上可以工作，但在其他浏览器上就不行了。

安全漏洞:通常被认为是后端问题，但在前端，你也受到安全威胁。特别是，当你使用第三方注入的 javascript 与你自己的 javascript 功能相同时。

**可用性建议:**用户可能会面临一个具有挑战性的用户体验，他们认为该功能实际上不起作用，因此也会停止使用你的应用程序。

# 预防技术

鉴于开发过程中出现的错误是不可避免的，我们的目标不应该仅仅是消除它们。相反，我们应该开发方法和手段来帮助我们尽快地识别、调试和修复缺陷。

考虑到这一点，bug 可能会变得很少，我们非但不会感到不安，反而会兴奋地发现它，并将其视为进一步改善客户体验的机会。

# 1.监视

了解用户面临的错误和事件是消除这些错误的重要一步。

> 如果你不能衡量它，你就不能改善它。—彼得·德鲁克

*   有几个工具可以让你监控你的错误，比如[哨兵](https://sentry.io/signup/)、 [Bugsnag](https://www.bugsnag.com/content/javascript-error-monitoring-best-practices) 或[空气制动](https://airbrake.io/)，它们可以很容易地集成到你的应用中。
*   使用 [Instabug](https://instabug.com/) 和 [Usersnap](https://usersnap.com/) 等工具为你的用户提供沟通渠道，可以给你很好的反馈和见解。
*   记录日志，不仅记录错误和异常，还记录信息和警告，因为它们为您提供有价值的信息，让您了解用户的行为。

# 2.CI/CD

Martin Fowler 更好地解释了 CI/CD 的几个优点。在消除 bug 的背景下，它不仅强迫你进行自动化测试，还允许你更快地发货。

CI/CD 可以极大地减少您的平均解决时间(MMTR ),因为小的代码变更更容易推理，并且检查潜在的缺陷变得没有痛苦。

考虑到您可以添加更准确的阳性和阴性测试，这也将增加您的测试的可靠性。

这将导致更低的修复错误的成本和更满意的客户。下面是前端开发人员对 CI/CD 的一个很好的介绍

[](https://blog.logrocket.com/from-front-end-developer-to-a-devops-an-intro-to-ci-cd-7a8a8713fb34/) [## 从前端开发人员到开发人员:CI/CD - LogRocket 博客简介

### 对于所有有抱负的前端开发人员来说，2019 年是一个真正令人惊叹的时刻。有大量的教育材料，课程…

blog.logrocket.com](https://blog.logrocket.com/from-front-end-developer-to-a-devops-an-intro-to-ci-cd-7a8a8713fb34/) 

# 3.代码质量

高代码质量意味着缺陷数量较少。随着时间的推移，人们开发了无数的方法来实现这一目标。

其中一些是 TDD，结对编程，模块化你的代码库，不断地执行代码审查，K.I.S.S，DRY，故事书，重构等等。

重要的是，在编写代码时要小心，不要浪费时间，避免“让它工作”,然后继续心态，而是瞄准[让它工作，让它正确，让它快速](https://tknilsson.com/2018/05/25/programmer-friday-make-it-work-make-it-right-make-it-fast/)

# 4.静态测试

那就是不用运行代码就能发现缺陷的过程。它主要分为两个阶段:静态回顾和静态分析。

**评审:**通常需要人工输入，它包括代码评审，评审测试用例，验收标准，可能对于大的特性，向团队展示一遍

**分析:** [使用简单的工具](https://kentcdodds.com/blog/eliminate-an-entire-category-of-bugs-with-a-few-simple-tools)你可以执行分析来消除许多 bug，而无需运行你的代码。

## **4.1 类型检查(流/类型脚本)**

拥有类型 [**并不能**](https://medium.com/javascript-scene/the-shocking-secret-about-static-types-514d39bf30a3) 消除所有的 bug，错误地使用会给人一种错误的安全感。然而，它消除了一些子类的错误，如错别字或流行。

`Uncaught TypeError: Cannot read property '..' of undefined`

`Uncaught TypeError: x is not a function`

## 4.2 过磅(Eslint/JsHint)

Linters 以及帮助标记编程和风格错误。例如，Linters 可以通过[eslint-plung-react-Hooks](https://www.npmjs.com/package/eslint-plugin-react-hooks)执行钩子的[规则。](https://reactjs.org/docs/hooks-rules.html)

如果你有特定于代码库的规则，你也可以[编写你自己的定制林挺插件](https://medium.com/@bjrnt/creating-an-eslint-plugin-87f1cb42767f)。

# 5.自动化测试

测试软件是减少 bug 数量和保持交付给客户的质量的一个重要且关键的步骤。通常由重复的点击和验证任务组成。

随着你的软件的成熟，你有更多的场景来测试“*特别是多语言应用*”，因此当手动完成时，测试成本会大幅度增加，并且通常会逐渐减少。

自动化测试减少了测试时间，防止了测试过程中的人为错误，鼓励了重构，增加了发布新功能的信心。

如前所述，由于前端有几种类型的 bug，等价地，也有几种测试，如单元测试、集成测试、e2e 测试、快照测试和截图测试

自动化测试也非常适合前端， [Kent C. Dodds](https://medium.com/u/db72389e89d8?source=post_page-----c09a64ceee9a--------------------------------) 有几篇关于这个话题的惊人的[博文](https://kentcdodds.com/blog/?q=testing)。

# 6.战斗测试

即使有 100%的单元测试覆盖率，你仍然不能避免错误，因为自动化测试不能保证没有错误。让一个人手动检查系统来发现错误是很重要的。出于这个原因，存在几种[类型的软件测试](https://devqa.io/qa/types-of-testing)。

通过让你的团队投入一些时间，疯狂地对你的系统进行模糊测试、探索性测试或特别测试，来对你的软件进行战斗测试。

# 7.所有权和激情

在所有权和软件质量之间有一个积极的[关系，因此对你写的代码有高度的所有权感是很重要的。再加上激情，你就有了为你自己和你的用户带来巨大乐趣的秘诀。](https://docs.microsoft.com/en-us/azure/devops/learn/devops-at-microsoft/code-ownership-software-quality)

我希望您现在已经意识到了在实现新特性时要寻找的几种类型的错误。和一些帮助您减少 bug 数量的技术。

你一直在使用其他技术吗？我错过了什么，请在评论中告诉我，或者在 twitter 上联系我。

## **用简单英语写的 JavaScript 笔记**

我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到[**submissions@javascriptinplainenglish.com**](mailto:submissions@javascriptinplainenglish.com)给我们，我们会把你添加为作者。

我们还推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**