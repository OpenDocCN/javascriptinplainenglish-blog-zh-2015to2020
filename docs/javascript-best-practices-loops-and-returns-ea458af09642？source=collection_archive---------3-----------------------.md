# JavaScript 最佳实践—循环和返回

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-loops-and-returns-ea458af09642?source=collection_archive---------3----------------------->

![](img/a5b2099fc2279ff2e34eb92699247ab3.png)

Photo by [Blake Wheeler](https://unsplash.com/@blakesox?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 坦白地说

你知道我们有四种出版物吗？通过[**plain English . io**](https://plainenglish.io/)—关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**

JavaScript 是一种非常宽容的语言。编写可以运行但有错误的代码很容易。

在本文中，我们将研究编写循环和函数(如循环长度、嵌套和返回)的最佳实践。

# 让我们的循环足够短，以便一次查看所有内容

我们的循环应该很短，这样就可以在页面上看到整个事情。

不然太长了，要分手。

# 将嵌套限制在 3 层

嵌套通常会使我们的代码更难阅读。因此，我们可以将它限制在最多 3 级。

除此之外，它太多了，我们应该通过将一些代码放入函数中来消除它们。

# 将长循环的循环内部移到函数中

如果我们有一个长循环，这意味着我们必须把代码分成更小的函数。

小函数和循环更容易阅读，所以我们应该编写它们，而不是把所有东西都放在一个大循环中。

# 让长循环特别清晰

如果我们不能避免长循环，那么我们应该尽可能地使它们清晰。

否则，我们将很难读懂自己的代码。

# 循环和数组之间的对应关系

循环和数组通常是相关的。许多循环用于操作数组。

循环计数器变量因此与数组索引一一对应。

然而，使用 JavaScript，我们并不总是需要编写循环来进行数组操作。

例如，`Array`构造函数中有许多方法可以用来创建循环。

我们可以使用`map`通过向数组中传递一个回调来将每个数组条目映射到新值。

例如，我们可以用它来计算数组中所有数字的平方，如下所示:

```
const arr = [1, 2, 3];
const result = arr.map(a => a ** 2);
```

还有一个`filter`方法，用于返回一个数组，该数组包含满足某些给定条件的条目。

我们可以找到数组中的所有偶数，如下所示:

```
const arr = [1, 2, 3];
const result = arr.filter(a => a % 2 === 0);
```

同样，为了搜索一个数组项，我们可以使用如下的`find`方法:

```
const arr = [1, 2, 3];
const result = arr.find(a => a % 2 === 0);
```

上面的代码将找到数组中的第一个偶数。

除了写出循环，我们还可以使用更多的方法。

在使用循环之前，我们可能需要考虑这些因素。

# 一个函数的多次返回

在 JavaScript 中，我们不必在函数结束时退出函数。

我们可以使用`return`关键字来停止该功能。

例如，我们可以这样写:

```
const fn = () => {
  if (invalid) {
    return;
  }
  //...
  return {
    //...
  }
}
```

在上面的例子中，当`invalid`设置为`true`时，我们可以停止该功能。

这让我们通过减少嵌套来清理代码。

# 当回车增强可读性时使用它

我们可以添加`return`语句来增强可读性。

正如我们所看到的，当满足某些条件时，停止一个函数是很有用的。

![](img/7761323ba56a3bdd548edfc14237b988.png)

Photo by [Jean Colet](https://unsplash.com/@apchf?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 使用 Guard 子句简化复杂的错误处理

从上面的例子可以看出，我们可以提前返回。

让我们提前停止运行函数的一组代码称为 guard 子句。

例如，在上面的函数中:

```
const fn = () => {
  if (invalid) {
    return;
  }
  //...
  return {
    //...
  }
}
```

以下是警卫条款:

```
if (invalid) {
  return;
}
```

这比写类似这样的东西要好得多:

```
const fn = () => {
  if (!invalid) {
    //...
    return {
      //...
    }
  }
}
```

做同样的事情，但是有更多的巢。

嵌套很难理解，而 guard 子句让我们避免嵌套。

# 最小化每个函数中的返回次数

保护条款很棒，但是我们不应该太频繁地使用它们。

代码中的 return 语句越多，就越难理解。

# 结论

嵌套在循环或函数中是不好的，所以我们应该减少它们。

在函数中，我们可以用 return 语句减少嵌套。在某些情况下，我们可以在最后一行之前返回一个函数。

除了循环，我们还可以使用数组方法来进行数组操作。