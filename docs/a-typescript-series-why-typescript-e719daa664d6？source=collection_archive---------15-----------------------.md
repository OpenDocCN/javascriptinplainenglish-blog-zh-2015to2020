# 现在是 2020 年，你应该用打字稿

> 原文：<https://javascript.plainenglish.io/a-typescript-series-why-typescript-e719daa664d6?source=collection_archive---------15----------------------->

![](img/f524d328cc2d937ceb6a14e535e99957.png)

Photo by [Rich Tervet](https://unsplash.com/@richtervet?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

也许你已经使用 JavaScript 有一段时间了，你已经听到了周围的嗡嗡声… *TypeScript* 。在这个 2 部分系列中，我们将看看 ***为什么要打字稿？*的**和**的*为什么不打字稿？***

# 什么是 TypeScript？

据 [TypeScript 网站](https://www.typescriptlang.org/)*“TypeScript 通过添加类型来扩展 JavaScript……TypeScript 代码通过 TypeScript 编译器或者*[*Babel*](https://babeljs.io/)*转化为 JavaScript 代码。这个 JavaScript 是干净、简单的代码，可以在 JavaScript 运行的任何地方运行……”*。简而言之，许多人将 TypeScript 称为 JavaScript 的*超集*。

在我看来，这有点像真的，也有点像假的。从技术上讲，不是所有*有效的 JavaScript 代码也可以是类型化代码。我们稍后会谈到这一点。但是，从本质上讲，TypeScript 旨在允许开发人员将 JavaScript 这种动态编程语言(一种在运行时解释的语言)转变为静态类型编程语言(一种在运行前编译的语言)。让我们开始吧。*

# *打字稿的原因*

## *提高代码质量(潜在的)*

*TypeScript 利用了类似于 C#或 Java 等语言的特性:它真正是面向对象的。这允许更健壮的代码。它使开发人员能够实现类型检查并利用接口、类、继承等。当查看 JavaScript 程序和 TypeScript 程序时，这可能会使游戏升级到干净的代码。*

**为什么把* ***这个词潜在地扔进去？*** 嗯，大多数有效的 JavaScript 代码都可以变成 TypeScript 代码。这意味着您不必使用 TypeScript 的面向对象或类型化功能。记住，TypeScript 是 ***可选*** 打出来的。这是*而不是*所要求的。如果 TypeScript 仅仅像 JavaScript 一样被使用，那么改进代码质量实际上可能不是一件事。*

## *更容易捕捉错误(理论上)*

*许多团队会切换到 TypeScript，因为它是可选类型的。让我们看看下面的代码。*

```
*function displayAge(age: number) { console.log("You are " + age + " years old.");}displayAge(25); // No Error. 25 is of type number and displayAge is expecting a numberdisplayAge("25"); // Error: Argument of type 'string' is not assignable to parameter of type 'number'*
```

*TypeScript 的优点之一是您可以定义类型。 *displayAge* 希望参数 Age 的类型为 number。第一次调用 *displayAge* 编译正确，因为 25 是 number 类型。对 *displayAge* 的第二次调用给出了一个编译错误，因为它是字符串类型。在 JavaScript 中，在运行代码之前不会捕捉到这个错误。其实可能根本就没抓到。虽然这是一个相当简单的函数，但是能够在编译时而不是运行时捕获 bug 是非常方便的。*

**为什么理论上扔这个词？*与上一节类似，TypeScript 是 ***可选*** 类型化。该功能在技术上可以定义如下。*

```
*function displayAge(age) { console.log("You are " + age + " years old.");}*
```

**显示年龄*现在假设年龄的数据类型为 *any* 。所以，不会有误差。总是定义你的类型是一个好习惯。这是开发 TypeScript 的主要目的之一。*

## *更易于扩展*

*JavaScript 非常适合快速开发。但是，随着程序变得更大、更复杂，JavaScript 可能会使事情变得非常混乱。TypeScript 可以更容易地扩展，这仅仅是因为类型。它允许更干净的代码和更快的错误/bug 捕获。这让开发人员了解问题并快速解决它们，而不是想知道为什么 JavaScript 在运行时会说类似于 *'TypeError: 'undefined '不是对象"**的错误。**

## *富裕社区*

*TypeScript 由微软开发，是开源的。事实上，你可以在这里看到代码。微软在 2012 年发布了 TypeScript，它的受欢迎程度持续增长。越来越多的大型科技巨头和大公司正在使用 TypeScript。这有助于相对年轻的语言开始发展成熟，我只能想象这将随着时间的推移继续下去。*

# *结论*

*切换到 TypeScript 有许多很好的理由。尤其是如果你的团队使用 JavaScript。虽然 TypeScript 可能很棒，并且有一些非常酷的特性，但是有一些理由让*不要*像使用任何语言或框架一样使用 TypeScript。检查回第二部分的 ***一个打字稿系列*** 我们会看到 ***为什么不打字稿？****