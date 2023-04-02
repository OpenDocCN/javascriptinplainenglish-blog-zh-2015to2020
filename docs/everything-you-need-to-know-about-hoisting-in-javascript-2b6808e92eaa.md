# 关于在 JavaScript 中提升你需要知道的一切

> 原文：<https://javascript.plainenglish.io/everything-you-need-to-know-about-hoisting-in-javascript-2b6808e92eaa?source=collection_archive---------23----------------------->

## 了解 JavaScript 中的变量提升、函数提升和类提升

![](img/3c7744623589cb1d6075db1a025996e9.png)

Photo by [Daniel Frank](https://unsplash.com/@fr3nks?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了充分了解 JavaScript 的工作原理，任何开发人员都应该知道其中的一个关键概念。对提升的良好理解肯定有助于减少代码中的运行时异常和意外行为。因此，让我们深入了解一下起重是如何工作的。

# 什么是吊装？

举个例子更容易理解什么是吊装。让我们看看下面的代码片段。

这里可以看到`printVariable`函数在定义之前就被调用了，传递给函数的变量也在定义之前就被使用了。但是，如果您运行代码，您会注意到程序没有抛出任何错误。它只是在控制台中打印出`undefined`。似乎程序在某种程度上意识到了代码中这些尚未定义的元素。这背后的魔力是提升。

提升是通过在编译时为代码中的变量声明、类声明和函数声明分配内存位置来实现的。因此，感觉上这些元素被提升到了它们作用域的顶端，这样程序就可以提前知道它们。

下面的代码片段显示了编译器读取上面代码中的元素的顺序，给人一种被举起来的感觉。重要的是要记住，提升与编译器如何通读程序有关，它并不真的改变代码。

# 可变提升

JavaScript 中的变量可以用`let`、`const`或`var`来声明，所有的变量声明(不是它们的赋值)都会被提升。然而，提升声明的初始化方式有所不同。对于用`var`声明的变量，被提升的变量声明用`undefined`初始化，直到变量后来被赋予它的实际值。因此，当我们试图在实际定义之前访问用`var`声明的变量时，它会返回`undefined`。

对于用`let`和`const`声明的变量，提升的声明不会用 undefined 或任何其他值初始化。因此，当这些变量在定义之前被访问时，程序抛出`ReferenceError`。

# 功能提升

## 命名函数

在 JavaScript 中，命名函数的函数声明被挂起。函数声明包括关键字`function`、分配给函数的名称、输入参数和函数体。它基本上提升了一个功能为了达到它的目的所需要的所有部分。因此，我们可以在实际定义命名函数之前调用它们，并期望它们按照预期的方式运行。

## 匿名函数/函数表达式

匿名函数可以是箭头函数，也可以是常规函数，通常会赋给变量。因为它们被赋给变量，不像命名函数，只有变量声明被提升。提升变量声明的行为类似于我们之前讨论的变量提升，它们是不可调用的。因此，我们不能在定义匿名函数之前使用它们。

注意，当我们试图调用未用任何东西初始化的被提升的`let`声明时，程序抛出了一个`ReferenceError`，当我们调用用`undefined`初始化的被提升的`var`声明时，程序抛出了一个`TypeError`。

# 分级提升

## 类声明

JavaScript 类声明在编译时被抛出，但是它们没有用任何值初始化。这些声明的行为类似于我们观察到的提升的`const`和`let`变量声明。即使 JavaScript 设法为我们创建的类找到了一个引用，我们也不能在代码中实际定义它之前使用它。

## 类别表达式

类表达式，类似于函数表达式，赋给变量；因此只有变量声明被提升。因此，我们不能在定义类表达式之前使用它们。

# 结论

提升是一个隐喻概念，与 JavaScript 编译器在编译时如何读取代码有关。为了理解 JavaScript 如何真正工作并避免意外行为，必须清楚地了解变量声明、函数声明和类声明是如何被提升的。

感谢阅读！

# 资源

[JavaScript 中的提升是什么？](https://medium.com/javascript-in-plain-english/https-medium-com-javascript-in-plain-english-what-is-hoisting-in-javascript-a63c1b2267a1)由[苏尼尔·桑德胡](https://medium.com/u/a7b125868703?source=post_page-----2b6808e92eaa--------------------------------)

[在 JavaScript 中提升](https://stackabuse.com/hoisting-in-javascript/)

[吊装](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting)

[用 JavaScript 理解吊装](https://www.digitalocean.com/community/tutorials/understanding-hoisting-in-javascript)

[WTF 是 JavaScript 变量提升](https://youtu.be/j-9_15QBW2s)