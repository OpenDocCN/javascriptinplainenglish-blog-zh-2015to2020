# JavaScript 实用程序库

> 原文：<https://javascript.plainenglish.io/javascript-utility-libraries-d9aad9ea2401?source=collection_archive---------4----------------------->

![](img/bf5904525d4032e5d99894db6c6d75ec.png)

# JS、Ramda 和 Lodash 的比较

有了 [*ECMAScript 2020*](https://tc39.es/ecma262/) 可用，函数式编程(FP)就不需要外部库了——特别是 currying 和 composition。这类工作的两个主要库是 [Ramda](https://ramdajs.com/) 和 [Lodash FP](https://github.com/lodash/lodash/wiki/FP-Guide) 。UnderscoreJS 是另一个，但是 Lodash 通常被认为是对这个库的改进。Lodash 是下划线的一个分支，它为什么分支的[历史相当有趣。](https://stackoverflow.com/a/13898916)

然而，对于更复杂的 FP 情况，使用这些久经考验的库仍然是一个好主意。如果没有利用这些复杂的场景，普通的 JavaScript 在很大程度上可以跟上实用程序库。一些值得注意的例外是来自 [Lodash](https://lodash.com/) 的`debounce`和来自 Ramda 的`merge`。

重申一下，导致使用 Ramda 和 Lodash 的许多好处已经融入到普通 JavaScript 中。箭头函数允许一个 currying 版本，与链接函数一起，可以充分地组合函数。类似地，每个版本都添加了原型方法，这使得 Lodash 越来越没用了。

*注意*:箭头函数不允许*实际的*(即`(a, b) => {}`与`a => b => {}`相同，即函数本身跟踪它定义了多少个参数)，非常接近。

本文将:

*   简要概述 Ramda 和 Lodash (FP)
*   请注意投资图书馆是否有意义的案例
*   给出几个突出的方法的上下文
*   提供一个表格摘要，说明哪个图书馆在哪些方面更好
*   提供一个 [**REPL**](https://repl.it/@irmerk/Comparing-Utility-Libraries) 和 [**库**](https://github.com/irmerk/javascript-utilities-ramda-lodash) 用于生成基准

所有这些都是公开的，这意味着你可以自由地对列表做出贡献和调整

# Java Script 语言

如前所述，在过去几年中，原生 JavaScript 变得更加强大。虽然助手和实用程序库仍然很有帮助，但是它们中的大部分内容都可以简化为`filter()`、`map()`和`reduce()`的某种组合。

我在我的[现代 Javascript 技术](https://medium.com/better-programming/modern-javascript-techniques-cf2084236af4)文章中写了更多。

# 使用案例:

*   所需的功能很简单，只需要很少的步骤或转换
*   需要一些额外步骤的复杂功能并不是一个障碍
*   束尺寸很重要
*   从其他库中学习进入这些简化的助手函数的过程

# 拉姆达

Ramda 强调更纯粹的功能风格，不变性和无副作用的功能是设计哲学的核心。Ramda 是关于*转换*数据和*组成*函数。这就是为什么像`throttle`和`debounce`这样的东西不被支持，因为它们涉及副作用。为了以一种纯粹的方式实现这一点，[功能性反应式编程](https://en.wikipedia.org/wiki/Functional_reactive_programming)将需要用事件流对此进行抽象。

Ramda 功能是自动执行的。这样就可以通过不提供最终参数来轻松地从旧函数构建新函数。排列 Ramda 函数的参数是为了方便 currying。要操作的数据通常最后提供。这最后两点使得将函数构建为更简单的函数序列变得非常容易，每个函数都转换数据并传递给下一个函数。Ramda 被设计成支持这种风格的编码。

> *Ramda 提供了几个函数，当应用于不合适的输入时，返回有问题的值，如*`*undefined*`*`*Infinity*`*或* `*NaN*` *。这些被称为* [*部分功能*](https://en.wikipedia.org/wiki/Partial_function) *。部分功能需要使用防护装置或零位检查。**

*对此的补救办法可能是 [Sanctuary](https://sanctuary.js.org/) ，一个受 [Haskell](https://www.haskell.org/) 和 [PureScript](http://www.purescript.org/) 启发的 JavaScript 函数式编程库。它比 Ramda 更严格，并提供了类似的功能套件。*

# *使用案例:*

*   *合成，最后获取数据，始终保持一致*
*   *特定方法，通常涉及复杂操作，例如`merge`、`assoc`、`pluck`...*
*   *类似的常用方法在多个地方使用*
*   *使用`R.converge()`进行复杂的非线性合成*

# *洛达什*

*这里没什么可研究的。Lodash 是一个非常高效的实用程序库。虽然包的大小在过去是一个问题，但是 Lodash 在格式上变得更加模块化。这使得像 webpack 和 parcel 这样的构建工具能够进行树抖动并删除任何未使用的函数，从而减小包的大小。*

*请记住，有许多功能[可以在本机](https://github.com/you-dont-need/You-Dont-Need-Lodash-Underscore)中完成。*

**注意*:虽然 Lodash 在下面使用`_.toString()`方法的基准测试中出现得更快，但结果实际上与 JS 和 Ramda 中的相同函数并不相同。*

# *使用案例:*

*   *`debounce`*
*   *类似的常用方法在多个地方使用*

# *Lodash FP*

*Lodash 提供了`lodash/fp`，一个促进更函数式编程风格的模块。这个模块允许 Lodash 函数的可修改版本。这使得 Lodash 成为 Ramda 的良好替代品。*

# *使用案例:*

*   *合成，最后获取数据，始终保持一致*

# *基准测试结果*

*请注意，我已经用我和我的团队使用的常用方法开始了这个列表，它绝不是详尽的。请随意查看[库](https://github.com/irmerk/javascript-utilities-ramda-lodash)并打开一个 pull 请求来添加更多的方法或测试。*

# *结论*

*Ramda 和 Lodash 重叠，不应该在同一个项目中使用。根据您正在处理的数据和使用的方法，这些库可能非常有用，也可能没有必要。*

*应该采取一种“香草-JavaScript-优先”的方法，这些库不应该被用作数据方法的一揽子方法。一旦你遇到用普通 JavaScript 很难完成的事情，就切换到这些库中的一个。哪一个？归结为味道。两者的语义风格非常相似。*

*Ramda 通常是函数式编程的一种更好的方法，因为它是为此而设计的，并且有一个在这种意义上建立的社区。*

*当需要特定功能时，Lodash 通常更好。`debounce`)。*

*无论哪种方式，**都要确保投资树抖动**来最小化这些库的包大小，因为很可能你只会使用一些方法，而不需要整个库。*