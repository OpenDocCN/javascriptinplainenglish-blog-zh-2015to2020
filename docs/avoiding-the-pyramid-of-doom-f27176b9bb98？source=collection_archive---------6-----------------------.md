# 避开末日金字塔

> 原文：<https://javascript.plainenglish.io/avoiding-the-pyramid-of-doom-f27176b9bb98?source=collection_archive---------6----------------------->

## 因为嵌套代码不利于心理健康

![](img/e3b9b99a268eae2196ea24adad83688c.png)

Picture courtesy of [Thais Cordeiro](https://unsplash.com/@thaiscord)

我经常在看大三写的代码时，看到很多嵌套的代码。在这篇文章中，我将试图阐明为什么它是有问题的，以及应该怎么做。

# 对这个问题的简单说明

让我们一起来看一个微不足道的例子:

这个函数足够简单；它要么接收输入，要么不接收。目前，这个函数的几乎所有代码都受到一个`if`语句的“保护”。也就是说，如果不满足条件，几乎不会执行任何代码。如果不是，那么只需要执行一个日志语句。

对于这样一个简单的函数，这不是一个*大*问题，但它仍然是一个问题。为什么？

# 根本问题

像前面这样的函数是有问题的，因为它们嵌套了不需要嵌套的代码。嵌套代码更难阅读/理解。

只要有机会，就删除不必要的嵌套，这样做可以降低代码的复杂性。

有一个有趣的代码质量指标叫做[圈复杂度](https://en.wikipedia.org/wiki/Cyclomatic_complexity)，它基于控制流图来评估程序的复杂度。如果没有条件语句/决策点，那么复杂度为“1”，因为代码中只有一条路径。

如果有一个`if`，那么现在有两条不同的路径，复杂度上升到“2”。如果在那个`if`中有一个条件语句或决策点，那么复杂度上升到“3”，等等。

因此，从根本上来说，这个想法是“嵌套”越少，代码就越不复杂。而且，对我来说，不太复杂的代码几乎总是赢，不管功能是否小/简单。

我举的例子可能微不足道，但这个原则已经适用了。代码库更复杂的部分肯定会有两层或更多的嵌套，导致臭名昭著的[末日金字塔](https://en.wikipedia.org/wiki/Pyramid_of_doom_(programming))。例如，当您链接回调时，也会发生同样的情况，因此相同的基本问题会出现不同的情况:随着嵌套层次的增加，复杂性也会增加。

那么我们能做些什么来降低这些代码的复杂性呢？

> 提示: [TSLint](https://palantir.github.io/tslint/rules/cyclomatic-complexity/) 和 [ESLint](https://eslint.org/docs/rules/complexity) 都有内置的规则，可以检查你的代码的圈复杂度，所以要确保这些都被启用了。

# 使用 guard 语句的简单代码

这是我们玩具例子的一个稍微好一点的版本:

在这里，我删除了大部分代码的一层嵌套，并完全删除了`else`分支。

`if (!noteContent) { ... return; }`检查就是[马丁·福勒](https://martinfowler.com/)在他的[重构](https://martinfowler.com/books/refactoring.html)一书中所说的*守卫语句*。顾名思义，保护语句是用来保护代码的其余部分的。

在这种情况下，如果没有输入(`null`论证)，那么继续下去就没有意义；于是有了`return`。因为如果没有达到最小期望值，我们就简单地离开函数，那么代码的其余部分就不需要保持嵌套。

# 但是应该有单一的退货单吧？！

[代码完成](https://www.amazon.com/Code-Complete-Practical-Handbook-Construction/dp/0735619670)其他来源有时不建议在一个函数中有多个 return 语句。一般来说，我同意并坚持这样做，因为它通常会简化调试，但在像这样的简单情况下，我坚信最好的方法是立即返回并简化代码的其余部分。

正如在几个 [StackOverflow 问题](https://stackoverflow.com/questions/36707/should-a-function-have-only-one-return-statement)中解释的那样，单次返回有时会导致如下噩梦般的代码:

我不知道你怎么想，但是如果我偶然发现了这样的代码，我会收拾好自己的东西，飞得越远越好，再也不回头；-)

# 结论

在这篇小文章中，我解释了为什么嵌套是有问题的，即使对于简单的函数。圈复杂度是一个需要控制的重要的代码质量指标，像这样的简单技巧确实有助于简化代码。

我还给出了一个例子，说明如何提高代码质量，从而提高代码的可维护性。

每当你想引入嵌套时，考虑一下这一点，并尝试其他策略(比如引入适合目的的函数)来避免不必要的复杂性；这个世界已经够复杂了；-)

今天到此为止！

PS:如果你想学习大量关于软件/Web 开发、TypeScript、Angular、React、Vue、Kotlin、Java、Docker/Kubernetes 和其他很酷的主题的其他很酷的东西，那么不要犹豫[去拿一本我的书](https://www.amazon.com/Learn-TypeScript-Building-Applications-understanding-ebook/dp/B081FB89BL)并订阅[我的简讯](https://mailchi.mp/fb661753d54a/developassion-newsletter)！