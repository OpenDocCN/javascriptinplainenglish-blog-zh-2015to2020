# 4 年的编程生涯教会了我如何编写类型化的 JavaScript

> 原文：<https://javascript.plainenglish.io/what-4-years-of-programming-taught-me-about-writing-typed-javascript-2bac38b45f79?source=collection_archive---------11----------------------->

## 非类型化 JavaScript、TypeScript、Flow 和 PropTypes:应该使用哪一个？

![](img/d5f8559fabf6c27e470e93dd89c666e6.png)

Photo by [Kevin Canlas](https://unsplash.com/@kvncnls) on [Unsplash](https://unsplash.com/s/photos/javascript)

## 类型的类型

大多数情况下，静态类型语言被批评为限制了开发人员。另一方面，他们喜欢带来关于错误的早期信息，记录组件(如模块、方法等)。)，现在还有其他更高级的功能，比如自动补全。

2009 年关于非类型化语言的一项初步研究给了我们一些关于这些利弊的参考。今天，另一种类型的语言也被广泛使用:动态类型语言。

动态类型语言不同于它的对应物，它引入了类型，但在运行时。这样，您可以拥有比强类型语言更多的自由，同时保留它们的优点。

 [## 为什么要使用动态语言而不是静态类型的语言？

### 自从有编程语言以来，动态和静态类型的语言就一直并存…

erik-engheim.medium.com](https://erik-engheim.medium.com/why-use-a-dynamic-language-over-a-statically-typed-one-fb434994e2b6) 

从我们的列表中，我们有一个单一的动态类型语言:TypeScript。这并不完全准确，TS 也可以被称为软类型语言，介于动态和静态类型语言之间。由于这不是今天的主题，好奇的读者可以看看下面这篇文章:

[](https://itnext.io/typescript-static-or-dynamic-64bceb50b93e) [## TypeScript:静态还是动态？战争结束了。

### TypeScript 越来越受欢迎，这让一场古老的静态和动态语言之战死灰复燃。当你投入其中时，你…

itnext.io](https://itnext.io/typescript-static-or-dynamic-64bceb50b93e) 

为什么我说只有一个？当然，JavaScript 被认为是无类型的(或弱类型的)，而 PropTypes 是一个允许在运行时进行类型检查的包。

[](https://www.npmjs.com/package/prop-types) [## 道具类型

### React props 和类似对象的运行时类型检查。您可以使用 prop-types 来记录预期的…类型

www.npmjs.com](https://www.npmjs.com/package/prop-types) 

心流是…都不是。在实践中，它看起来很像 TypeScript，两者经常被比较。在您的 ide 和 CLI 中，它们是相似的，但是它们的引擎不同:TypeScript 是一种语言，而 Flow 被称为“静态类型检查器”。

在 Flow 中，您编写“注释”来设置类型。在编译时，必须删除这些注释，这将创建没有任何超集的 JavaScript 文件。

这是一个有利于心流的论点:性能。两种解决方案具有几乎相同的功能，但是 Flow 消除了 TypeScript 编译后的任何开销。

## 我的经历

我在 2016 年以 Angular2(和 TypeScript)开始了我在 JavaScript &前端世界的职业生涯。

在这个前端项目之前，我主要做 C#，Java，和一点香草 JavaScript。我讨厌它。对我来说，普通的 JavaScript 没有结构、类型和面向对象的概念，简直是地狱。

经验多了，用 angular 2(TypeScript 自带)练习一下，然后反应过来，我差不多开始享受了。那时我认真考虑了一种键入 JavaScript 的方法:我使用了一段时间的 TypeScript(当时我还没有掌握)，但我用 React 又回到了非类型化的 JavaScript。

使用非类型化的 JavaScript 进行反应工作得还不错，但是我觉得缺少了一部分:

*   在 React 项目上工作几周后，代码很容易变得混乱和难以理解，对新手来说更是如此。
*   我们需要很多时间来阅读一段代码。
*   构建后的错误数量太多。

在需要避免这些问题的实践中，我的经验告诉我键入 JavaScript 是最优先的。经过一些研究:我在这里，看着 TypeScript、Flow 和 PropTypes。

PropsTypes 允许我对我的道具进行类型检查，但这还不够。React 组件之外的类型呢？作为 CI 渠道的一部分，我可以验证我的应用程序是类型安全的吗？我能把它作为一个提交钩子来验证吗？

嗯，你可以找到一些方法在你的测试中[验证类型，并在组件](https://stackoverflow.com/questions/26124914/how-to-test-react-proptypes-through-jest)之外[使用它，但是 PropTypes 并没有考虑到这一点。它旨在为您提供实时信息(在运行时)。](https://blog.jim-nielsen.com/2020/proptypes-outside-of-react-in-template-literal-components/)

由于 PropTypes 不能说服我，我只有两个选择:TypeScript 和 Flow。就功能而言，它们看起来是一样的，但在流程方面有一些优点:

*   没有开销意味着更好的性能。
*   由脸书撑腰，以及作出反应。

这些足以产生影响，这就是为什么我开始使用 Flow with React。

[](https://flow.org/) [## flow:JavaScript 的静态类型检查器

### 使用数据流分析，flow 推断类型并在数据通过代码时跟踪数据。你不需要完全…

flow.org](https://flow.org/) 

## 流动

大约两年。那是我一直用心流和唯心流的时候。它与 React / React Native 配合得非常好，提高了我们团队的工作效率，是我们 CI 渠道中的一个很好的工具，通常帮助我们交付产品。

然后，我们碰壁了。一旦我们的项目变得越来越大，流服务器开始挣扎，变得越来越慢。在某种程度上，我们根本无法运行它，我们的 CI 运行时间和成本直线上升。

不幸的是，与此同时，我开始涉足其他项目。其中:采用 Arduino 的嵌入式项目使用[强尼五号](http://johnny-five.io) ⁴.这时我意识到了 Flow 的第二个弱点:它的社区支持。

JavaScript 中的类型系统就是这样工作的:用一种解决方案编写模块，为另一种解决方案编写独立的类型定义。TypeScript 有很多支持，我想即使在今天，我使用的没有支持的库的数量也不到 10 个。

Flow 是不同的:当我在 React 生态系统中使用它的时候，有一些库没有支持，但是我仍然可以在没有它的情况下工作。在这个生态系统之外，支持是一团乱麻，我甚至找不到一个使用心流的库。我找到了将 TypeScript 定义转换为流的解决方案，但最终，它不起作用。

在某些时候，我想知道我是否真的必须使用 TypeScript，即使是对于 React / RN 项目。这有多矛盾:使用脸书等公司支持的技术，用微软的打字系统替换它的打字系统？而且，React 对 TypeScript 的支持肯定是一塌糊涂吧？

[](https://www.typescriptlang.org/) [## 任意比例的输入 JavaScript。

### TypeScript 通过向语言中添加类型来扩展 JavaScript。TypeScript 通过以下方式加速您的开发体验…

www.typescriptlang.org](https://www.typescriptlang.org/) 

## 以打字打的文件

> 我(重新)发现了一个全新的世界。

之后，在 Johnny-Five 或 React 的辅助项目中重新打字，我简直不敢相信我是多么喜欢它。

不仅我的性能和库支持问题消失了，而且我爱上了它的语法。自 2016 年和大量更新以来，我找不到任何可以责备 TypeScript 的地方。

React / React 原生支持是完美的，整个生态系统如 eslint with prettier，jest 等。

我的下一步很简单:因为我之前使用 Flow 为我的团队编写了模板/样板，它们很快就被替换成了 TypeScript 中的对等物。从那以后，我们只使用 TypeScript。

Flow 在很长一段时间内都很棒，它的性能和支持问题让他不知所措，而 TypeScript 后来变得令人惊叹:我的选择很简单。

## 结论

如果你问我在非类型化 JavaScript、PropTypes、Flow 和 TypeScript 之间做什么选择，我会告诉你:

*   如果您在一个项目上工作不到一周，之后就放弃了，并且不想学习任何类型解决方案，那么非类型化 JavaScript 可能是一个不错的选择。
*   如果出于任何原因，您不能使用 Flow 或 TypeScript，PropTypes 是一个很好的工具，但对于一个大项目来说还不够。
*   “心流”很棒，从那以后我没有再尝试过，但也不会在它上面下注。
*   TypeScript 是一个很好的解决方案，在我看来是最好的，也是任何高风险 JavaScript 项目所需要的。

关于使用类型库，我遇到了一些反对意见，我想在这里回答一下。

*   性能损失:今天的解决方案很棒，您可能承受的无限小的性能损失将被类型化代码带来的结构所平衡。
*   缺乏 TypeScript / Flow 方面的知识:如果你的项目有很高的风险，而你不想使用类型化的 JavaScript，因为你的开发人员不了解它，那么这个项目注定会失败。改变你的团队或者训练他们，这是唯一的方法。
*   时间的损失:我将引用[干净代码:敏捷软件技术手册](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) ⁵来自[罗伯特 c .马丁](https://fr.wikipedia.org/wiki/Robert_C._Martin)⁶:**的确，花在阅读和写作上的时间比远远超过 10:1。**“如果一件事，键入 JavaScript 将帮助你阅读代码，从而提高而不是降低你的生产力。

如果您对类型化 JavaScript 或 Flow 有不同的看法，请随时联系我，以便我可以链接您的。

感谢阅读。

# TL；速度三角形定位法(dead reckoning)

打字稿。

[](https://morintd.medium.com/about-me-teddy-morin-9fb1d65fe24e) [## 关于我——泰迪·莫林

### 嗨，我是泰迪，一个反应的爱人🚀。

morintd.medium.com](https://morintd.medium.com/about-me-teddy-morin-9fb1d65fe24e) 

参考资料:

*   **【1】使用非类型化编程语言的成本—首次实证结果:**[https://www . science direct . com/science/article/pii/s 1474667016339969](https://www.sciencedirect.com/science/article/pii/S1474667016339969)。
*   **【2】如何通过 jest 测试 react prop-types:**[https://stack overflow . com/questions/26124914/How-to-test-react-prop types-through-jest](https://stackoverflow.com/questions/26124914/how-to-test-react-proptypes-through-jest)。
*   **【3】在 React 之外使用 prop-types:**[https://blog . Jim-Nielsen . com/2020/prop types-outside-of-React-in-template-literal-components/](https://blog.jim-nielsen.com/2020/proptypes-outside-of-react-in-template-literal-components/)。
*   **【4】Johnny-Five——Javascript&IOT 机器人平台:**[http://Johnny-Five . io](http://johnny-five.io)。
*   **【5】Clean Code:敏捷软件工艺手册:**[https://www . Amazon . com/Clean-Code-Handbook-Software-craftness/DP/0132350882](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)
*   **【6】罗伯特·c·马丁:**[https://fr.wikipedia.org/wiki/Robert_C._Martin](https://fr.wikipedia.org/wiki/Robert_C._Martin)