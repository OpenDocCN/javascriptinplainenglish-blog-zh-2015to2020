# 为什么应该停止全局安装 npm 软件包

> 原文：<https://javascript.plainenglish.io/why-you-should-stop-installing-npm-packages-globally-1b56b97b70cd?source=collection_archive---------2----------------------->

## 全局安装 npm 软件包几乎没有任何用处

几年前当我开始使用 npm 时，我并不介意在全球范围内安装许多包。如今，我认为在全球范围内安装 npm 包几乎没有任何用处。

![](img/d64a65be7859784617a682f7f2e6b85c.png)

让我告诉你为什么全球安装的软件包不那么好，你应该做什么。

# 全球安装的 npm 软件包的问题

在[之前的一篇文章](https://medium.com/@dSebastien/why-and-how-to-use-stable-node-and-npm-versions-across-your-project-team-a4886f51a96c)中，我解释了为什么冻结 node 和 npm 的版本对任何项目都非常有用。事实上，冻结项目依赖项的版本更加重要。

然而，通常人们似乎只考虑到实际的项目依赖需要一个稳定的版本。公用事业经常被遗忘/忽视。尽管如此，不同的构建/开发工具对于稳定性和限制“它在我的机器上工作”综合症非常重要。

如果构建/运行项目所需的工具不是 package.json 文件的一部分，那么这意味着您必须维护单独的文档。首先，这对任何加入团队的人都会产生摩擦。仅此一点就足以成为列出构建/开发依赖项的理由。此外，很容易忘记更新文档，使其不同步，并可能导致未来的时间浪费。

另一点是，全球安装的软件包就像已安装的应用程序。一段时间后，你会忘记他们为什么会在那里。如果你的一些项目需要它们，那么你就有可能无法再参与这些项目。相反，如果这些包列在项目的依赖项中，那么就没问题了:安装一下，就可以开始了。

# 可供选择的事物

对于依赖 npm 的项目，最简单的替代方法是将所有工具添加到项目的 package.json 文件的 devDependencies 中。这样，无论何时安装项目，您都可以获得所需的所有工具。如果您这样做了，那么在 npm 脚本中使用这些工具就像它们被全局安装一样自然，因为 node_modules/的内容。bin 被添加到脚本可见的“路径”中。

我可能会写一篇关于 npm 脚本的文章，因为我非常依赖它们。他们帮助我为整个团队引入“共享命令”。由于 devDependencies 和 npm 脚本，每个团队成员都可以访问相同的构建/工具/命令，这真的很有价值。对我来说，这就像一个更现代/用户友好的 Makefile-)

当您考虑全局安装包时，您可以做的另一件事是使用 npx。npx 命令与 npm 一起提供，允许轻松执行 npm 二进制文件。

npx 的工作方式如下:它首先检查当前路径(包括本地 node_modules/。bin 文件夹)来查找您试图调用的命令。如果它在“node_modules/”中找到它。bin”，然后执行它(这意味着如果您的构建工具/cli 是您的依赖项的一部分，那么 npx 会找到它)。

如果 npx 在当前路径中(无论是本地还是全局/在 npm 缓存中)没有找到该命令，那么它会在执行它之前尝试安装它。npx 通常在寻找安装内容方面做得很好。如果失败了，您可以使用“- package”标志来帮助它。当然，您可以[为这个](https://itnext.io/bash-aliases-are-awesome-8a76aecc96ab?source=your_stories_page---------------------------)利用 bash 别名；-)

请注意，您可以通过清除 npm 缓存来轻松消除 npx 产生的浪费空间。

TLDR；

*   devDependencies + npm 脚本对稳定性有很多好处，限制了配置偏差，避免了不必要的文档，消除了麻烦，并帮助每个人共享相同的环境
*   npx 可以用来避免全局安装包，您只需要清理 npm 缓存来消除混乱

# 结论

在这篇文章中，我分享了一些关于为什么我几乎从不在全球范围内安装 npm 包的想法。如果你用的是纱线，答案可能会有所不同，但总体思路应该是相似的。全局安装的包就像全局变量:污染！；-)

今天到此为止！

## 喜欢这篇文章吗？点击下面“喜欢”按钮查看更多内容，并确保其他人也能看到！

PS:如果你想学习大量关于软件/Web 开发、TypeScript、Angular、React、Vue、Kotlin、Java、Docker/Kubernetes 和其他很酷的主题的其他很酷的东西，那么请不要犹豫去[拿一本我的书](https://www.amazon.com/Learn-TypeScript-Building-Applications-understanding-ebook/dp/B081FB89BL)并订阅[我的简讯](https://mailchi.mp/fb661753d54a/developassion-newsletter)！

## **来自简明英语团队的通知**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页[**plain English . io**](https://plainenglish.io/)找到所有这些内容——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**