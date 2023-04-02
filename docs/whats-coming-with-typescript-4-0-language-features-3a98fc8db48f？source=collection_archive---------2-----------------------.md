# TypeScript 4.0 带来了什么—语言特性

> 原文：<https://javascript.plainenglish.io/whats-coming-with-typescript-4-0-language-features-3a98fc8db48f?source=collection_archive---------2----------------------->

[TypeScript 3.9](https://medium.com/@dSebastien/whats-new-with-typescript-3-9-9095ff9f68a5) 刚刚[发布](https://medium.com/@dSebastien/whats-new-with-typescript-3-9-9095ff9f68a5)，自然要看看接下来的内容了！好消息是，由于 TypeScript 是公开开发的，所有人都可以看到这些计划。

![](img/b110b79b2c5820c3801ed3d0f2cd6af8.png)

在这篇文章中，我将尝试总结我通过查看 [4.0 迭代计划](https://github.com/microsoft/TypeScript/issues/38510)所能发现的内容。我将只讨论语言特性。我可能会写一些额外的帖子来涵盖编辑器的生产力、性能和错误修复。

请记住，本文是基于路线图，并不意味着一切都将成为 4.0 的一部分，甚至实际上已经实现！；-)

# 从构造函数推断类属性

目前，当 tsc 配置为`noImplicitAny`模式时，以下 TS 代码不会编译:

现在[这个 PR](https://github.com/microsoft/TypeScript/pull/37920) 已经被合并，因此从 TS 4.0 开始，*上面的代码将*编译，TypeScript 将推断出`x`的类型为`string | boolean`。

这是 TypeScript 的类型推断将帮助我们的又一个例子！

# 短路赋值运算符

Daniel Rentz 介绍的这个[提案](https://github.com/microsoft/TypeScript/issues/37255)对应的是一个 [TC39 提案，名为“提案-逻辑-分配”](https://github.com/tc39/proposal-logical-assignment)，目前处于第 3 阶段(即[基本就绪](https://tc39.es/process-document/))！).

它旨在结合逻辑运算符和赋值表达式。结合从 TS 3.7 开始我们就有的 [nullish 合并](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-7.html#nullish-coalescing)，我们将能够编写更精简的代码。

这是提案中给出的一个例子:

```
obj1.obj2.obj3.x ??= 42;
```

没有这些新的短路操作符的相同代码:

```
obj1.obj2.obj3.x = obj1.obj2.obj3.x ?? 42;
```

如你所见，有了这种支持，我们将拥有一种更具表达力的语言，我们将能够结合检查和赋值，这将是非常棒的。

正如 Daniel Rosenwasser 所提到的，我们会为每个逻辑运算符设置一个这样的运算符，因此:

```
*LeftHandSideExpression* &&= *AssignmentExpression**LeftHandSideExpression* ||= *AssignmentExpression**LeftHandSideExpression* ??= *AssignmentExpression*
```

对应于我们目前可以做的事情:

```
*LeftHandSideExpression* && (*LeftHandSideExpression* = *AssignmentExpression*)*LeftHandSideExpression* || (*LeftHandSideExpression* = *AssignmentExpression*)*LeftHandSideExpression* ?? (*LeftHandSideExpression* = *AssignmentExpression*)
```

鉴于相应的提案已经进入第三阶段，我敢打赌这将是 TS 4.0 的一部分，这将是一个伟大的，因为这是一个非常好的功能。

# 允许未知的 catch 子句绑定

目前，如果您尝试向 catch 子句添加类型注释，编译器会报错:

上面的代码无法编译，并引发以下错误:`TS1196: Catch clause variable cannot have a type annotation`

在这一点上，我们根本不能向 catch 子句添加类型注释，从类型安全的角度来看，这是相当可悲的。问题是错误总是被认为是`any`，它允许我们在 catch 块中对对象做任何事情。

这种行为仅仅是因为，最初，`unknown`关键字并不存在。但是现在，在这里使用它会更有意义。

正如这个提案的[的评论中指出的，我们可以得到一个新的严格标志，让我们在默认情况下强制执行这个(即，让所有的 catch 子句错误成为未知类型)。这将迫使我们在块中使用它之前正确地检查类型。](https://github.com/microsoft/TypeScript/issues/36775)

这是我真正感兴趣的一项改进！

# 标记元组元素

另一个[提议](https://github.com/Microsoft/TypeScript/issues/28259)，由 Brian Kim 提出，旨在给我们定义元组元素标签的能力。我不是元组的最大粉丝(我通常更喜欢对象/自定义类型)，但有时它们确实很有用，例如在编写测试时(或者像 React :p 这样奇怪的库的类型定义)。

目前，元组是这样声明的:

```
// length, count
type Segment = [number, number];
```

因为我们不能给元组元素分配标签，所以最简单(但是很难看)的解决方案是依靠注释来提醒我们每个元素对应什么。

另一个解决方案(更干净)是使用具有更有用名称的自定义类型。尽管如此，仍有改进的空间。

一些语言如 c#[和 Python 支持这一点。](https://docs.microsoft.com/en-us/dotnet/csharp/tuples#named-and-unnamed-tuples)

如果这被添加到语言中，那么我们将能够更简单地创建更具表达力的元组:

```
type Segment = [length: number, count: number];
```

在这里，通过查看元组，我们可以直接知道每个数字对应于什么。

这对于清楚地理解元组是由什么组成的非常有用。此外，正如提案中提到的，它还将为操纵/返回元组的 API 增加更多的表达能力。

正如 Daniel Rosenwasser 所说，元组元素名称不会在类型系统中强制任何东西；它们的存在纯粹是为了传达意图。

# 远期申报

正如 [Daniel Rosenwasser 解释的](https://github.com/microsoft/TypeScript/issues/31894)，有时候我们需要告诉 TypeScript 一个类型*可能*存在，这取决于代码执行的环境。

当出现这种情况时，我们可以利用[声明合并](https://www.typescriptlang.org/docs/handbook/declaration-merging.html)。这是可行的，但是有一些问题，特别是当不同的环境有冲突的类型时。此外，它只适用于自定义类型和接口。

有了这个[新提议](https://github.com/microsoft/TypeScript/issues/31894)，TypeScript 将通过让我们定义占位符类型来为我们提供正确处理这种情况的方法，占位符类型将在实现可用之前充当占位符。此时，TypeScript 将表现为占位符类型不再存在。

这是提案中的一个例子:

(提议的)新的`exists`关键字将让我们定义那些占位符类型。

然后，当引入实际的实现类型时，TS 编译器将确保实现遵守占位符定义的约束:

如果实现类型不考虑占位符类型约束，那么编译器将退出:

请注意，`exists`关键字只是建议中的一个，还没有确定的选择。

查看[提案](https://github.com/microsoft/TypeScript/issues/31894)了解更多详情。

# 更好地支持承诺和等待

在 TS 3.9 的发行说明中，有一个最初我不理解的关于潜在的“等待”关键字的说明[，声明它还没有准备好进入黄金时段。](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-9.html#what-about-the-awaited-type)

自从我在 2016 年爱上 Rx 和 Observables(哈哈)以来，我就不是一个承诺的超级粉丝，但在看了 TS 4.0 的计划后，我偶然发现了这个 [bug 报告](https://github.com/microsoft/TypeScript/issues/27711)，它帮助我理解了这是怎么回事。

在 JavaScript 中，嵌套的承诺总是打开的；它们会自动变平。这里有一个例子来说明我的意思:

无论一个承诺被包装了多少次，它都会自动为我们打开。这在现实世界中也是可行的。如果你把礼物包在盒子里的盒子里的盒子里，然后给孩子们，你会看到它是如何工作的；-)

但是在 TypeScript 中，JavaScript 的这种正常行为目前并没有正常工作。以下代码编译正常，但类型不正确:

对于上面的代码，编译器应该会抱怨，因为`p1`承诺实际上是一个`Promise<number>`而不是一个`Promise<Promise<number>>`。

实际上，在 TypeScript 中还有一些关于承诺和 await 关键字的其他问题，正如在[这篇文章](https://github.com/microsoft/TypeScript/pull/17077)中所解释的。TypeScript 目前很难找到用于复杂承诺链的正确返回类型。此外，TS 也很难正确识别类型变量的“等待类型”。

TypeScript 团队打算引入一个`awaited T`操作符来明确定义“等待类型”。这旨在正确地模仿 JavaScript 的运行时行为，但它没有涵盖上面的承诺解包问题。

由于这仍在不断变化，还不清楚这将如何演变，以及 4.0 中将会有什么。不过有一点是肯定的:TypeScript 团队肯定会在即将到来的版本中改善 TS 中承诺的现状；-)

# 调整 TypeScript 对 React 的支持

就像 TypeScript 一样，React 移动速度非常快。

自从我出版了关于 TypeScript、React、Angular 和 Vue 的书之后，事情一直在发展。我关于 React 的章节仍然相关，但是 React.createElement API 正在[改变](https://github.com/reactjs/rfcs/blob/createlement-rfc/text/0000-create-element-changes.md)。

由于 TypeScript 支持 JSX，它确实需要遵循这些发展。这在[本期](https://github.com/microsoft/TypeScript/issues/34547)有追踪。

# 结论

在本文中，我已经分享了我到目前为止所发现的关于 TypeScript 4.0 中可能出现的内容。

我等不及要看我们会得到什么了！

# 喜欢这篇文章吗？点击下面“喜欢”按钮查看更多内容，并确保其他人也能看到！

PS:如果你想学习大量关于软件/Web 开发、TypeScript、Angular、React、Vue、Kotlin、Java、Docker/Kubernetes 和其他很酷的主题的其他很酷的东西，那么不要犹豫[去拿一本我的书](https://www.amazon.com/Learn-TypeScript-Building-Applications-understanding-ebook/dp/B081FB89BL)并订阅[我的简讯](https://mailchi.mp/fb661753d54a/developassion-newsletter)！

# **简明英语笔记**

你知道我们推出了一个 YouTube 频道吗？我们制作的每个视频都旨在教给你一些新的东西。点击此处 查看我们，并确保**订阅频道**😎