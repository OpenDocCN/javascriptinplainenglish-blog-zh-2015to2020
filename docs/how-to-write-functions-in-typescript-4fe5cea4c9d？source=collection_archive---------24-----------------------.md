# 如何在 TypeScript 中编写函数

> 原文：<https://javascript.plainenglish.io/how-to-write-functions-in-typescript-4fe5cea4c9d?source=collection_archive---------24----------------------->

## 初学者友好系列，让您的应用程序经得起未来考验

## 嘿，你好

为了方便起见，本系列的其他部分如下。🥳

[面向未来的应用第 1 部分](https://blog.usejournal.com/future-proof-your-react-app-pt-1-10213b1d0022)

[让你的应用适应未来的第二部分](https://medium.com/javascript-in-plain-english/future-proof-your-react-app-55fc6ac2cb10)

我们之前讨论了 prop-type，并继续介绍 TypeScript。尽管如此，我们的 TypeScript 介绍还是非常简短，主要集中在原始数据类型的类型化系统上。这篇文章将继续展示一个函数如何在执行函数体之前先检查参数类型。在探索了 TypeScript 如何处理函数之后，我们还将看看一些更复杂的类型。

![](img/ac7624c74c34de4d604eabdb20956b1f.png)

Photo by [James Harrison](https://unsplash.com/@jstrippa?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## JavaScript 缺陷

首先，让我们来看看处理 JavaScript 中的类型错误或意外行为所需的额外代码量，这只是在您实际上有经验和远见来预料这些事情的时候。JavaScript 代码实际上不能期望传入的参数类型。JavaScript 函数可以接受任何数据类型，但是我们在函数体中指定处理意外数据类型的逻辑。

```
**JS code:**
const findSum = (numOne, numTwo) => numOne + numTwofindSum(5, '6') -> outputs 56 *** Notice the '6' is a string in this case and the integer 5 gets coerced into a string as well, resulting in a concatenated string value. Definitely not what we want in this case - and sure, we can write logic inside the function body like below.**const findSum = (numOne, numTwo) => {
  if(typeof numOne !== 'number' || typeof numTwo !== 'number'){
    throw new Error('passed in args are not integers')
  }
  return numOne + numTwo
}
```

第二段 JavaScript 代码肯定是有效的，而且它工作正常。然而，在 TypeScript 中有一种更简单的方法来编写它。我们写的代码越少，通常意味着我们需要阅读和维护的代码就越少。每个人都“喜欢”那个！

```
**TS code:** const findSum = (numOne:number, numTwo:number) => numOne + numTwofindSum(5, '6')*** This will result in an error that looks like the line below.****Error** - Argument of type 'string' is not assignable to parameter of type 'number'.
```

我们可以在声明变量时利用相同语法的类型注释— **<变量/参数:期望的数据类型>** 。TypeScript 中的类型系统将处理意外数据类型的逻辑，并在不编写额外代码的情况下引发错误。请注意，如果参数旁边没有提供类型，将会推断出“any”类型。

## 功能，参数，自变量…太空堡垒卡拉狄加？

还记得 JavaScript 如何喜欢在大多数时候无声地失败，并且每当某个地方有一个小 bug 时就抛出 undefined 吗？有时这是由于缺少参数，它不会以与 TypeScript 相同的方式运行。让我们看看下面代码中的 TypeScript 错误是什么样子的。

```
**JS code:** const findSum = function (numOne, numTwo) => {
  console.log(numOne, numTwo) ------> **// 6, undefined**
  return numOne + numTwo
}console.log(findSum(6)) ------------> **// NaN****TS code:** const findSum = (numOne:number, numTwo:number) =>  numOne + numTwofindSum(5)**Error** - Expected 2 arguments, but got 1.
      - An argument for 'numTwo' was not provided.
```

这些错误是有益的，除非在 TypeScript 中用可选参数显式指定，否则灵活性肯定会降低。

```
**JS code:** const greeting = function (name) => {
  return "hello " + (name || 'stranger')
}console.log(greeting()) **// hello stranger****TS code:** const greeting = (name?:string) => {  **<-- notice the question mark?**
  return `hello ${name || 'stranger'}`
}The question mark after the parameter allows developers to make a given parameter optional.
```

为参数分配默认值看起来与 JavaScript 相同。

```
**TS code:** const greeting = (name = 'stranger') => {
  return `hello ${name}`
}console.log(greeting()) **// outputs 'hello stranger'**
```

TypeScript 将根据默认值的类型在这里执行类型推断。因此，在这种情况下，默认值是“string”类型——接收任何其他数据类型都会导致错误。既然我们讨论的是类型推断的话题，还值得一提的是，TypeScript 通过查看 return 语句后的值来推断返回类型。在上面的代码中，问候函数的返回类型将是一个“字符串”如果出于某种原因，您必须将返回值存储到另一个指定数据类型的变量中，这将导致抛出错误。现在让我们看看显式声明返回类型。

```
**TS code:** const myAge = (age = '18')**: string** => { 
  return Number(18)  ** 
  // returning any other data type will cause an error**
}**// explicit annotation after closing parenthesis to state return type****// Type 'number' is not assignable to type 'string'.**
```

如果一个函数因为缺少参数或者忘记返回值而返回 **undefined** ，TypeScript 会产生一个有用的错误，因为 **undefined** 也是一个数据类型。超级整洁！

## 结论

感谢您加入我学习 TypeScript 的旅程。我个人认为 TypeScript 有助于保护我们的代码。这无疑有助于提高可读性，并消除了在大型工程环境中工作时对函数应该接收和返回什么的猜测。

希望我能够清楚地表达我的想法，我们都从这篇文章中学到了很多。现在，我们知道了如何读写参数的类型注释，像在 JavaScript 中一样给出默认值，以及如何确定或显式声明函数的返回类型。这可能已经成功了一半以上，因为函数被视为一等公民，可以传递给其他函数。

关于 TypeScript 的下一篇文章将介绍如何将类型应用于更复杂的数据结构，如数组和对象。永远学习，对吗？！下次再见了。

## 参考

[](https://www.tutorialsteacher.com/typescript/typescript-variable) [## TypeScript 变量声明:var，let，const

### 了解使用 var、let 和 const 关键字在 TypeScript 中声明变量的不同方式。

www.tutorialsteacher.com](https://www.tutorialsteacher.com/typescript/typescript-variable) [](https://ts.chibicode.com/todo/) [## 面向知道如何构建 Todo 应用程序的 JS 程序员的 TypeScript 教程

### 2011 年，Backbone.js 是最受欢迎的 JavaScript 库之一(React 于 2013 年问世；2014 年的 Vue)。当…

ts.chibicode.com](https://ts.chibicode.com/todo/) [](https://www.typescriptlang.org/) [## 任意比例的输入 JavaScript。

### TypeScript 通过向语言中添加类型来扩展 JavaScript。TypeScript 通过以下方式加速您的开发体验…

www.typescriptlang.org](https://www.typescriptlang.org/) [](https://2ality.com/2018/04/type-notation-typescript.html) [## 理解 TypeScript 的类型标记法

### 这篇博客文章是对静态类型的 TypeScript 符号的快速介绍。目录:看完这个…

2ality.com](https://2ality.com/2018/04/type-notation-typescript.html)