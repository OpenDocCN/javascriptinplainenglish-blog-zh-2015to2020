# JavaScript & Node.js 简史

> 原文：<https://javascript.plainenglish.io/a-brief-history-of-javascript-9289a4d344d2?source=collection_archive---------2----------------------->

## 在 JS、TS、Node.js 和 Deno 的引擎盖下达到顶峰

## es、Deno 和 TypeScript 的过去、现在和可能的未来

# 起源、浏览器战争和交互式网络

最初， *web 没有任何交互行为*，本质上只是通过计算机网络协议，如 [FTP](https://en.wikipedia.org/wiki/File_Transfer_Protocol#History_of_FTP_servers) ，在网络上显示文本文件。

> 本地网络，或者学校和其他组织的内部网络最终融合在一起，成为内部网络或者**互联网**。

```
This article is part of an ongoing educational series that will be [turned into a book](https://medium.com/@HansOnConsult/learn-how-to-code-in-2020-52bed38a2987) and therefore is considered a "**living article**" subject to change.If you have question, want to contribute or just wanna chat about the content, **leave a comment**! **If you have a find a bug, typo, DM me on twitter** [**@HansOnConsult**](https://twitter.com/HansOnConsult)**.** If you want to contribute to the book, see the GitHub page: [https://github.com/HansUXdev/UniversalJavaScript](https://github.com/HansUXdev/UniversalJavaScript)
```

![](img/e91acb4f1b0020c0dbf5449a8c3208be.png)

Photo by [Alina Grubnyak](https://unsplash.com/@alinnnaaaa?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

最终，网络随着更新的协议如 [DNS](https://en.wikipedia.org/wiki/Domain_Name_System) 、 [HTTP](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol) 而扩展，并使用 [HTML](https://en.wikipedia.org/wiki/HTML#History) 随着[个人电脑](https://en.wikipedia.org/wiki/Personal_computer)和网络浏览器的引入，中产阶级第一次可以访问网络。 [**马赛克** **浏览器**](https://en.wikipedia.org/wiki/Mosaic_(web_browser)) 背后的创作者创造了一种叫做**“LiveScript”**的语言，并于 1995 年发货。

> 不到 3 个月，它就被**更名为“JavaScript”**以建立程序员对 Java 的大肆宣传，这是一种完全独立和不相关的语言。

最终，微软做了他们一直在做的事情，从别人的产品中窃取源代码并发布他们自己的版本，使用“JScript”的 Internet Explorer。浏览器大战开始了，长故事、短故事、马赛克和其他浏览器由于 Internet Explorer 而消亡。然而，JS 的多种分支以及其他脚本语言仍然存在。所有这些都是为了解决提供超越超链接和页面重载的**浏览器交互行为**的相同问题。

## 背后的语言和引擎

[Implementing a JavaScript Engine](https://www.slideshare.net/RednaxelaFX/implement-js-krystalmok20131110) by Kris Mo

> 如果你想了解更多关于 JS 引擎和这种语言背后的代码，看看这个由 Kris Mok 制作的很棒的幻灯片，因为不像我…
> 
> 他实际上构建了一个 JavaScript 引擎…

# 通过脚本实现浏览器行为的标准化

标准化脚本语言的第一次尝试是在 1997 年，使用了 ECMAScript。( **ES-1** )作为**欧洲计算机制造商协会** ( **ECMA** )的一部分。然而，直到 2009 年，不同的实现、竞争的语言和 egos 阻止了任何真正的标准化。在此期间，ES-4 的(失败的)提议(由 Mozilla 和其他人领导)试图调用更传统的编程概念，如类、模块等。

该标准之所以被放弃，很大程度上是因为担心“破坏网络”和引入了允许客户端动态内容的 AJAX(异步 JavaScript 和 XML)。由于多种因素的影响， [jQuery](https://en.wikipedia.org/wiki/JQuery) 于 2006 年诞生，主要是为 JavaScript 和 AJAX 的不同实现提供跨浏览器支持。到 2009 年 **ES-5** 发布，基本上成为了*事实上的 JavaScript 标准，大多数人仍然引用*。

注意是很重要的，事实上 ES-4 中提出的每一个特性都将在 ES-6 中实现，比如[类](https://en.wikipedia.org/wiki/Class_(computer_programming))、[生成器](https://en.wikipedia.org/wiki/Generator_(computer_programming))和[迭代器](https://en.wikipedia.org/wiki/Iterator)、[析构赋值](https://en.wikipedia.org/wiki/JavaScript_syntax#Destructuring_assignment)，以及最重要的[模块系统](https://en.wikipedia.org/wiki/Modular_programming)。唯一真正明显缺少的特性是各种类型的重新实现。

[](https://auth0.com/blog/the-real-story-behind-es4/) [## ECMAScript 4 背后的真实故事

### 我们的 JavaScript 历史文章引发了关于 ECMAScript 4 时代到底发生了什么的有趣评论…

auth0.com](https://auth0.com/blog/the-real-story-behind-es4/) 

# Node.js & JavaScript 模块系统的诞生

从 2009 年开始，serverJS 被创建来给 JavaScript 一个模块系统，后来被重新命名为 [commonJS](https://en.wikipedia.org/wiki/CommonJS) 。目标是“为网页浏览器之外的 JavaScript 建立关于*模块*生态系统的约定”，这可能与一些失败的 ES4 提案有关。

*The video linked shows Ryan Dahl discussing it* ***2 years later****.*

同年晚些时候，Ryan Dahl 将在此基础上创建[node。js](https://nodejs.org/en/)是 JavaScript 的运行时环境，它使用了 Chrome V8 引擎以及其他引擎，如 [libuv](https://github.com/libuv/libuv) ，并于 2009 年 5 月发布。

> 这个运行时环境允许 JavaScript 在安装了该环境的任何地方运行。

在 Node.js 发布后，它永远地改变了 js 语言，并帮助它慢慢地变成更多的编程语言而不是脚本语言。这是因为两件事，异步代码的回调和模块系统，模块系统允许通过包管理器导入和导出多个文件，包管理器 NPM 后来成为开源软件的最大来源之一。Node.js API 还附带了一些允许读/写文件的基本方法(比如 [FS](https://nodejs.org/api/fs.html) )和一个基本的 [HTTP 方法](https://nodejs.org/api/http.html)，这两个方法对于创建一个简单的服务器都是必不可少的。

在 Node.js 发布之后，这个运行时环境和它的主要包管理器， [npm](https://en.wikipedia.org/wiki/Npm_(software)) ，让这个生态系统越来越壮大。浏览器端和节点端的库变得更容易发布、分发，并通过 grunt、gulp、webpack 等工具连接在一起。

> 这使得开发人员更容易在前端和后端快速构建网站原型。因此，向全栈开发人员的过渡突然向任何人敞开了大门，因为它不需要切换到其他语言，如 PHP、python、pearl 等。

## 启动场景

随着时间的推移，由于各种原因，Node.js 越来越受欢迎。

> 也就是说，该环境使事情变得容易学习，因为您不必学习如何配置本地 apache 服务器 xampp、配置 vhost 文件，就像您使用 php & LAMP 等一样。几乎所有你能想到需要的东西，在前端或后端都有一个库，只需一个 npm 安装命令。

> 哦，对了，在许多情况下，当正确实现(异步编码模式)并用于正确的用例时，服务器速度[很快，可以用最小的内存](http://blog.mixu.net/2011/01/17/performance-benchmarking-the-node-js-backend-of-our-48h-product-wehearvoices-net/)处理大量的并发流量。哦，他们编码非常快。

这对于新的或有经验的开发者来说是一个绝对的梦想，而*尤其是* ***初创公司*** *几乎总是* ***推动编程炒作和趋势。*** 一旦它成熟，人们就会看到它在速度、服务器成本、学习成本、更少的*潜在*培训&招聘成本、快速原型的速度、前端和后端团队之间的沟通等方面的优势，而且最重要的是，由于一个优秀的全栈工程师可以同时做前端和后端工作，总体薪资成本可能会下降。后者对创业公司尤其重要，因为这意味着更少的股本和更少的管理费用。

# 响应式设计和移动应用开发

在 Node.js 于 2009 年首次创建到 2013 年暴涨的这段时间里，手机变成了智能手机，应用成为了创业公司成败的关键。这是一种将你的软件全天候放在某些人手中的方式，可以让你的竞争对手获得优势，或者颠覆行业中的其他巨头，建立一个帝国。

![](img/641f8f051ecadb174ec930c706069f27.png)

Photo by [Neil Soni](https://unsplash.com/@neilsoniphotography?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

媒体询问是在 2008-2009 年引入的，而[响应式设计](https://thrivehive.com/responsive-design-history/)是在 2010 年创造的一个术语，用来解释这种技术和我们的社会的根本转变所产生的需求。在响应式设计处理网页设计需求的地方，新技术即将出现来扰乱移动应用程序的开发。

到 2011 年，另一项技术开始流行起来，很大程度上是受响应式设计哲学的影响。Apache Cordova ，它允许 web 开发者使用 HTML、CSS 和 JS 来构建移动应用。在此之前，你必须精通像 android 的 Java 或 iOS 应用的 objective C 这样的语言。这些语言不仅明显更难学习，而且开发环境过去(现在仍然)更难调试，开发时间更长，因为您必须等待代码重新编译。Cordova 提供了一个解决方案，一种编程语言(JS)、html(标记)和 CSS(样式),而且更容易学习。

当然，这也有很大的缺点，也就是说，应用程序的运行速度比原生应用程序慢，因此仍然不能像 Node.js 那样进行探索。 [Ionic](https://ionicframework.com/) 于 2013 年在此基础上建成，此后更进一步，并在很大程度上取代了 Cordova。但这也不足以拯救微软 windows 手机，因为没有人为他们的市场开发应用程序…

> 个人来说，我叔叔为微软工作了 20 多年&大部分时间都在用他们的手机。所以这就是为什么我试图在整篇文章中进行幽默的抨击。他就像我的第二个爸爸，当我看到 2000-2008 年生产的**最新智能手机 MS**也有**全互联网**([windows mobile](https://en.wikipedia.org/wiki/Windows_Mobile))时，我总是感到惊讶和鼓舞。在响应式设计诞生之前整整十年。

![](img/85c7bba61fd45d2ad2c411476200342a.png)

Photo by [Louis Reed](https://unsplash.com/@_louisreed?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在硬件方面， [Johnny-Five.io](http://johnny-five.io/) 于 2012 年首次亮相，通过它，你可以利用 JS 的简单性以及 Node.js 和 NPM 背后的强大功能来首次快速开发硬件原型。

过去所有需要静态类型的面向对象语言的领域都被侵占了。

这允许我们作为开发者使用 build [Arduino](http://johnny-five.io/#arduino) ， [Tessel 2](http://johnny-five.io/#tessel) ， [Raspberry Pi](http://johnny-five.io/#raspberrypi) ，以及基本上任何你可以在上面安装 Node.js 和 johnny-five 的东西。不仅仅是机器人，今天的许多 IOT 设备都是建立在这个基础上的，它已经在很多方面产生了深远的影响，甚至在 JavaScript 场景中可能没有完全意识到或意识到。

> 结果，J **avaScript 作为一种语言成为*可以说是*最通用和最容易使用的*编程语言*** ，现在可以在客户端(浏览器)、服务器、手机/平板电脑应用程序上使用，甚至在某种程度上，可以通过 [**Johnny-Five**](http://johnny-five.io/) 在微控制器上使用。
> 
> 哦，甚至有几个图书馆建立虚拟现实，甚至游戏…

# 节点分叉 ES6 问题

到了 2014 年，Node.js 因为各种原因开始有了一些不同的分叉。最值得注意的是 io.js，它最终与 node.js 合并在一起。但是还有其他几个分叉，我不会提到，它们背后的原因各不相同，从技术原因(如承诺)，缺乏贡献者，甚至*琐碎和坦率地说，不成熟的*自我相关的个人差异(但我不会链接到那个蠕虫罐，让我远离我，谢谢…)。

到 2015 年，最新的 JavaScript 标准， **ECMAScript 6** 发布了，随之而来的是几乎所有最初在 ES4 中起草的东西，除了突破性的变化，特别是引入了`let`、`const`和`symbol`，而不是更传统的局部/全局变量和静态、强类型声明。同样，**不像最初的 ES4 草案**那样*会破坏 web* ，这种方法具有 [**巴别塔**](https://babeljs.io/) 的能力，并允许我们使用未来的功能，今天通过从 ES6+编译到 ES5(当时的标准)。

> Node.js 让这一切成为可能。

这些新的 JavaScript 特性包括 ESM 或 ECMAScript 模块(导入/导出，而不是通过 commonJS 的`require()`)、async/await、fetch browser API 以及许多其他不在最初的 ES4 草案中的特性。其中一些特性还在不同程度上引入了与 Node.js 核心架构的兼容性问题。最值得注意的是，在过去 5 年中，ESM 标准一直是一个非常现实和持久的问题，需要使用第三方软件包、实验性标志或使用`.mjs`文件，具体取决于各种考虑因素。

# TypeScript 的诞生和兴起:对 ES4 和 ES6 的回应？

然而，TypeScript 悄悄潜伏在后台，于 2012 年首次推出，但直到 2014 年才发布 1.0 版本，几乎与 ES6 成为新标准的时间相同。

从现在开始。我将根据我对历史和 2020 年当前形势的理解，开始插入我个人的一小部分观点，我将作为一名社会学家和一名拥有近五年经验的 JavaScript 开发人员，尝试对未来做出一些预测。

我认为 JavaScript 在很大程度上是一种破损的语言，在我们整个全球经济和技术的大部分与我们的社会现实交织在一起之前，它就应该被修复。换句话说，他们关于 ES4 的提议可能是对的…但是现在已经太晚了。

> 最后，我认为 **TypeScript 真的是** **令人惊叹的**，它调试了语言本身固有的错误，并在快速原型和高质量代码之间取得了良好的平衡，我迫不及待地想看看 Deno 为这种语言准备了什么。

# 德诺宝宝的诞生

> Deno 于 2018 年首次宣布，当时 Node.js 的最初创造者 Ryan Dahl 通过引入完全重写，完全从零开始，基于现代 js 标准，如 **promises & async/await** 、 **ESM** 、**类型化数组**和 **TypeScript** ，席卷了 JavaScript 世界。

由于所有这些历史和其他因素，最初的创造者瑞安·达尔开始从事一些新的工作。在演讲中，他谈到 Deno 主要是一个*“思想实验”*，并表达了他对构建 nodeJS 的最大遗憾，对 TypeScript 的热爱，对 dart 的憎恨。

## Deno 的版本 1

今天，演示版已经准备好了，并且已经稳定，可以供您试用，它的版本是 1。

事实上，自从一月份他们将安装编译成可执行文件以来，它已经足够稳定了。

无论如何，这是它的创作者最近的一个视频。我会让他说的。

我可以去写我自己的 Deno 教程，并将其与上面的节点示例进行比较(我也这样做了……)。但是，我删除了它，一旦这些视频出来，因为和重复使用上述历史部分的其他部分。

不管怎样，布拉德·特拉弗斯一如既往地做着令人惊叹的工作。第一个视频在我个人看来有点太长了。

然而，第二个视频很棒，因为它超过了 [denon](https://deno.land/x/denon/) ，就像 nodemon、npm 脚本& package.json 的奇怪混合。它真的很酷很甜。

# 可能的未来？

我想以我去年教高中生时谈到的同样的预测开始，告诉他们注意将改变发展前景的三件事:

1.  TypeScript & Deno
    学习行业岗位所需的后端&代码质量(方)。
2.  CSS Houdini
    用于浏览器优化、自定义布局等等。
3.  web Assembly &[Assembly script](https://github.com/AssemblyScript/assemblyscript)
    针对服务器、移动应用& VR 进行原生式优化。

> 换句话说，**这就像** **我们又回到了 2009 年，只是现在轮到了打字稿**到**用自己的运行时环境来颠覆**的格局。

> 不是 JavaScript 和 Node.js，而是 TypeScript 和 Deno。
> 
> 随着我们学会在 5G 和云游戏的世界中调整这个全球疫情的现实，它可能会取代移动应用和响应式设计，而是 VR/AR 设计界面…
> 
> 与 Cordova、Ionic 或 NativeScript 不同的是，使用包装器编译以本机运行，它将是您在 TypeScript 中编写和调试的东西，并编译成 Web 程序集，性能将不会成为大问题。

# 想法或评论？

我希望你喜欢读这篇文章，不要把我的任何批评当成针对你个人的(除非你是我叔叔，在这种情况下，这只是一个有趣的玩笑)。我很想听听你的想法！*特别是*如果你是 JavaScript / ECMAScript 历史的一部分，因为我直到 2013 年才开始使用它，而且*直到 2015–2016 年才真正成为*全栈。我尽力做最好的研究。

# 用简单的英语写的便条

你知道我们推出了一个 YouTube 频道吗？我们制作的每个视频都旨在教给你一些新的东西。点击此处 查看我们，并确保订阅该频道😎

# 关于作者

Brett“Hans”McMurdy 是一名自学成才的开发人员，在前端、后端以及两者之间的几个主要领域拥有 6 年多的经验。

![](img/420c186f90f4ca10032437e46e41a40f.png)

他目前是一名全职爸爸，正在寻找一份全职工作，如果你有兴趣雇佣他，可以看看他的 Linkedin。

与此同时，他正在从事一些很酷的开源项目，这应该会让你考虑资助他。

1.他正在写一本关于 JavaScript 的开源书籍，用 node.js 而不是浏览器来教授这门语言。这也是一个由 GitPod 驱动的远程开发环境，因此您不需要一台昂贵的计算机，只需打开书本，在一个预配置的环境中开始学习。

2.他正在创建一些简单但强大的 vscode 扩展。

3.当他的追随者达到 50 人时，他想在 Twitch 上推出一个免费的课程 [**。**](https://www.twitch.tv/hansoncoding)