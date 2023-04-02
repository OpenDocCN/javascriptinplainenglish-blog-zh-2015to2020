# TypeScript 4.0 的新增功能

> 原文：<https://javascript.plainenglish.io/whats-new-with-typescript-4-0-beta-a2e674846ef3?source=collection_archive---------1----------------------->

## TypeScript 4.0 有哪些新的亮点—语言特性

我们刚刚完成向 TS 3.9 的迁移，4.0 版本已经在这里了！

![](img/b110b79b2c5820c3801ed3d0f2cd6af8.png)

在本文中，我将回顾作为 [TypeScript 4.0](https://devblogs.microsoft.com/typescript/announcing-typescript-4-0/) 的一部分刚刚发布的所有内容。

我将只讨论语言特性。我可能会写一些额外的帖子来介绍编辑器的生产力、性能和错误修复。

# 从构造函数推断类属性

目前，当`tsc`配置为`noImplicitAny`模式时，以下 TS 代码不会编译:

现在[这个 PR](https://github.com/microsoft/TypeScript/pull/37920) 已经被合并，因此，从 TS 4.0 开始，*上面的代码将*编译，TypeScript 将推断出`x`的类型为`string | boolean`。

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

# 允许未知的 catch 子句绑定

目前，如果您尝试向 catch 子句添加类型注释，编译器会报错:

上面的代码无法编译，并引发以下错误:`TS1196: Catch clause variable cannot have a type annotation`

在这一点上，我们根本不能向 catch 子句添加类型注释，从类型安全的角度来看，这是相当可悲的。问题是错误总是被认为是`any`，这让我们可以对 catch 块中的对象做任何事情。

这种行为仅仅是因为，最初，`unknown`关键字并不存在。但是现在，在这里使用它会更有意义。

正如在[这个提议](https://github.com/microsoft/TypeScript/issues/36775)的注释中所指出的，我们可以得到一个新的严格标志，让我们在默认情况下强制这样做(也就是说，让所有的 catch 子句错误成为未知类型)。这将迫使我们在块中使用它之前正确地检查类型。

这是我真正感兴趣的一项改进！

# 可变元组类型

一个可怕的新功能的野蛮的名字。如果你还不了解元组，[先去了解那些](https://www.typescriptlang.org/docs/handbook/basic-types.html#tuple)。

我不是元组的最大粉丝(我通常更喜欢对象/自定义类型)，但有时它们确实很有用，例如在编写测试时(或者像 React :p 这样奇怪的库的类型定义)。

TypeScript 4.0 改进了类型推断，现在允许正确地对处理元组的函数进行类型化。

首先，它现在在定义元组类型时支持泛型，允许它们对元组元素使用在函数上定义的泛型类型。正如发行说明所述，这意味着我们可以表示元组和数组上的高阶操作，即使我们不知道我们正在操作的实际类型。

发行说明包括几个示例:

正如你在上面看到的，`tail`函数返回一个数组，或者类型为`T`的元素。这段代码写起来/理解起来都很简单，这太棒了。多亏了这个新功能，您可以看到`r1`和`r2`被正确地输入了。

另一个改进是扩展元素现在可以出现在元组中的任何位置；不仅仅是在结尾:

使用 TS 3.9.x 及更早版本，无法编译。有了 TS 4.0，我们将能够做到这一点，编译器将高兴地展平跨页，无论它们位于何处。

正如发行说明中所解释的，通过结合这两个特性，我们现在可以更好地键入类似于`concat`函数的东西:

厉害！

这些新的类型推断改进将对我们的代码质量产生巨大影响，我迫不及待地想在生产中使用它们。

更多详情请查看[完整发行说明](https://devblogs.microsoft.com/typescript/announcing-typescript-4-0-beta/)。例如，它们还涵盖了这将如何改进像函数组合和部分参数应用这样的用例。

# 标记元组元素

另一个由 Brian Kim 介绍的[提议](https://github.com/Microsoft/TypeScript/issues/28259)，旨在给我们定义元组元素标签的能力。

目前，元组是这样声明的:

```
// length, count
type Segment = [number, number];
```

因为我们不能给元组元素分配标签，最简单的(但非常丑陋的)解决方案是依靠注释来提醒我们每个元素对应的内容。

另一个解决方案(更干净)是使用具有更有用名称的自定义类型。尽管如此，仍有改进的空间。

一些语言，例如 [C#](https://docs.microsoft.com/en-us/dotnet/csharp/tuples#named-and-unnamed-tuples) 和 Python 支持这一点。

如果这被添加到语言中，那么我们将能够更简单地创建更有表现力的元组:

```
type Segment = [length: number, count: number];
```

这里，通过查看元组，我们直接知道每个数字对应的是什么。

这对于清楚地理解元组是由什么组成非常有用。此外，如提案中所提到的，它还将增加操纵/返回元组的 API 的表达能力。

正如 Daniel Rosenwasser 所说，元组元素名称不会在类型系统中强制执行任何操作；它们的存在纯粹是为了传达意图。

# 调整类型脚本对反应的支持

就像打字稿一样，反应速度快得惊人。

自从我出版了关于打字脚本、反应、角度和 Vue 的书以来，事情一直在发展。我关于反应的章节仍然相关，但是反应. createElement API 正在[改变](https://github.com/reactjs/rfcs/blob/createlement-rfc/text/0000-create-element-changes.md)。

由于 TypeScript 支持 JSX，它确实需要遵循这些发展。本期在[中对此进行了跟踪，已经打开了](https://github.com/microsoft/TypeScript/issues/34547)[PR](https://github.com/microsoft/TypeScript/pull/39199)。该修改应包含在 TS 4.0.1 中。

接下来，对定制 JSX 工厂的支持将登陆 TS 4.0，允许我们通过`jsxFragmentFactory`选项定制片段工厂。查看发行说明和 PR 之后的[以了解详细信息。](https://github.com/microsoft/TypeScript/pull/38720)

# 重大变化

TS 4.0 计划有一些突破性的变化:

*   lib.d.ts 已经更改(DOM 类型已经调整)，这意味着当升级到这个新版本时，我们可能会面临一些新的编译错误。首先，已经过时很久的属性`document.origin`已经被删除了
*   用属性覆盖访问器(反之亦然)现在在所有情况下都被认为是错误的。以前仅在使用`useDefineForClassfields`时出现错误。因此，如果您的派生类覆盖了基类的 getter/setter，那么 TS 4.0 就会出现编译错误。查看[公关](https://github.com/microsoft/TypeScript/pull/37894)了解详情
*   对于 TS 4.0，当处于`strictNullChecks`模式时(即始终，对吗？？！)，则`delete`运算符的操作数必须是`any`、`unknown`、`never`或者是可选的(即包含`undefined`)；否则，代码不会编译。查看[公关](https://github.com/microsoft/TypeScript/pull/37921)了解详情

# 贬值

由于 TS 4.0 引入了新的节点工厂 API，用于生成 TS AST 节点的旧工厂函数[已被弃用](https://github.com/microsoft/TypeScript/pull/35282)。

对于绝大多数项目来说，这不应该是一个问题。

# 结论

在本文中，我分享了 TypeScript 4.0 的新功能和亮点。

这种令人惊叹的语言继续以闪电般的速度发展。

迫不及待想把这个用在生产上！

# 喜欢这篇文章吗？点击下面“喜欢”按钮查看更多内容，并确保其他人也能看到！

PS:如果你想了解大量关于软件/Web 开发、TypeScript、Angular、React、Vue、Kotlin、Java、Docker/Kubernetes 和其他很酷的主题的其他很酷的东西，那么不要犹豫，去[拿一本我的书](https://www.amazon.com/Learn-TypeScript-Building-Applications-understanding-ebook/dp/B081FB89BL)并订阅[我的简讯](https://mailchi.mp/fb661753d54a/developassion-newsletter)！

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**