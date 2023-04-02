# 我如何使用 Node.js 实现工作自动化

> 原文：<https://javascript.plainenglish.io/how-i-made-my-own-youtube-downloader-2a59b44a93c5?source=collection_archive---------6----------------------->

## 通过自动化重复任务来提高性能

![](img/43f38183e000b94bb06cfda4757f1e31.png)

Photo by [Nicole Wolf](https://unsplash.com/@joeel56?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

你知道你在工作中必须做的那些乏味的任务:更新配置文件，复制和粘贴文件，更新吉拉机票。

一段时间后，时间累积起来。2016 年我在一家网络游戏公司工作时，情况就是如此。当我不得不为游戏构建可配置的模板时，这份工作有时会非常有益，但我大约 70%的时间都花在了制作这些模板的副本和部署重新换肤的实现上。

# 什么是 Reskin？

该公司对 reskin 的定义是使用相同的游戏机制、屏幕和元素定位，但改变了视觉美学，如颜色和资产。因此，在一个简单的游戏如“石头剪子布”的背景下，我们将创建一个基本资产模板如下。

![](img/c0596c49833ea048f7d76078e20e8f75.png)

但是当我们创建一个这样的皮肤时，我们会使用不同的资产，游戏仍然可以运行。如果你看看像糖果粉碎或愤怒的小鸟这样的游戏，你会发现他们有许多相同游戏的变种。通常在万圣节、圣诞节或复活节发行。从商业角度来看，这非常合理。现在…回到我们的实现。我们的每个游戏都将共享相同的捆绑 JavaScript 文件，并加载一个具有不同内容和资源路径的 JSON 文件。结果呢？

![](img/fd425fcdbd22f987d68990f3cd8af545.png)

我和其他开发人员把每天的日程排得满满的，我的第一个想法是，“很多事情都可以自动化。”每当我创建一个新游戏时，我必须执行以下步骤:

1.  对模板存储库进行 git 提取，以确保它们是最新的。
2.  创建一个新分支—由主分支中的吉拉票证 ID 标识。
3.  复制一份我需要构建的模板。
4.  大口地跑。
5.  更新 **config.json** 文件中的内容。这将涉及资产路径、标题和段落以及数据服务请求。
6.  在本地构建并检查内容是否与涉众的 word 文档相匹配。*是的，我知道。*
7.  向设计师确认他们对外观是否满意。
8.  合并到主分支，然后继续下一个。
9.  更新吉拉票证的状态，并为相关利益方留下评论。
10.  冲洗并重复。

![](img/43509d5f0733c624218bf1cee15f5d81.png)

对我来说，这感觉更像是行政管理而不是实际的开发工作。我在以前的角色中接触过 Bash 脚本，并利用它创建了一些脚本来减少工作量。其中一个脚本更新了模板并创建了一个新的分支，另一个脚本执行了提交并将项目合并到了试运行和生产环境中。

手动设置一个项目会花费我三到十分钟的时间。可能需要 5 到 10 分钟来部署。根据游戏的复杂程度，可能需要十分钟到半天的时间。脚本很有帮助，但是很多时间还是花在了更新内容或者寻找丢失的信息上。

![](img/03f3a4976871f7a866237ea84a1ef28e.png)

Photo by [Sincerely Media](https://unsplash.com/@sincerelymedia?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

编写代码来节省时间是不够的。我在想一个更好的方法来处理我们的工作流程，这样我就可以更多地利用脚本。将 word 文档中的内容移动到吉拉票证中，将其分解到相关的自定义字段中。设计者不发送指向公共驱动器上资产所在位置的链接，而是建立一个内容交付网络(CDN)存储库，该存储库具有指向资产的暂存和生产 URL，这将更加实用。

# 吉拉 API

像这样的事情可能需要一段时间来执行，但我们的过程确实随着时间的推移而改进。我对我们的项目管理工具吉拉的 API 做了一些研究，并对我正在处理的吉拉门票做了一些请求。我收集了大量有价值的数据。如此有价值，以至于我决定将它集成到我的 Bash 脚本中，以读取吉拉门票的值，并在完成后发布评论和标记利益相关者。

# Bash 转换到节点

Bash 脚本很好，但是如果有人在 Windows 机器上工作，它们就不能运行。在做了一些挖掘之后，我决定使用 JavaScript 将整个过程包装到一个定制的构建工具中。我把这个工具叫做**梅森**，它将改变一切。

# 硬币指示器 （coin-levelindicator 的缩写）命令行界面（Command Line Interface for batch scripting）

当您使用 Git 时，我假设您在终端中使用，您会注意到它有一个非常友好的命令行界面。如果你拼错或输入了一个错误的命令，它会礼貌地对你输入的内容提出建议。一个名为 **commander** 的库应用了相同的行为，这是我使用的许多库之一。

考虑下面的简化代码示例。它正在引导命令行界面(CLI)应用程序。

# src/梅森. js

```
#! /usr/bin/env nodeconst mason = require('commander');
const { version } = require('./package.json');
const console = require('console');// commands
const create = require('./commands/create');
const setup = require('./commands/setup');mason
    .version(version);mason
    .command('setup [env]')
    .description('run setup commands for all envs')
    .action(setup);mason
    .command('create <ticketId>')
    .description('creates a new game')
    .action(create);mason
    .command('*')
    .action(() => {
        mason.help();
    });mason.parse(process.argv);if (!mason.args.length) {
    mason.help();
}
```

使用 npm，您可以从您的 **package.json** 运行一个链接，它将创建一个全局别名。

```
...
"bin": {
  "mason": "src/mason.js"
},
...
```

当我运行项目根目录下的 npm 链接时。

```
npm link
```

它将为我提供一个我可以调用的命令，称为 mason。因此，每当我在终端中调用 mason 时，它都会运行那个 **mason.js** 脚本。所有的任务都归属于一个名为 mason 的保护伞司令部，我每天都用它来构建游戏。我节省的时间是…难以置信的。

你可以在下面一个假设的例子中看到我当时做的事情，我把一个吉拉的票号作为参数传递给命令。这将卷曲吉拉 API，并获取更新游戏所需的所有信息。然后，它将继续构建和部署项目。然后，我会发表评论，并标记利益相关者和设计师，让他们知道已经完成了。

```
$ mason create GS-234
... calling Jira API 
... OK! got values!
... creating a new branch from master called 'GS-234'
... updating templates repository
... copying from template 'pick-from-three'
... injecting values into config JSON
... building project
... deploying game
... Perfect! Here is the live link 
http://www.fake-studio.com/game/fire-water-earth
... Posted comment 'Hey [~ben.smith], this has been released. Does the design look okay? [~jamie.lane]' on Jira.
```

只需轻轻几下就能搞定！

我对整个项目非常满意。

# 概念背后的想法

![](img/7afc3fe38bb70d8d2a617361f6e417f1.png)

Photo by [ThisisEngineering RAEng](https://unsplash.com/@thisisengineering?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

第一部分是配方的集合，或者作为单个全局命令的指导性构建块。这些可以在你一天的工作中使用，并且可以在任何时候被调用来加速你的工作流程或者纯粹为了方便。

# 开发方法

![](img/a7368093136446867fab1cea7e6dcd1d.png)

Photo by [Edgar Castrejon](https://unsplash.com/@edgarraw?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

第二部分是从头开始创建跨平台构建工具的演练。每一个完成特定任务的脚本都是它自己的命令，一个主要的总括命令通常是你的项目的名字，把它们都封装起来。

我知道每个企业的情况和流程都不同，但你应该能够找到一些事情，即使是很小的事情，可以让你在办公室的一天变得轻松一些。

花更多时间开发，花更少时间管理。希望你会喜欢，我会在下一阶段做改进，直到那时见到你！