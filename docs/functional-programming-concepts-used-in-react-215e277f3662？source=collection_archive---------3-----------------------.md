# React 中使用的函数式编程概念

> 原文：<https://javascript.plainenglish.io/functional-programming-concepts-used-in-react-215e277f3662?source=collection_archive---------3----------------------->

![](img/5a029b8b38ded262429e734bf3d35dd2.png)

Photo by [Erika Varju](https://unsplash.com/@verika?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解如何在 React 应用程序中使用函数式编程特性。

# 一等品

JavaScript 包含的一个很大的函数式编程特性是，函数和其他任何东西一样都是对象。

这样，我们可以将它们设置为属性值，为它们分配变量，并将它们作为参数传入。

例如，如果我们有:

```
const add = (a, b) => a + b;
```

然后我们可以通过编写来创建一个函数:

```
const log = func => (...args) => {
  console.log(...args);
  return func(...args);
}
```

那么我们可以使用`log`如下:

```
const add = (a, b) => a + b;
const logAdd = log(add);
logAdd(1, 2);
```

我们可以使用`logAdd`来创建它。

`log`是一个高阶函数，因为它将一个函数作为自变量。

它还返回一个可以用自己的参数调用的函数。

我们将`add`函数传递给`log`来返回一个函数。

然后我们可以通过向返回的函数传递参数来调用它。

# 纯函数

纯函数是不产生副作用，只返回某些东西的函数。

这意味着它们不会改变函数之外的任何东西。

不纯的函数更难调试，因为它们改变了函数之外的东西。

例如，如果我们有:

```
const add = (a, b) => a + b
```

那么这就是一个纯函数，因为它只返回某些东西，不做任何其他事情。

另一方面，以下不是一个纯函数:

```
let sum;
const add = (a, b) => {
  sum = a + b;
  return sum;
}
```

因为它改变了函数之外的`sum`的值。

# 不变

不可变数据是另一个函数式编程特性。

这很好，因为它减少了数据意外变化的机会。

既然不能不小心更改数据，就不用担心有意外了。

有些事情会改变数据。例如，数组的`push`方法改变了现有的数组。

例如，我们可以写:

```
arr.push(3)
```

然后我们加 3 作为`arr`的最后一个值。

另一方面，如果我们使用 spread 操作符，那么我们不会改变现有的数组:

```
const newArr = [...arr, 3];
```

我们给`arr`和`newArr`分配了一个新的数组。

# Currying

Currying 是将接受多个参数的函数转换为一次接受一个参数的函数的过程。

例如，如果我们有以下内容:

```
const add = (a, b) => a + b
```

我们写道:

```
const add = a => b => a + b
```

对于第二个`add` 函数，我们通过编写来调用它:

```
const sum = add(1)(2);
```

多次重用由`add`返回的函数是一种方便的方法。

所以我们可以写:

```
const addOne = add(1);
const sum1 = addOne(2);
const sum2 = addOne(3);
```

# 作文

组合是将多种功能组合起来做更高级或更复杂的事情的概念。

例如，我们可以有:

```
const add = (a, b) => a + b
const cube = x => x ** 3
```

然后可以通过书写将它们结合起来:

```
const cubed = cube(add(1, 3));
```

我们调用`add`，然后对返回的结果调用`cube`。

![](img/1ca070e42bf88ef1991e57772a28ce38.png)

Photo by [Free To Use Sounds](https://unsplash.com/@freetousesoundscom?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 函数式编程和反应

我们使用函数式编程来对嵌套组件做出反应，也用于创建更高阶的组件。

高阶组件在向它们传递一个组件后返回一个新组件。

还有。钩子可以组成。

例如，我们可以将一个函数传递给`useState`钩子中返回的函数，用新值更新现有值。

# 结论

函数式编程在 React 项目中很重要。

我们将经常使用它来创建嵌套组件、高阶函数、钩子等等。

## 简单英语的 JavaScript

你知道我们有四种出版物吗？通过 [**plainenglish.io**](https://plainenglish.io/) 找到他们——通过关注我们的出版物和 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**