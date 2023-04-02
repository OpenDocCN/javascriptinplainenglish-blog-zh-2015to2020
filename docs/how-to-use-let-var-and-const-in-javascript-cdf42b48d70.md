# 如何在 JavaScript 中使用 let、var 和 const

> 原文：<https://javascript.plainenglish.io/how-to-use-let-var-and-const-in-javascript-cdf42b48d70?source=collection_archive---------1----------------------->

## 有 3 种方法可以在 JavaScript 中创建变量:let 是块范围，var 是函数范围，const 是块范围但不可变(即常量)。

![](img/1ace58afe13d6b599064c27ac296be53.png)

Photo by [François Kaiser](https://unsplash.com/@r3siak?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

TT`let`、`var`和`const`之间的区别是一个常见的采访问题，也是一个经常混淆的来源，而**很难解释**。

最近，我参加了一个关于这个主题的 15 分钟闪电演讲，其中包含了许多关于 JavaScript 解析、执行上下文和提升的伟大细节。

但是，我发现自己仍然很难解释这个概念，直到我这样写下:

*   `let` ==块范围
*   `var` ==功能范围
*   `const` ==区块范围像让但不可变

下面是一个简单的例子:

让我们来看看`let`、`var`和`const`在 JavaScript 中的实际含义。

![](img/40a813a225dcf5a817ae75c2519d35f6.png)

Photo by [Bryan Minear](https://unsplash.com/@bryanminear?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# `let` ==块范围

[区块范围](https://medium.com/@josephcardillo/the-difference-between-function-and-block-scope-in-javascript-4296b2322abe)表示发生在代码区块内。

这是许多程序员期望变量发挥作用的程度，因为大多数编程语言只有块范围。

使用`let`关键字初始化变量对于我们希望限制对变量的访问或仅临时需要变量时非常有用。

一个很好的例子是写一个`for`循环:

![](img/23bf6a9f55d614428600fd0212fd25f6.png)

Photo by [Ankush Minda](https://unsplash.com/@an_ku_sh?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# `var` ==功能范围

[功能范围](https://dev.to/sandy8111112004/javascript-introduction-to-scope-function-scope-block-scope-d11)表示发生在某个功能内。

在 JavaScript 中，由于[执行上下文](https://medium.com/@js_tut/execution-context-the-call-stack-d1fbe34f6fe9)和[提升](https://medium.com/front-end-weekly/javascript-understanding-hoisting-585e2191b4bf)的概念，功能范围不同于块范围。

将此与函数中未包含`var`关键字的情况进行比较:

![](img/c0fcc59ff60cb85bf2ed079aae984118.png)

Photo by [Toa Heftiba](https://unsplash.com/@heftiba?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# `const` ==区块范围与`let`类似，但不可变

和`let`一样，`const`也使用块作用域。但是`const`创建了一个[常量](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)，一个*不可变的*变量，其值不能改变。

这里，不可变意味着引用不能被改变，因为像数字和字符串这样的原始值在 JavaScript 中也是不可变的。

请注意，对象是一个例外，因为它们的属性仍然可以更改。根据个人偏好，一些程序员将 const 对象视为实际上不可变的，并试图根本不改变它们的数据。

下面是一个使用`const`关键字初始化变量的例子:

![](img/da9b40fe9e83b6f012dae63720913a04.png)

Photo by [Drew Dau](https://unsplash.com/@daunation?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 词汇课:提升和关闭

如果不简要提及**提升**和**闭包**的概念，任何关于 JavaScript 词法范围的文章都是不完整的。

## 什么是吊装？

术语“ [**提升**](https://medium.com/javascript-in-plain-english/https-medium-com-javascript-in-plain-english-what-is-hoisting-in-javascript-a63c1b2267a1) ”是指在代码运行时，所有的 var 和函数声明都被拉到代码的顶部(或被提升)。

> 当 Javascript 编译您的所有代码时，所有使用`*var*`的变量声明都被提升到它们的函数/局部范围的顶部(如果在函数内声明)或全局范围的顶部(如果在函数外声明)，而不管实际声明是在哪里进行的。— [苏尼尔·桑德胡](https://medium.com/u/a7b125868703?source=post_page-----cdf42b48d70--------------------------------)在[用简单英语写的 JavaScript](https://medium.com/javascript-in-plain-english/https-medium-com-javascript-in-plain-english-what-is-hoisting-in-javascript-a63c1b2267a1)

请注意，变量声明被挂起，但值赋值没有被挂起。这意味着提升可能会产生意想不到的结果:

但是删除一个`var`关键字会由于提升而改变行为:

使用`let`或`const`时不会出现这种情况。这里有一个例子:

![](img/d73a9ad9ac696a4f8ca5dfe7ceee8755.png)

Photo by [Hannah Busing](https://unsplash.com/@hannahbusing?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 什么是闭包？

一个“ [**闭包**](https://medium.com/javascript-in-plain-english/examples-of-closures-in-javascript-9bc397bbf1a1#e3cb) ”包含了创建函数定义时作用域内的所有变量。它保持相同的词法范围。

> 当一个函数返回一个函数时，闭包的概念就变得更加重要了。返回的函数可以访问不在全局范围内的变量**，但是它们只存在于它的闭包**中。— [奥利维尔·德·默德](https://medium.com/u/555725a03e9c?source=post_page-----cdf42b48d70--------------------------------)在[日报上](https://medium.com/dailyjs/i-never-understood-javascript-closures-9663703368e8)

闭包就像一个背包。函数带有一个小背包，里面装着函数创建时作用域内的所有变量。

下面是一个快速的闭包实例:

当变量`multiplyByFive`在本地上下文中被声明时，它被赋予一个函数定义和一个闭包。

在这个例子中，变量`number`是闭包的一部分。

所以现在当调用并执行`multiplyByFive`时，它可以访问闭包中的变量`number`以及作为参数传递的变量`innerNumber`，因此能够返回产品。

![](img/983775cc4434b89e927d64c2210cfe3a.png)

Photo by [Artur Aldyrkhanov](https://unsplash.com/@aldyrkhanov?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 如果我忘记使用关键字怎么办？

在这种情况下，JavaScript 试图通过创建一个全局`var`来帮助您。

通常这被认为是一件坏事，因为现在变量可以被访问(和改变！)绝对是代码中的其他地方。

在这种情况下，提升的工作方式略有不同，因为没有变量声明要提升到代码的顶部。因此，直到函数被执行，变量声明才被定义。

与显式声明变量相比，即使是在代码末尾:

注意"`[use strict](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode)`"和 [ESLint](https://eslint.org/) 都会捕捉到这个错误，但是通常最好的做法是首先使用`let`或`const`而不是`var`。

![](img/e53bc86737fe4c346eb87e86622e1a5b.png)

Photo by [Sébastien Bourguet](https://unsplash.com/@cmdor?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 概括一下:

> **var 是函数作用域。**
> let 和 const 都是块作用域。
> 
> 函数范围在函数内。
> 块范围在花括号内。
> 
> — [乔·卡迪罗](https://medium.com/u/cb08a5227dd4?source=post_page-----cdf42b48d70--------------------------------) [在他的媒体博客](https://medium.com/@josephcardillo/the-difference-between-function-and-block-scope-in-javascript-4296b2322abe)

# 其他资源:

*   [泰勒·麦金尼斯](https://medium.com/u/c52389e3ee63?source=post_page-----cdf42b48d70--------------------------------)在`let`、`var`和`const`的视频上有一篇[博文:](https://tylermcginnis.com/var-let-const/)

[](https://tylermcginnis.com/var-let-const/) [## JavaScript 中的 var vs let vs const

### 在这篇文章中，你将学习两种在 JavaScript (ES6)中创建变量的新方法，let 和 const。一路上我们会寻找…

tylermcginnis.com](https://tylermcginnis.com/var-let-const/) 

*   [玛雅·沙文](https://medium.com/u/98cbd966a4c9?source=post_page-----cdf42b48d70--------------------------------)讨论[前端周刊](https://medium.com/front-end-weekly/es6-cool-stuffs-var-let-and-const-in-depth-24512e593268)中`let`、`const`、`var` 的执行上下文和提升意味着什么；

[](https://medium.com/front-end-weekly/es6-cool-stuffs-var-let-and-const-in-depth-24512e593268) [## ES6 酷的东西——在深度上变化、变化和保持不变

### 在这篇文章中，我们将继续探索一些 ES6 酷的东西——让，const 语句与我们的经典变量…

medium.com](https://medium.com/front-end-weekly/es6-cool-stuffs-var-let-and-const-in-depth-24512e593268) 

*   Eric Elliott 断然拒绝在他的代码中使用 T6。原因如下:

[](https://medium.com/javascript-scene/javascript-es6-var-let-or-const-ba58b8dcde75) [## JavaScript ES6+: var，let，还是 const？

### 也许要成为一名更好的程序员，你能学到的最重要的事情就是保持事情简单。在…的背景下

medium.com](https://medium.com/javascript-scene/javascript-es6-var-let-or-const-ba58b8dcde75) 

*   [Diana J. Yoo](https://medium.com/u/d049f0863e24?source=post_page-----cdf42b48d70--------------------------------) 简单明了地总结了`let`、`var`、`const`、:

[](https://medium.com/@dianajyoo/var-let-or-const-ad319d958842) [## Var，let，还是 const？

### 最近，我被要求在一个技术屏幕上解释 var、let 和 const 的区别，所以我想到了…

medium.com](https://medium.com/@dianajyoo/var-let-or-const-ad319d958842) 

德里克·奥斯汀博士是《职业编程:如何在 6 个月内成为一名成功的 6 位数程序员 》一书的作者，该书现已在亚马逊上架。