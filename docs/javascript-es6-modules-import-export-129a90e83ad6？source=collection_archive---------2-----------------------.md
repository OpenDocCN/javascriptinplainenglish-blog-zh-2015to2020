# JavaScript ES6 模块导入导出

> 原文：<https://javascript.plainenglish.io/javascript-es6-modules-import-export-129a90e83ad6?source=collection_archive---------2----------------------->

## ES6 模块导入导出业务用 JavaScript 和 JS 遗留代码

![](img/b0a0ad62a4d095efd815199ad33aec21.png)

[Karolina Grabowska](https://www.pexels.com/@karolina-grabowska), by [pexels](https://www.pexels.com/photo/american-and-chinese-flags-and-usa-dollars-4386371/) (CC0)

**快速读者**:如果你在这里直奔主题，跳转到章节“**更强更快更好 ECMAScript 2015**

不是所有发光的东西都是金子。1995 年 Javascript 诞生的时候，还没有模块这种东西。为了什么？这是需要回答的正确问题。JavaScript 最初是作为静态网页的小助手，让网站的某些部分变得有点动态。因此，几行代码不需要打包成模块。

随着越来越受欢迎，编码人员发现了 JavaScript 的潜力，有一个愿望必须得到满足。模块化代码！尤其是对于浏览器部分，每个 JavaScript 文件都被加载到一个全局名称空间中。文件没有区别，全局命名空间把每个变量都放在一堆变量中，不管你把变量放在哪个文件中。在两个独立的文件中有两个同名的变量？那你就有问题了。因此，使用某种分隔是 JavaScript 的一项职责。如果没有模块，这是如何实现的？

我们知道开发人员有能力提出解决方案并帮助自己。一个构造可以在名称空间内创建一个名称空间，即函数。想出一个基本的解决方法，将你的整个文件封装到一个函数中，你就得到它了。一个“模块”——非常相似，但不相同。因此，这个函数的内容只在本地声明。为了避免全局命名空间中的冲突，函数本身最好是匿名的，因为您需要一些同名的函数。

由于没有模块的导入/导出，您必须直接调用这个函数，方法是将普通的大括号直接放在后面，以便立即调用这个函数。不幸的是，它并不像解释的那样工作。JavaScript 不允许函数匿名，它希望我们在函数关键字后写一个标识符。
然而，这里有一个小技巧:在整个结构周围加上支架。JS 会认为这是理所当然的，你现在可以运行代码了！

我给大家介绍的有个名字:立即调用函数表达式(life)[1]。在本文中，我将向您展示，如今您不需要这种技巧，并且可以使用 ES2015(又名 ES6)中引入的导入/导出关键字生成干净的代码。

# 服务器上的 CommonJS

2009 年是 Node.JS 的诞生年【2】node . js 使得在服务器端运行 JavaScript 成为可能。有什么区别？好吧，在服务器端，架构建议你在模块中运行代码，我们知道今天它是使用微服务的艺术状态，但是 JavaScript 的架构还没有为此做好准备！因此，Node 的发明者。JS 不得不依靠 Common.JS 的方法，为服务器端 JS 代码引入基于模块的解决方案。

为什么需要使用基于模块的服务器端代码？一个理由是服务器的构建方式不同，而服务器可以加载许多客户机不应该加载的文件。这是因为简单地避免了拒绝服务。客户端应该加载几个文件，但要比多个单个文件大。在这里，您的首要任务是保存 HTTP 请求，并尽可能降低请求率。

有共同之处。JS 还提供了关键字“require”来加载代码部分，这是由“module.exports”提供的。购买/出售物品的功能可以像这样导出:

该语句导出一个具有“buyItem”和“sellItem”属性的对象。

*注意:只有对象将被导出。而不是对象的名称。在这个文件之外，对象的名称(“供应商”)是不存在的。您需要将它附加到导入该代码部分的文件内的一个新名称上。*

此外，所有变量在外部都是不可见的。JS 默认设置使一切都是私有的。IIFE 由 Node.JS 的内置包装功能隐藏执行。

当您在其他地方需要这个导出的对象时，使用“require”访问它，您将获得与您曾经导出的对象相同的对象。

*注意:这样每个文件只能导出一个对象。如果您想要导出不同的功能，您必须将其包含在一个对象下。*

# 更强更快更好 ECMAScript 2015

它来了，像飓风一样震撼你！全新的原生模块系统！向您介绍新的关键字:进口/出口。“Import”取代了单词“require”,“export”取代了“module.exports”。但是这种行为是不能 1:1 转移的。

先看导出关键词。坚持平凡。JS 模式中，没有显式导出的所有内容都被标记为私有。准备导出的是所有以 export 为前缀的函数和数据类型。

通常，当使用 export 关键字时，您会注意到可以在一个文件中的多个函数/变量前面设置 export。

这里是“module.exports”和“export”的区别。“module.exports”仅导出封装为对象的函数。而不是函数的名称。而“出口”恰恰做到了这一点。从外部，您可以看到导出函数的名称。当这种情况发生时，我们称之为“命名导出”。

如果您的文件变得比平常稍大，跟踪导出的功能可能是一项单调乏味的任务。“module.exports”的一个优点是在最后将您的导出作为一个对象进行收集。幸运的是，“导出”也可以做到这一点。

*注意:这不是物体！它看起来像是因为花括号，但它不是。它是命名导出的集合！*

# 出口在哪里，进口就在不远处

在 ES6 中，将供应商导入到另一个文件中相当容易。只需使用 import 关键字，并确保不要落入导入的对象是对象的陷阱，尽管它们看起来很像。

如果您想将导出的对象作为对象导入，那么有一种语法适合您:

这使得访问像一个适当的对象，并为我们提供了代码清理的功能。

编写干净的代码很好也很容易，但是如果我们有两个函数名相同的模块呢？请记住，我们不是导出整个文件，而是只导出标有“export”的函数，因为我们知道我们导出的函数包括它们的名称，所以我们最终会遇到冲突。

一种解决方法是将它们作为对象导入。当我们这样做时，我们给函数分配一个新的名称空间。
另一种方法是用“as”关键字重命名已命名的导出，如下所示:

这给我们带来了两大优势。首先:编译器或 JavaScript 运行时环境可以检查文件的哪些部分在加载期间是需要的，关键字树摇动。第二，每当您重构代码并想要重命名导入/导出的代码时，您的开发环境就会知道这一点，因为它们被命名为导出和可引用的。

# 未命名或默认导出

有时不需要导出已命名的文件。有些情况下，您只想导出单个值或变量，而将命名的决定权留给导入该值或变量的编码人员。我们应该在不使用“as”关键字的情况下这样做。

“默认”导出是专门为这种情况设计的。在导出之后加上 default，你就已经得到了。

*注意:您不能内联声明一个常量默认导出。首先，内联声明并初始化“const ”,在第二行中，您可以将其标记为未命名的导出。*

如果导入这个，只导入值，而不导入名称。不使用“as ”,我们现在可以更改导入的名称。在这里设置没有花括号。

从技术的角度来看，这只不过是一个命名的导出，但不是用户定义的名称，而是默认的指定名称，与命名名称相反，真实的命名导出不会出现双重命名。对默认或未命名导出的数量设置了限制。每个文件只能有一个。永远记住这一点。

有趣的是，我们可以将两者混合在一起。

这样做时，导入程序可以选择选择某些功能，或者将整个文件作为一个包导入。这类似于*-import 方法。

# 遗留代码

使用遗留代码总是很繁重。由于新旧技术的混合，产生新漏洞的可能性很高。同样的道理也适用于模块，一旦用 CommonJS 写好，你会把这些代码转移到 ES6 模块吗？

坚持使用命名的导出，因为命名的导出很容易被 CommonJS 导入。因为整个导入将被转换成一个具有所有属性的对象。使用默认导出变得很困难，因为我们必须更改导出的名称，这是默认的，这是一个关键字。此外，对于第二个未命名的导出，我们将遇到命名问题。第一种方法是在析构时重命名，第二种方法是重写“require”。

因为这是一项繁琐的工作，所以最好坚持使用指定的导出，这样可以更快地完成工作！

# 结论

由于 ES6 引入的这个模块系统是 JavaScript 朝着正确方向迈出的重要一步，并且得到了浏览器的支持，所以没有人反对它。Typescript 和预编译器 Babel 都支持 ES6 模块。尽可能快地在你的项目中实现这个系统。或者当开始一个新的，直接设计你的架构。模块化是编写更少代码和更好应用的关键。

一个常见的陷阱是使用花括号“{}”的独特方式。花括号不是用来引入对象文字的，它们只是一个语法工具。

您是否正处于命名导出和未命名导出之间？选择一如既往的好的命名出口。与默认/未命名的导出相比，它们有足够的优势，并且在迁移项目或调整遗留代码以实现更好的互操作性时，它们让您的生活更加轻松。

[***节省自己大量的时间，专注于重要的主题。***](https://arnoldcodeacademy.ck.page/26-web-dev-cheat-sheets)

延伸阅读:

[](https://medium.com/next-level-source-code/enums-typescript-4-0-and-javascript-guide-all-you-need-to-know-5e090355bff6) [## Enums TypeScript 4.0 和 JavaScript 指南—您需要知道的一切

### 你将读到的关于 enums 的最后一个指南！

medium.com](https://medium.com/next-level-source-code/enums-typescript-4-0-and-javascript-guide-all-you-need-to-know-5e090355bff6) [](https://medium.com/next-level-source-code/do-you-follow-these-10-principles-for-good-programmers-1445727af447) [## 你遵循了优秀程序员的这 10 条原则吗？

### 从接吻和干燥到足球和 YAGNI 和聪明屁股代码的 10 个原则！

medium.com](https://medium.com/next-level-source-code/do-you-follow-these-10-principles-for-good-programmers-1445727af447) [](https://medium.com/next-level-source-code/javascript-es6-var-let-or-const-88da65f3a0df) [## JavaScript ES6 Var，Let 或 Const

### 掌握 JavaScript 中的变量，避免用 var，let，const 提升

medium.com](https://medium.com/next-level-source-code/javascript-es6-var-let-or-const-88da65f3a0df) 

参考资料:

*   [1]生活:[https://developer.mozilla.org/en-US/docs/Glossary/IIFE](https://developer.mozilla.org/en-US/docs/Glossary/IIFE)
*   【2】生活:[https://flaviocopes.com/javascript-iife/](https://flaviocopes.com/javascript-iife/)
*   [3]节点的诞生。Js 和生日:[https://nodejs.dev/learn/a-brief-history-of-nodejs](https://nodejs.dev/learn/a-brief-history-of-nodejs)
*   [4]常见:[https://requirejs.org/docs/commonjs.html](https://requirejs.org/docs/commonjs.html)