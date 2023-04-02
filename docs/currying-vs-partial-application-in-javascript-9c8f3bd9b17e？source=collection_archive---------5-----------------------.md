# JavaScript 中的 curry vs 部分应用

> 原文：<https://javascript.plainenglish.io/currying-vs-partial-application-in-javascript-9c8f3bd9b17e?source=collection_archive---------5----------------------->

![](img/ad391c5a95dbe0b41c3858b620551e55.png)

Photo by [Shubham Sharan](https://unsplash.com/@shubhamsharan?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# Currying

**Currying** 与印度菜没有联系——它来源于数学家哈斯克尔·库里(Haskell Curry)的名字(哈斯克尔编程语言也是以他的名字命名的)。Currying 是一个转换过程——我们转换一个函数。奉承的另一个名字是**schnfinkelization**，以另一位数学家摩西·施芬克尔的名字命名。

## 那么什么是 currying？

> **Currying** — 它是将一个有多个参数的函数转换成一组有一个参数的嵌套函数。

## currying 是如何工作的？

当调用一个传递了一个参数的 curried 函数时，它将返回一个新函数，该函数将期待下一个参数的到来。然后调用传递了第二个参数的第二个函数，并等待下一个参数。依此类推，直到函数收到它需要的所有参数。现在，函数获得了执行计算所需的一切—此时它返回计算结果。

让我们看看这个简单的例子中“currying”是如何工作的。

假设我们有一个函数，它总结了两个论点:

但是如果我们有 5 个参数呢？

不太漂亮，你同意吗？

我们来咖喱吧！

很好！看一下变量 **curry1** 。但是如果我们想让**去掉空括号**呢？让我们一起来做吧:

**注意看“f.toString”！**

## 与图书馆打交道

我们可以自己编写函数或者从库中使用它，例如从 **lodash 的 _ 中。curry** (func，[arity=func.length]) —我将把它的链接留在 Links 块中。

## 当我们使用 currying 时。

*   当我们多次调用同一个函数时，第一个参数保持不变。
*   在部分应用的情况下(在“部分应用”块中阅读)。

# 部分应用

> **部分应用** —指将多个自变量固定到一个函数上，产生另一个更小 arity 的函数的过程。

正如我们前面注意到的，对于部分应用程序，我们需要首先处理该函数。

让我们用函数**sumcuried 2**做一个局部应用。假设我们只知道函数的第一个自变量，并想调用它。

我们不能用以前的代码这样做:

但是我们可以这样做:

注意变量库里的 —它存储了我们的值‘0’，我们将使用它。

好，假设我们现在知道了第二个论点:

在这里我们用新的论证称之为库里‘1’。因此，我们从上一步获得了“0”，并在当前步骤中添加了“1”。

0 + 1 => 1

我希望它对你来说变得更加透明。

现在，另一个论点是:

最终结果保持不变 10。

# 链接

1.  斯托扬·斯特凡诺夫[http://shop.oreilly.com/product/9780596806767.do](http://shop.oreilly.com/product/9780596806767.do)的奥莱利《JavaScript 模式》一书
2.  Lodash，_。咖喱()[https://lodash.com/docs/4.17.15#curry](https://lodash.com/docs/4.17.15#curry)