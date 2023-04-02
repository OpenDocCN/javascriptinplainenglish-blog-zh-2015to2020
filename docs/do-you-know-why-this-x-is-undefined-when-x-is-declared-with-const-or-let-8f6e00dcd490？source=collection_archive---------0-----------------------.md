# 知道为什么用 Const 或 Let 声明 x 时 this.x 是未定义的吗？

> 原文：<https://javascript.plainenglish.io/do-you-know-why-this-x-is-undefined-when-x-is-declared-with-const-or-let-8f6e00dcd490?source=collection_archive---------0----------------------->

## Java Script 语言

## 这是因为 ECMAScript 规范

![](img/d829936b37aec4d2c39887d0e295273c.png)

Photo by [Hannah Busing](https://unsplash.com/@hannahbusing?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我在为帖子做一些例子。这是关于普通函数和箭头函数的区别。我在变量声明中使用了关键字`const`,并创建了一个从内部调用外部变量的函数，如下所示。

```
const x = 1;function f() {
  console.log(this.x);
}f(); // **undefined**
```

但给出的结果是`undefined`。(在你的控制台上运行这个)所以我想为什么…这有什么问题…

我钻研了 ECMAScript 规范来找出确切的原因，因为没有“只是”，就像“它只是未定义的”。我通过研究文档编造了我的故事。

**免责声明**
你可能对此有不同的观点。当然，我可能是错的，因为我没有研究 v8 代码。因此，如果你有不同的想法，请随时发表评论，并与我们分享你的想法。

# 执行上下文和领域

我认为你应该对 JavaScript 中的执行上下文有一个基本的了解。如果你完全不知道这是什么，我建议你读一读我关于执行上下文的帖子。

[](https://medium.com/better-programming/execution-context-lexical-environment-and-closures-in-javascript-b57c979341a5) [## JavaScript 中的执行上下文、词汇环境和闭包

### 你应该知道的高级 JavaScript 概念

medium.com](https://medium.com/better-programming/execution-context-lexical-environment-and-closures-in-javascript-b57c979341a5) 

根据文档，对于所有的执行上下文，有三个组成部分——代码评估状态、函数和领域。我们将研究第三个组件，领域。

领域是一种与 ECMAScript 资源相关联的世界。并且运行执行的领域被称为*当前领域*。为什么这是一个问题？当您执行 JavaScript 代码时，它会创建一个全局执行上下文，其当前领域*是全局领域。在这个领域中，一个名为 *globalEnv* 的对象被创建和 envolved，它包含一个词法环境。*

# 全球词汇环境

每个词法环境都有一个环境记录，其中包含您的变量并不断跟踪它们。

```
var name = 10;var globalLexicalEnvironment = {
  environmentRecord: {
    name: name
  }
};
```

在全局执行上下文中创建的词汇环境包含了`window`对象。

但是为什么和如何？

当生成全局执行上下文时，它的环境记录被创建，它被认为是所有 ECMAScript 元素共享的最外层范围。在全球环境记录中有一个领域，换句话说，它是最外层的范围。这个环境记录为内置变量或函数提供绑定。这是那些内置东西的列表。

*   无穷
*   圆盘烤饼
*   不明确的
*   evaluate 评价
*   排列
*   布尔代数学体系的
*   …

这些属性被分配到的对象被称为`window`，对于浏览器 JavaScript 来说。这种关系可以表示如下，然而，当然，这不是 JavaScript 的工作方式。

```
var window = {
  Infinity: ...,
  NaN: ...,
  undefined: ...,
  eval: ...,
  Array: ...,
  Boolean: ...,
};var globalExecutionContext = {
  environmentRecords: {
    window: window
  }
};
```

# Var 与 Const & Let

那为什么`var`变量变成了`window`的财产而`const`和`let`没有呢？如果你到目前为止都很好的跟着我就很好理解了！

当事物被声明并分配给全局环境记录中涉及的名为`window`的对象时，会有另一个声明被添加到全局环境记录中。它叫做*TopLevelVarDeclaredNames*。它定义了 JavaScript 如何处理顶层范围内的`var` ed 变量。根据规范，在程序的开始是一个空数组。

问题是，没有*topleveletdeclarednames*或*toplevelcontdeclarednames*。意思是，`const` ed 和`let` ed 变量不添加到全局对象，`window`。

# 那我就永远不能访问它们了？

你可以。别担心。但这肯定不是推荐的方式。

```
const x = 1;function print() {
  console.log(this.x);
}window.x = x;
print(); // 1
```

创建变量，赋值，并将它们放入`window`。

清晰，简单，但不好。(仅作为示例)

# **结论**

“为什么不能访问用`const`或`let`关键字声明的变量”的结论非常简单。当程序开始运行时，会创建全局执行，并且所有 ECMAScript 元素的最外层作用域(称为全局领域)的公共区域会被赋予属性。`var` ed 变量可以被访问，因为全局领域知道如何处理，但是对于`const` ed 和`let` ed 变量，全局领域什么也做不了。没有任何关于他们的报道。(没有将`const`和`let`作为全局变量的声明或文档)

# 资源

*   [执行上下文— ECMAScript 规范](https://www.ecma-international.org/ecma-262/6.0/#sec-execution-contexts)
*   [代码领域— ECMAScript 规范](https://www.ecma-international.org/ecma-262/6.0/#sec-code-realms)
*   [词法环境— ECMAScript 规范](https://www.ecma-international.org/ecma-262/6.0/#sec-lexical-environments)
*   [全球环境记录— ECMAScript 规范](https://www.ecma-international.org/ecma-262/6.0/#sec-global-environment-records)
*   [全局对象— ECMAScript 规范](https://www.ecma-international.org/ecma-262/6.0/#sec-global-object)
*   [顶级词汇范围声明— ECMAScript 规范](https://www.ecma-international.org/ecma-262/6.0/#sec-block-static-semantics-toplevellexicallyscopeddeclarations)