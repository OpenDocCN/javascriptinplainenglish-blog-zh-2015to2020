# 下一个 TypeScript 代码的五个专业技巧

> 原文：<https://javascript.plainenglish.io/top-5-pro-tips-for-your-next-typescript-code-33ae2c6f1e5e?source=collection_archive---------3----------------------->

![](img/f13d60cbbc6d5e5ede94e9f378b0147d.png)

Image credits: freecodecamp.org

## 到目前为止，我已经写了大约 2 年的 TypeScript 代码。我想分享一些我在代码中使用的特殊东西。

## JavaScript 与 TypeScript

几十年前，JavaScript 通过在网页上做简单的动态事情开始了它的旅程。但是现在 JavaScript 有了非常有用的特性，比如类支持和强大的标准内置对象，就像任何其他现代编程语言一样。JavaScript 是一种动态类型的编程语言，因为整个概念是通过解释的方式在用户的沙箱环境中执行源脚本。

TypeScript 非常受欢迎，因为它引入了一种新的方式来帮助开发人员使用静态类型概念编写 JavaScript。因此，在执行代码之前很容易识别数据类型。

让我们跳到技巧和窍门部分！

## 相交与结合

在某些情况下，我们需要用一个函数的相同参数传递不同类型的变量，并且需要组合多个变量类型的属性。

**联合**:当特定函数参数支持多种变量类型时有帮助。

**交集**:将两个变量类型合并为一个类型，该类型将具有给定变量类型的所有属性。

## 魔法或

如果有几个变量，并且需要将 [truthy](https://developer.mozilla.org/en-US/docs/Glossary/Truthy) 值设置为另一个变量，它可以通过几个 If 条件简单地求解。但是使用`||`来代替更清晰和明智的代码怎么样？

## 抽象属性

我们都熟悉抽象类，因为我们经常使用这个概念来使用面向对象的设计原则来实现良好的可维护代码。TypeScript 也支持抽象属性，因此我们能够在非具体类中定义一些抽象属性，并通过在具体类中赋值来使用它。

## 外部库定义

某些类型脚本项目可能使用非类型脚本全局库。我们的老朋友 Jquery 就是一个很好的例子。如果特定的外部库有 TypeScript 定义，就不会有 transpilation 的问题。然而，如果特定的外部库是定制的，或者不是公开的，你可以使用以下技巧来避免 transpilation 错误。

**偷懒道**

只是声明整个外部库为*任何*。例如，将 Jquery 的`$`声明为`declare let $: any`

**主动方式**

更正确的方法是为外部库创建自己的 TypeScript 定义(如果它不是公开可用的)。只需使用接口或名称空间创建带有所需定义的`index.d.ts`。一个简单的例子是 [Neutralinojs](https://neutralino.js.org/) 的类型脚本定义可以在[这里](https://github.com/neutralinojs/neutralinojs-typescript/blob/master/src/index.d.ts)找到

## 你简单的如果条件可能是错误的

开发人员大多使用`if(variable)`和`if(!variable)`来检查某个特定变量是否可用。重要的是`0`在 Javascript 中不是真值。因此，我们需要小心检查真值。

在上面的例子中，`if`条件表现不正确，因为`itemIndex`也可能是`0`。因此，如果我们将条件修改到像`if(item.itemIndex != null)`这样的点上，一切都会像我们预期的那样正确运行。

大家编码快乐！😎

## 简单英语的 JavaScript

你知道我们有三份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)找到一切的链接！