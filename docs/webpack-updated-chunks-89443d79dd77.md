# 挂钩到 Webpack 更新的块

> 原文：<https://javascript.plainenglish.io/webpack-updated-chunks-89443d79dd77?source=collection_archive---------11----------------------->

## 通过仅对已更新的代码段执行某些任务来减少工作量

![](img/ab829982ccee9dd494ca8182e87ebac7.png)

Photo by [Malte Wingen](https://unsplash.com/@maltewingen?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/splitting?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Webpack 是一个帮助我们以模块化方式捆绑所有文件的工具。默认情况下，Webpack 生成一个输出包。在本文中，我们将研究一个场景，其中由于代码分割而生成了多个包。然而，代码分割并不是本文的本质。

# 勾画场景

想象一下，您正在构建一个多品牌应用程序，所有这些都托管在同一个平台上。每次我们构建代码时，我们都必须将它推到这个平台上，以可视化我们的更改。

理想情况下，我们的代码库将被分成几个包，每个品牌单独的包，以及一些公共包。当更新链接到某个包的代码时，Webpack 将确保在查看时只更新那个特定的块。

现在，我们想做一些构建后的步骤，比如将代码推送到平台，但只想将它们应用到更新的块。为此，我们将编写一个小的 Webpack 插件。

# 处理更新块的 Webpack 插件

## 设置

使用以下样板代码为 Webpack 插件创建一个 JavaScript 文件。插件初始化时，将执行构造函数方法。当生成被触发时，将执行 apply 方法。编译器对象将让我们访问所有的构建信息。

processUpdatedChunksPlugin.js

将插件添加到您的 Webpack 配置中。

webpack.config.js

## 查找更新的块

当 Webpack 创建一个块时，它会在旁边保存块的散列。如果块的输出发生变化，哈希也会发生变化。因此，要检查块是否已经更新，我们只需要检查哈希是否已经更改。

因此，在我们的插件中，我们将保留一个块名和散列的映射。然后，我们将过滤掉散列值等于存储散列值的块。对于每个块，我们都存储更新后的哈希。这产生了一个更新块的列表，通过循环这个列表，我们可以为每个块执行一个任务。

现在我们有了一个可以访问我们的块数据的地方，只针对已经更新的块。您可以扩展它来采用回调方法，或者完全定制插件来实现特定的目标。

## 排除故障

在 IntelliJ 中，你可以在你的插件文件中添加断点。接下来，在调试模式下运行 Webpack 脚本，检查里面发生了什么，并检查值。

如果你想检查插件并在你的项目中使用它，检查我的 [npm 包](https://www.npmjs.com/package/process-updated-chunks)。

*更多内容请看*[***plain English . io***](http://plainenglish.io/)*。报名参加我们的* [***免费周报***](http://newsletter.plainenglish.io/) *。在我们的* [***社区***](https://discord.gg/GtDtUAvyhW) *获得独家获得写作机会和建议。*