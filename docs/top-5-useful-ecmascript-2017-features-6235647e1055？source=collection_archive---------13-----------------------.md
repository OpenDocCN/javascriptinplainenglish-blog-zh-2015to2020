# ECMAScript 2017 的 5 大有用功能

> 原文：<https://javascript.plainenglish.io/top-5-useful-ecmascript-2017-features-6235647e1055?source=collection_archive---------13----------------------->

## JavaScript ECMAScript 2017 的 5 个特性及实际例子

![](img/a7334b7e97cb976df1ba0f7f67c3b9a8.png)

Photo by [AltumCode](https://unsplash.com/@altumcode?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

JavaScript 是一种强大而流行的编程语言，尤其是在 web 开发中。它作为网页的脚本语言最为人所知。JavaScript 由 Brendan Eich 于 1995 年发明，并于 1997 年成为 ECMA 标准。ECMAScript 是该语言的官方名称。除此之外，这种语言的每一个新版本都有令人惊叹的特性，使得开发人员的开发过程更加容易。

在本文中，我们将探索 JavaScript ECMAScript 2017 的一些新功能。拿着你的咖啡，我们开始吧。

![](img/532274e388f88cf314b6502507ca49a9.png)

Photo by [Ilyuza Mingazova](https://unsplash.com/@ilyuza?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 1.JavaScript 字符串填充

ECMAScript 2017 增加了两个字符串方法:`**padStart**` 和`**padEnd**` ，允许你在一个字符串的开头和结尾添加填充。你可以看看下面的例子来理解这些方法:

The **padStart** Method.

The **pad End** Method.

如果你愿意，你可以在你的浏览器控制台上试试。

# 2.JavaScript 对象值

JavaScript 2017 还引入了返回对象值的一维数组的`**Object.values**` 。看看下面的例子:

**Object.values** in JavaScript.

# 3.JavaScript 对象条目

`**Object.entries**`类似于`**Object.values**`，但是返回对象值和属性(键)的多维数组。看看下面的例子:

**Object.entries** in JavaScript.

# 4.JavaScript 异步函数

异步函数在 JavaScript 中异步执行。这意味着我们的代码可以同时执行多个任务，而不必等待任务转移到另一个任务。异步函数是用`**async**` 关键字声明的，并且在它们内部允许使用`**await**`关键字。让我们看看下面的例子:

**Async** Function.

正如你所看到的，承诺将出现在字符串之后，即使我们在函数中在字符串之前调用它。这是因为承诺需要 3000 毫秒来执行。异步代码不会等待它，这就是字符串先于承诺出现的原因。

# 5.object . getownpropertydescriptors()

这个有点奇怪。`**Object.getOwnPropertyDescriptors()**` 返回包含描述符的对象的每个属性。描述符基本上是添加到属性中的元信息，它定义了如何使用该属性。让我们看看下面的例子:

**Object.getOwnPropertyDescriptors( )**.

正如您所看到的，这个函数为您提供了一种以非常简单的方式获取对象的所有描述符信息的方法。

# 结论

这些简单的特性对您的 JavaScript 开发非常有用。探索最新的工具、特性和技术将使你成为更好的开发人员。技术变化很快，我们需要不断学习，因为它从未停止，尤其是在科技行业。感谢您阅读本文，希望您觉得有用。