# 在 TypeScript 中尝试/捕捉

> 原文：<https://javascript.plainenglish.io/the-dark-side-of-typescript-try-catch-deeded18ba0d?source=collection_archive---------0----------------------->

## 用 unknown 转义 Typescript 4.0 中的任何错误异常

![](img/440715f87014160c9aa9c69e07aed506.png)

[Jakob Owens](https://unsplash.com/@jakobowens1) via [unsplash](https://unsplash.com/photos/PVPsGlhs3rU)(CC0)

# 他们是故意这样做的吗？

TypeScript 涵盖了 JavaScript 的许多概念。还有，有问题的。一些错误处理案例可能相当有趣，但是本文将引导您穿越丛林！

微软将 TypeScript 描述为 JavaScript 的超集。我告诉你这些是什么意思？简单地说，每个 TypeScript 代码也是有效的 JavaScript 代码。TypeScript 可以做得比其父代更多，但不能更少。如果不是这样，项目的迁移就不会像现在这样容易，开发人员的接受程度也会降低。

正因为如此，他们也拿走了不好的。比如异常处理。如果分解一下，错误处理与 C#中的非常相似。 *Throw* 用于触发异常。当你猜对了这一个，你也会猜到下一个:*试着用&抓*来对付它们。因此，异常是默认的还是用户定义的无关紧要。

*最后*也成了打字稿。你只能使用 *try-finally* ，这里不需要 catch。*最后*用于确保我们清除不需要的内存分配。但这不是什么新鲜事，这已经实施了很多年了。

# 它甚至不必是错误类型？！

尽管 JavaScript 没有经典的类型系统，但它提供了几种错误类型。*错误* —这是用于任何场合的全球化错误类型。 *SyntaxError* —被指定为在发生语法错误时抛出，而 *ReferenceError* —当任何值被赋给未声明的变量或赋值时没有使用 *var* 关键字，或者变量不在我们当前的范围内[1]。

这种语言区分了这三种类型，并在合适的情况下抛出其中一种。或者在我们的例子中，代码有错误。Typescript 派生了所有这些错误，这意味着下面的代码是有效的。

有一个问题在我脑海中浮动。参数异常有什么类型？我认为很明显这必须是类型*错误*或者至少是一个与所有其他命名错误类有共同点的类型。某种错误——接口。

参数的类型是 *any* 。这使得我们想做什么就做什么，因为编译器不会抱怨，不管我们用参数*异常*做什么。我们将可以访问任何已定义的属性。Typescript 不应用严格使用的类型安全。

这里有两个问题:

1.  为什么会这样？
2.  为什么这会有意义？

我用几秒钟的时间回答了第一个问题:JavaScript 并没有强迫我们将错误作为异常使用。你甚至不需要从它那里得到什么，你可以扔任何你想要的东西，一个球，一个坚果，一个棒球棍…它就在你的手中。

这个投掷是无意义的，但是语法上是正确的。为了执行这段代码，程序将捕捉到值 42。宇宙万物的答案。

这就是第一个问题的答案。Typescript 从 any 进行推断，因为它无法猜测甚至不知道程序的其他部分会抛出什么代码。

接下来是这个问题的答案，如果这是好的？

# 错误错误错误

我真的不认为这是好事。为什么？因为 Typescript 应该制止它，而不是支持不安全的错误类型！

但是你怎么能这样做呢？向后兼容性是 Typescript 给我们开发人员的主要承诺。它可以简单地使用任何类型之外的另一种类型。比如*未知*。这不会有太大的区别，因为*未知*限制了属性的可访问性，甚至不给这样做的机会。这是有原因的，我们不知道未知是什么，所以不去接触一个我们甚至不知道的东西是合乎逻辑的。

不幸的是，这并没有带来更多的类型安全，而是带来了一点一般性的安全，因为它不允许我们访问属性。从 Typescript 4.0 开始，可以在 catch 语句中声明一个类型。我们强迫程序明确声明*任何未知的*或*。我们希望*未知*型达到我们的目标。*

*因为 linters 对每个开发人员和团队来说都是一种祝福，所以也有一个规则。*ESLINT-plugin*【2】得到了*no-implicit-any-catch*【3】的规则。这将帮助你迫使任何人去编码*尝试&捕捉*有用的东西。你甚至可以强制使用未知类型。这在类型安全方面非常有帮助。这就是为什么你使用 Typescript。*

# *铅字护板*

*因为您的代码中已经有许多任何类型的异常，您不希望手动更改它们，但是为了更好地处理异常，您最好转换它们。类型保护是一种有用的方法。这个函数必须检查一个值的类型是否正确，如果可能的话，就转换这个值。*

*当然，有一个 NPM 模块。是的，为什么不呢？名字是*defe kt*【4】。这个可以创建你自己的错误类。比如这个。*

# *结论*

*我也喜欢 Typescript & JavaScript。它们是很棒的语言，可以让简单的想法变得很快，每个人都可以理解。它们也非常适合制作非常酷和特别的 web 应用程序。但是 Typescript 必须摆脱的一件事是本文所涉及的主题。简单地利用 JavaScript 容易出错的旧属性，让它们保持原样。另一方面，如果没有与 JS 的向后兼容，TS 会是什么样呢？我真的不能对微软这样做提出指责。如果没有向后兼容，TS 也不会这么大。*

*为此，更重要的是要了解你的语言，了解你的语言是从哪里来的。JavaScript 是现代网络的基础。*

*[***节省自己大量的时间，专注于重要的主题。***](https://arnoldcodeacademy.ck.page/26-web-dev-cheat-sheets)*

*延伸阅读:*

*[](https://towardsdatascience.com/javascript-ecmascript-history-the-hidden-features-acb38af57be8) [## JavaScript ECMAScript 历史-隐藏的特性

### 关于 ECMAScript 版本中不流行的技巧的 JavaScript 历史指南

towardsdatascience.com](https://towardsdatascience.com/javascript-ecmascript-history-the-hidden-features-acb38af57be8) [](https://medium.com/next-level-source-code/if-else-and-switch-case-conditions-write-better-code-3-f7ca430eda62) [## If Else 和 Switch case 条件—编写更好的代码 3

### 如何通过修改 if-else 和 switch case 条件来编写更好的代码

medium.com](https://medium.com/next-level-source-code/if-else-and-switch-case-conditions-write-better-code-3-f7ca430eda62) [](https://medium.com/javascript-in-plain-english/javascript-break-promises-keep-callbacks-4dbf9cff3d9a) [## JavaScript 中的异步与同步编程

### 打破承诺，坚持回访

medium.com](https://medium.com/javascript-in-plain-english/javascript-break-promises-keep-callbacks-4dbf9cff3d9a) 

## 链接和参考

[1]异常处理
[https://basarat . git book . io/typescript/type-system/exceptions](https://basarat.gitbook.io/typescript/type-system/exceptions)
2)eslint-plugin
[https://eslint . org/docs/developer-guide/working-with-plugins](https://eslint.org/docs/developer-guide/working-with-plugins)
【3】无隐式任何 catch
[https://tinyurl.com/y8vckrmx](https://tinyurl.com/y8vckrmx)
【4】NPM modul defe kt
[https://www.npmjs.com/package/defekt](https://www.npmjs.com/package/defekt)*