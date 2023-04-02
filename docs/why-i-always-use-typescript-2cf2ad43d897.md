# 为什么我总是使用打字稿

> 原文：<https://javascript.plainenglish.io/why-i-always-use-typescript-2cf2ad43d897?source=collection_archive---------2----------------------->

## 为什么应该总是使用 TypeScript 而不是 JavaScript 的核心原因

![](img/52f2bdf48e036fbd92f2951a58df8369.png)

Photo by [John Barkiple](https://unsplash.com/@barkiple?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

作为开发人员，我们努力尽可能地写出最好的代码，同时“好代码”这个术语并不是完全客观的，只是在某一点上，在这之后，它确实变得非常主观，带有个人偏好。

有相当多的原则，当它们被遵循而不被滥用时，会产生好的代码，其中一些是:

*   ***KISS —“保持简单，笨蛋”***不要过分聪明，用更少的行做一些事情并不会让它变得更好，相反，它可能会伤害到下一个试图弄清楚是怎么回事以及如何添加一个新的简单功能的人。
*   ***干—“不要重复自己”*** 把其他地方会需要的代码模块化，复制粘贴不是办法，是偷懒的黑客行为。
*   ***【YAGNI】—“你不会需要它”*** 试图预测什么在未来会有用是一场艰苦的战斗，这将导致代码复杂性的增加和时间的浪费，目前尽可能保持简单。

太好了，现在我们有了可以被认为是好的代码，但是我们还能做什么呢？如果使用 Javascript，肯定有，它被称为…

# 以打字打的文件

它是 javascript 的超集，这意味着它可以编译成普通的 Javascript，所以它可以在普通 Javascript 可以使用的所有地方使用，只需要一个额外的编译步骤。

Typescript 的使用带来了许多好的特性，我不打算把它们都列出来，因为它们中的两个足以让我在做任何与 Javascript 相关的事情时把它作为默认语言来使用，它们是…

# 动态类型检查和接口

我想我们都同意，好的代码必须易于阅读，这是普通 Javascript 的弱点，因为您不能声明类型，这使您在运行应用程序时更难捕捉琐碎的错误，而在 Typescript 中，我们可以声明它们:

```
let num: number = 5;
let msg: String = `I have ${num} apples.`;
```

对于这样一个简单的例子来说，这似乎有些过分了，但是如果您希望更加明确的话，这是一个选项。我们也可以省略这些类型，但是编译器仍然会根据上下文知道它们，如果我们试图做一些可疑的事情，它会警告我们。

```
let num = 5;
let msg = "m";num * msg // NaN in plain JS
num * msg // Compile time error in TS
```

虽然这很好，但当我们开始使用接口时，它会变得更好。在其他语言中，接口就像一个类的契约，它必须有特定的方法。虽然在 Typescript 中它们也可以达到这个目的，但是当与 Javascripts 的最佳特性之一——对象文字一起使用时，它们才真正开始发光。

想象一下，如果我们有一个 API 调用，它返回一个猫的列表。我们可以创建一个名为`Cat`的接口来描述这个对象，我们声明我们的 API 方法返回一个它们的列表。

```
interface Cat {
    name: String;
}let getCats = (): Cat[] => {
    // api call to get the cats
    // some transformations 
    // perhaps another api call return [
        { name: "Prince" },
        { name: "Patch" }
    ];
}// somewhere deep in our application
let cats: Cats[] = getCats();
```

现在，在我们的代码中，当我们看到方法调用时，很明显返回的是什么类型的对象，不需要查看方法内部或检查 API 文档，所有信息都在代码本身中，只需很少的额外工作，尽管收益将随着项目的复杂性呈指数增长。

# 结论

对我来说，转换到 Typescript 花费了很少的时间，可能是因为我以前使用过的语言，但我确实相信 Typescript 值得所有的宣传，因为对我来说，它使代码更清晰，更容易管理，没有真正的缺点，可能是对它提供的所有功能有一个轻微的学习曲线。