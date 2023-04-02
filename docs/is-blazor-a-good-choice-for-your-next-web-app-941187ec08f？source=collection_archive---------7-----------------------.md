# Blazor 是你下一个 Web App 的好选择吗？

> 原文：<https://javascript.plainenglish.io/is-blazor-a-good-choice-for-your-next-web-app-941187ec08f?source=collection_archive---------7----------------------->

## Blazor 的优势&它可能不适合的情况

![](img/216a3b064c19bba78193ed1529c89c84.png)

本文将介绍为您的新 web 应用程序开发选择 Blazor 的一些原因。我们将主要从技术的角度来探讨这个主题，但也会简要地考虑您团队中的资源管理。

如果你以前没有听说过，Blazor 是一个单页面应用程序开发框架。它是开源的，建立在开放网络标准之上。诞生于微软之手，它结合了旧剃刀与新剃刀。Net 和 WebAssembly，它允许您创建浏览器和服务器端应用程序。

值得注意的是，截至今天，Blazor 的第一个版本是在 2 年前发布的，而它的服务器端部分，仅仅是在一年前。考虑到这一点，我们只想强调 Blazor 相当新，这一点必须贯穿整篇文章。

# WebAssembly 和 JavaScript

Blazor 设置了一个新的模式来玩 WebAssembly 带来的游戏。万维网联盟设计了 WebAssembly，在 2017 年为 C++或 Rust back 等其他语言提供了编译目标，可以在所有现代浏览器中工作，桌面或移动，开箱即用。要检查您的浏览器是否支持它，您可以前往[我可以使用 HTML5、CSS3 等的支持表](https://caniuse.com/?search=webassembly)。

这意味着现在有可能使用。Net 来全面开发您的 web 应用程序，限制(如果不是完全消除的话)JavaScript 的使用。对于一些人来说，这可能是选择 Blazor 的一个足够好的理由。

但是，如果你想使用 JavaScript 呢？别担心，布拉索会掩护你的。它带有 JavaScript 互操作性。通过 JS Interop，Blazor 应用程序可以从。NET 方法和。来自 JavaScript 函数的. NET 方法。

如果这还不足以为通过 Blazor 使用 WebAssembly 欢呼的话，正如我前面提到的，WebAssembly 是一种更高级语言的编译目标，与 JavaScript 相反，JavaScript 是一种即时高级语言。WebAssembly 的二进制格式使其以接近本机的性能运行。尽管有研究对 WebAssembly 声称的速度提出了质疑(1，2)，但总体而言，它仍然比 Javascript 有 20%到 34%的提高，如下面的研究中所引用的(1，2，3)。

1 — [没那么快:分析 WebAssembly 与本机代码的性能](https://www.usenix.org/conference/atc19/presentation/jangda)

2 — [注意差距:分析 WebAssembly 与本机代码的性能](https://www.groundai.com/project/mind-the-gap-analyzing-the-performance-of-webassembly-vs-native-code/1)

去 WASM 还是不去 WASM？—开发

要了解更多关于 Blazor 和 WebAssembly 如何工作的信息，你可以前往 ASP.NET 核心 Blazor 的[介绍。](https://docs.microsoft.com/en-us/aspnet/core/blazor/?view=aspnetcore-3.1#code-sharing-and-net-standard)

# 那么 Blazor 的 TypeScript 呢？

与 JavaScript 一样，可以在 Blazor 项目中使用 TypeScript。毕竟，TypeScript 是 JavaScript 的超集，它建立在 JavaScript 之上，并且得到了微软的官方支持。编译 Blazor 项目时，您的 TypeScript 代码将被转换成 JavaScript，两个文件将共存。但是，每次 TS 文件进行新的编译时，JS 文件都会被覆盖，所以请记住这一点。当然，您可以通过 MSBuild 属性配置 MSBuild 如何做到这一点。如果你想探索所有的 TypeScript 编译器选项，请查阅[手册——MSBuild](https://www.typescriptlang.org/docs/handbook/compiler-options-in-msbuild.html)中的编译器选项。

由于我们在同一篇文章中讨论的是 TypeScript 和 WebAssembly，所以 AssemblyScript 值得一提。AssemblyScript 是一个 TypeScript-to-WebAssembly 编译器，尽管 Blazor 似乎还没有对此提供支持，但没有什么能阻止它成为未来的一个选项。因此，您可以从 Blazor 项目中的 TypeScript 代码生成 WebAssembly，而不是 Javascript。也可能发生这样的情况，微软决定不把它作为一个选项，而是选择使用 C#来代替 TypeScript。但我们必须等待，看看这是否会成为现实。

# 中后端到前端代码的可重用性。网

因此，在这一点上很清楚，当使用 Blazor 时，我们正在使用。NET 贯穿整个开发过程。这带来了继承的好处，即能够从后端到前端重用代码，反之亦然。更不用说开发人员不需要了解两种不同的技术就能在两个领域工作的能力了。

同时，因为您使用的是相同的代码，所以您可以选择将代码放在您希望它运行的任何地方。让我们更深入地研究一下。Blazor 可以直接在浏览器中运行你的客户端代码，或者你可以分离出一部分代码放到服务器上。如果您决定分离代码，它会将客户端 UI 事件发送到服务器(使用信号 R ),并将 UI 更改从服务器发送回客户端。这允许您的客户端保持轻量级，或者利用您的服务器端计算能力来减少客户端的处理时间。

多亏了。NET 标准，而且因为它已经发布了一段时间，所以有大量的库可供您在服务器端 Blazor 开发中加以利用。

要知道你的目标是在 Blazor 上进行客户端开发还是服务器端开发，你可以看看 Fiodar Sazanavets:[Blazor 在 web 开发上的利与弊——科学程序员](https://scientificprogrammer.net/2019/08/18/pros-and-cons-of-blazor-for-web-development/)的这篇文章

# 外部供应商的支持

Blazor 相对较新，但出自微软之手，它很快获得了第三方供应商的信任。这没什么特别的，也不是其他基于 JavaScript 的框架所没有的，但它显示了开发社区对 Blazor 的投入和工作。

如果你想探究一些，这里有一个列表: [Telerik](https://www.telerik.com/blazor-ui) ， [DevExpress](https://www.devexpress.com/blazor-razor-components/) ， [Syncfusion](https://www.syncfusion.com/aspnet-core-blazor-components) ， [Radzen](https://blazor.radzen.com/) ， [Infragistics](https://www.infragistics.com/products/ignite-ui-blazor) ， [GrapeCity](https://www.grapecity.com/componentone/blazor-ui-controls) ， [jQWidgets](https://blazor.jqwidgets.com/)

# 开发工具

作为微软家族的一员，Blazor 得到了市场上最好的开发工具之一 Visual Studio Code 的支持。不仅数一数二，而且免费。适用于 Windows、Linux 和 macOS。[免费开发者软件&服务— Visual Studio](https://visualstudio.microsoft.com/free-developer-offers/)

# 。净技能

这可能太明显了，但是 Blazor 利用了你在。NET 并允许您只需做一点额外的工作就可以开始从事 web 开发，这大大缩短了学习曲线。所以如果你的团队或公司精通。NET 并缺乏 JavaScript 或 web 开发知识，Blazor 可能是加快生产过程的方法。

# 那你为什么会选择其他东西而不是布拉索呢？

列举一些显而易见的:如果你的团队或组织缺乏。NET 专业知识和/或习惯于使用另一个前端框架，并且您的项目不需要 WebAssembly 可以提供的额外性能。没有理由让你放弃知识资产，开始新语言和新框架的新的陡峭的学习曲线。

尽管 Blazor 看起来很好，但我们需要记住它在一年前还是一个新生儿，因此，它需要成长和发展。即使它正在快速成长，成为一个非常有能力的青少年，你也不会用一个还在上学的新天才来取代你有价值的高级工程师。同样的，如果你所有的开发都在另一种技术上，你没有理由去混淆和复杂化你的技术领域。当前流行的前端框架已经过测试，并在它们所做的事情上表现出色，它们不会因为 Blazor 而倒闭，所以你可以放心地继续使用它们。

# 关于布拉索的最后想法

Blazor 是一项可爱的技术，将为有经验的人创造奇迹。NET 开发人员，为每个刚进入这个世界的人开辟 web 开发的学习曲线。如果您从一个新项目开始，并且拥有。NET 技能，或者您根本不具备任何 web 开发技能，如果您想在应用程序运行的浏览器中挤出性能，那么就给 Blazor 一个机会吧！

另一方面，如果您已经有了一个使用 JavaScript 框架的项目和一个技术娴熟的 JavaScript 开发团队，以及一个不需要额外性能的工作应用程序，就没有必要求助于 Blazor。对您当前的项目来说不是，对具有相同特征的新项目来说可能也不是。

如果你想了解 Blazor 如何一步一步发展，请关注 Visual Studio 杂志: [TypeScript，Blazor，ASP.NET 核心指南，新闻，技巧和教程](https://visualstudiomagazine.com/pages/topic-pages/html5-javascript-tutorials.aspx)

*这篇文章由 Zartis 的高级软件工程师卡洛斯·弗恩德斯撰写。*

**引文:**

1.  [没那么快:分析 WebAssembly 与本机代码的性能](https://www.usenix.org/conference/atc19/presentation/jangda)

2.[注意差距:分析 WebAssembly 与本机代码的性能](https://www.groundai.com/project/mind-the-gap-analyzing-the-performance-of-webassembly-vs-native-code/1)

3.[去 WASM 还是不去 WASM？—开发](https://dev.to/linkuriousdev/to-wasm-or-not-to-wasm-3803)