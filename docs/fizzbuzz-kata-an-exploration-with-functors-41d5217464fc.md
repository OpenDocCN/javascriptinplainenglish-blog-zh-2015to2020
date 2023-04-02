# FizzBuzz Kata:对函子的探索

> 原文：<https://javascript.plainenglish.io/fizzbuzz-kata-an-exploration-with-functors-41d5217464fc?source=collection_archive---------7----------------------->

## 今天，当我们重温著名的 FizzBuzz 形时，我们将看到如何利用函子进行函数合成。

![](img/ea7dcf3375e2abe6d699c0ad18f163f2.png)

Photo by [Corey Agopian](https://unsplash.com/@corey_lyfe?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

*本文是 TypeScript 中著名的 kata 上演示的函数式编程概念系列文章的一部分。没有理论，只有实践！*

通过加强不变性和支持函数组合，函数式编程是一种强大而简单的方法，可以减少程序员的认知负荷、简化可测试性和减少错误(无共享状态)。

当然，有些语言比其他语言更适合函数式编程，但是在 [Arolla](https://www.arolla.fr/) 我们相信，人们总是可以在他们的日常语言中使用这种编程风格的一些成分，无论是 Java、C#、JavaScript 还是 Python。因此这个系列的名字:**休闲 FP** 。

所以今天，当我们重温著名的 FizzBuzz 形时，我们将看到如何利用函子来对任何类型的对象进行函数合成。

# 嘶嘶作响

FizzBuzz 是手工艺社区中众所周知的一种形式，因为它非常适合学习[测试驱动开发](https://www.arolla.fr/training/tdd-test-driven-development/) (TDD)，因为它简单且渐进，同时允许不止一次的重构。

这里的目的不是学习 TDD，因此我们将直接跳到一个足够好的*实现。测试和源代码可以在 [GitHub](https://github.com/mathieueveillard/fizzbuzz-js) 上获得。*

在现实生活中，我会就此打住:它有效，简单，易读。

但是…我还不太满意。直觉告诉我们，实现依赖于一个**管道**:

*   用空字符串初始化结果
*   如果数字是 3 的倍数，则追加“Fizz”
*   如果数字是 5 的倍数，则附加“Buzz”
*   最后，如果该数字不是 3 或 5 的倍数，则将其作为字符串追加

不过，我在代码中看不到它:人们不能用简单的英语把它当成一个句子来读，可能是因为可变状态(`let result`)。

我希望我可以通过组合纯函数来构建一个管道，并用一个初始的空字符串来填充管道。也就是说，在伪代码中:

```
"".map(appendFizzIfNIsMultipleOf3)
  .map(appendBuzzIfNIsMultipleOf5)
  .map(returnNOtherwise)
```

不幸的是，`String`原型没有公开一个`map`方法。这就是函子发挥作用的地方。

> 不幸的是，String 原型没有公开 map 方法。这就是函子发挥作用的地方。

# 函子

关于函子的精彩介绍，你可能想参考 Eric Elliott 的[作曲软件](https://medium.com/javascript-scene/composing-software-the-book-f31c77fc3ddc)一书([函子&类别](https://medium.com/javascript-scene/functors-categories-61e031bac53f)章节)。但是我建议你在看完这个用例之后再看*。关键的一点是**仿函数是一个可映射类型**，这是一个公开`map`方法的类型。*

> **函子是一种可映射类型。**

所以基本上，我们将把空字符串包装在一个函子中，进行映射，然后解开函子，返回它携带的修改后的字符串。

由于单元测试帮助我们构建了第一个实现，我们可以安全地进行重构。所以事不宜迟，让我们直接开始吧！

哇…这么小的功能却有这么多代码！这可能是过度设计，但我们将继续下去，因为它只是为了演示的目的。

让我们看看这里会发生什么:

*   首先我们定义函子，通过它的构造函数`IdentityFunctor`。它封装了一个给定类型的`value`(例如一个字符串)，并且可以映射到一个`IdentityFunctor`，其类型由映射函数`fn`决定。然而在这里，我们将只从一个字符串映射到另一个字符串。
*   然后我们定义纯映射函数，`appendIfMultipleOf`和`returnNOtherwise`，它们封装了大部分业务逻辑(有引号，但是嘿，你明白我的意思)。
*   最后，当我们可以使用这些基础时:在 5 行代码中，现在非常清楚，我们通过在*特定条件*下连接*事物*来构建一个空字符串，并返回结果。这几乎是简单的英语。

# 大局

当然，这是一个人为的例子:对于这么少的附加值来说，代码太多了。但是希望您已经直观地了解了函子是什么以及何时使用它:函子允许映射不可映射的对象。

> 仿函数允许映射不可映射的对象。

`IdentityFunctor`可能是人们可以想象的最简单的函子。还有其他类型的函子，从数组开始，以及类似函子的类型(如承诺)。

此外，函子主要用于处理管道中条件类型的映射，这对于不破坏管道非常方便。同样，[埃里克·埃利奥特](https://medium.com/javascript-scene/functors-categories-61e031bac53f)做了很好的详细解释。

欢迎评论。感谢您的阅读！

# 资源

*   [https://github.com/mathieueveillard/fizzbuzz](https://github.com/mathieueveillard/fizzbuzz)
*   [作曲软件:书](https://medium.com/javascript-scene/composing-software-the-book-f31c77fc3ddc)，作者埃里克·艾略特
*   [Kyle Simpson 编写的功能轻量级 JavaScript](https://github.com/getify/Functional-Light-JS)
*   [功能编程的基本指南](https://github.com/MostlyAdequate/mostly-adequate-guide)

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)和 [**找到它们订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**