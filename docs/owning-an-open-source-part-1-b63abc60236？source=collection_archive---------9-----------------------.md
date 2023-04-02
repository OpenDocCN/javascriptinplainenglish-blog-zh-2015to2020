# 开源系列:简介

> 原文：<https://javascript.plainenglish.io/owning-an-open-source-part-1-b63abc60236?source=collection_archive---------9----------------------->

![](img/c5ec74e30479bda659bdd977eb5e01a0.png)

## 或者旅程从哪里开始

在我的 [*个人博客*](https://www.justjeb.com/blog) *上可以免费获得这篇文章和其他文章。请务必注册以获得最新最棒的！*

你有没有想过拥有自己的开源项目？我打赌你有——你正在读这篇文章。也许你现在正在思考这个问题。也许你来这里是为了更好地了解会发生什么，你会面临什么样的挑战，以及如何应对它们。嗯，你来对地方了。

在接下来的系列文章中，我将分享我拥有一个开源项目的个人经历。拥有。不参与开源项目，但拥有一个开源项目。有很大的不同，在这个系列中，我们将了解其中的原因。

# **目录**

*   **简介**
*   [开始一个项目](https://medium.com/@justjeb/owning-an-open-source-project-part-2-2b55810aeb8)
*   [文档](https://medium.com/@justjeb/open-source-series-documentation-96ed1420ce81)
*   [宣传](https://medium.com/@justjeb/open-source-series-publicity-8b3be7d65c17)
*   [议题和减贫战略](https://medium.com/@justjeb/open-source-series-issues-and-prs-8cb1de880fd4)
*   [自动化](https://medium.com/@justjeb/open-source-series-automation-fe826e365b54)
*   [版本管理](https://justjeb.medium.com/open-source-series-version-management-dc91424aa63d)

# 我是谁

我叫 JeB，几年来我一直在维护一些开源项目。最受欢迎也是我学到最多的是 [@angular-builders](https://github.com/just-jeb/angular-builders) 。在写这篇文章的时候，它在 Github 上有大约 600 颗星，每月下载量大约 40 万次。

是的，它甚至比不上 Angular 或 React 这样的大型项目，但我觉得我有足够的经验与你分享，以帮助你避免犯我犯过的同样的错误，更重要的是，理解拥有一个开源项目的成本。

# 那么什么是开源呢？

让我们首先建立一种通用语言，并在关键术语和定义上达成一致。
什么是开源？这是一个非常通用的[定义](https://en.wikipedia.org/wiki/Open_source_(disambiguation))，来自一个众所周知的*开源*百科全书(又名维基百科):

> [开源](https://en.wikipedia.org/wiki/Open_source)是允许复制或修改对公众开放的信息的概念。

或者，就[软件开发](https://en.wikipedia.org/wiki/Open-source_model)模式而言:

> 开源模式是一种分散的[软件开发](https://en.wikipedia.org/wiki/Software_development)模式，鼓励[开放协作](https://en.wikipedia.org/wiki/Open_collaboration)。
> 
> [开源软件开发](https://en.wikipedia.org/wiki/Open-source_software_development)的一个主要原则是[对等生产](https://en.wikipedia.org/wiki/Peer_production)，产品如源代码、[蓝图](https://en.wikipedia.org/wiki/Blueprint)和文档免费向公众开放。

在维基的例子中，我们有编辑文章的人(*贡献者*)和批准编辑的人(更有经验的成员，版主)。如果我们把它投射到软件世界，编辑们将会组成一个开源项目的核心团队，贡献者也将会是贡献者。

维基百科是一个巨大的开源项目，但这一切都是从小事开始的。维基百科诞生于 Nupedia，它的诞生是有原因的:

> 尽管有感兴趣的编辑的邮件列表，有全职主编[拉里·桑格](https://en.wikipedia.org/wiki/Larry_Sanger)、威尔士雇佣的哲学研究生在场，但 Nupedia 的内容写作极其缓慢，第一年只写了 12 篇文章。

所以第一个问题来了:

# 为什么要费心开源呢

从前面的阅读中你可以想象，向更广泛的受众开放的主要原因之一是*获得合作者。*

> 我们一起强大
> (扎里亚，2016)

在撰写本文时，维基百科[拥有](https://en.wikipedia.org/wiki/Wikipedia:Wikipedians) 37，899，499 个注册账户，其中 134，022 个正在积极编辑。

随便想想…***134022 活跃合作者。*** 哦，还有它[有](https://en.wikipedia.org/wiki/Special:Statistics) 6M 内容页面！

如果 Nupedia 没有转向开源模式，这些数字还会一样吗？我非常怀疑。

谈到软件，没有什么不同。为了解决某个问题，你必须写代码。为了解决一个大问题，你必须写很多代码。为了正确地解决这个问题，你必须编写高质量的代码，做出高质量的设计等等。所有这些*都需要资源，*说实话，你没有。毕竟你有房租要付。

虽然获得合作者是一个合理的激励，但几乎没有人仅仅因为它而开始一个新的开源项目。显然原因可能不同，但让我们谈谈最受欢迎的原因。

# 小心你的愿望

我想到的几个场景是:

1.  你面临一个问题，外面没有任何东西可以为你解决它(或者有一些东西但是要花钱)，所以你必须自己解决它，你设法解决它，你对你的工作非常兴奋，你认为其他人可以从中受益。
2.  你想被认为是一个开源项目的创始人，你想在你的简历中加入这一行，你有一个自我(毕竟我们都是人)。如果这是你的主要原因，那么我向你保证，读完这个系列，你会重新考虑——这不值得。
3.  你面临一个问题，有一个开源项目实际上为你解决了它，但是它不够好(在你看来)或者没有你需要的确切特性。如果你仅仅因为这个而创建一个新的开源，那么*很可能*你处于第二位(自我)。让自己成为一个贡献者，为现有的项目创建一个 PR。
    *如果现有项目有不同的愿景，并且制作 PR 不是一个选项，您应该考虑通过在您的项目中重用其功能来扩展它，或者* [*分叉*](https://help.github.com/en/github/getting-started-with-github/fork-a-repo) *它。以后可能会省去你很多头疼的事。*
4.  你面临一个问题，外面没有任何东西可以为你解决它，你认为从一开始就把解决方案作为开源是一个非常好的主意。
    不是的。解决问题，确保它对你有用，然后去做第一件事。

这是我发现的创建一个新的开源项目的 4 个最受欢迎的激励，但是在这个系列中，我们将主要关注场景#1。

原因很简单——我相信如果开始一个开源项目的主要原因不是渴望分享和贡献，那么这个理由就站不住脚了。
很长一段时间以来，你帮助别人的事实可能是你得到的唯一回报。如果这不是你想要的那种满足感，那么也许你应该就此打住，不要浪费时间。

*还有一个非常流行的场景值得一提。
有些公司向社区开放部分代码。
比如 Angular(谷歌维护)、React(脸书维护)、VSCode(微软维护)等等。他们这样做的原因可能各不相同，但赢得合作者和为社区做贡献肯定是其中之一。
虽然我不能否认这种实践的重要性，但是这种场景与其他场景有很大的不同，因为维护这种项目* ***的员工可以从他们的工作中获得报酬*** *。
如果你在一家考虑创建开源项目的公司工作，这里的大部分内容对你来说仍然是相关的，但是动机可能会有所不同。*

# 包装它

如果我必须用一句话来总结这一部分，那就是:

> 确保你的意图符合你的期望

相信你想拥有一个开源项目并不等同于实际拥有一个，你将在下面的文章中看到这一点。

感谢您的阅读，如果您不想错过下一部分，请确保您在这里或在 [Twitter](https://twitter.com/_Just_JeB_) 上关注我，在下一部分中，我们将实际讨论 [**开始一个开源项目**](https://medium.com/@justjeb/owning-an-open-source-project-part-2-2b55810aeb8) 。