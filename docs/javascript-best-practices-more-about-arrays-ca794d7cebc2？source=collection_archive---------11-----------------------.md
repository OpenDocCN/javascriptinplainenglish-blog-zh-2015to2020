# JavaScript 最佳实践—关于数组的更多信息

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-more-about-arrays-ca794d7cebc2?source=collection_archive---------11----------------------->

![](img/016113eb6998d0e2a30322bd96e90019.png)

Photo by [Ondrej Machart](https://unsplash.com/@ondrejmachart?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。编写可以运行但有错误的代码很容易。

在本文中，我们将探讨用 JavaScript 操作数组条目的最佳方式。

# 使用`Array.from`代替 Spread 运算符来映射 Iterables

`Array.from`方法让我们将一个 iterable 对象转换成一个数组。此外，它还允许我们映射 iterable 对象的值，而无需创建中间数组。

因此，我们应该用它来代替 spread 操作符和`map`一起将一个 iterable 对象映射到一个新的数组。

例如，我们可以如下使用它:

```
const obj = {
  0: 1,
  1: 2,
  length: 2
}
const arr = Array.from(obj, (a) => a * 2);
```

在上面的代码中，我们有一个对象`obj`，它有整数键的条目和`length`属性，所以我们可以用`Array.from`方法将它转换成一个数组。

我们可以通过传入一个回调来映射条目，将条目映射到我们希望作为第二个参数的内容中。

这样，我们就不必使用 spread 操作符将一个 iterable 对象转换成一个数组，然后像下面这样对它调用`map`:

```
function* foo() {
  yield 1;
  yield 2;
}
const arr = [...foo()].map((a) => a * 2);
```

在上面的代码中，我们定义了一个生成器函数，然后调用它将返回的条目转换成一个数组。然后我们调用`map`将它映射到我们想要的条目中。

这并不好，因为我们必须创建一个数组，然后在上面调用`map`。

# 在数组方法回调中使用 Return 语句，单行箭头函数除外

我们应该总是在我们的数组方法回调中返回一些东西，除非我们为`forEach`或者在单行箭头函数中定义了一个回调，它隐式地返回一些东西。

例如，我们应该编写下面的代码来调用`map`,用回调函数返回一些东西:

```
const arr = [1, 2].map((a) => a * 2);
```

在上面的代码中，我们的回调是返回`a * 2`的`(a) => a * 2`。

同样，我们可以这样写:

```
const arr = [1, 2].map((a) => {
  return a * 2;
});
```

这样一来，`return`的表述就更清晰了。

我们不应该写像下面这样的东西:

```
const arr = [];
[1, 2].map((a) => {
  arr.push(a * 2);
});
```

在上面的代码中，回调没有返回任何东西。相反，它只是在回调函数之外的`arr`数组上调用`push`。它产生了一个副作用，没有返回任何东西，这两者都不好。

我们应该让它像`map`方法期望的那样在我们的回调中返回一些东西。

![](img/0f455c80b387142f0500b87e7fd79787.png)

Photo by [Adam Bignell](https://unsplash.com/@adam_29063?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 如果数组有多行，请在数组括号开始后和结束前使用换行符

如果一个数组条目有多行，那么我们应该在左括号之后和右括号之前添加一个换行符来分隔条目和括号。

这提高了数组条目的可读性，因为我们知道它们的开始和结束位置。

例如，我们可以编写以下代码来实现这一点:

```
const arr = [
  {
    a: 1
  },
  {
    b: 2
  }
]
```

在上面的代码中，我们有一个包含对象的数组，在数组的第一个条目之前和最后一个条目之后都有换行符，以使条目更容易阅读。

如果我们可以在一行中写下所有的内容，那么我们就不需要在数组代码中使用换行符来分隔条目和括号。

例如，我们可以编写以下内容:

```
const arr = [1, 2, 3];
```

在上面的代码中，我们把所有的东西都放在一行中，因为每个条目都很短，我们可以在一行中写下整个数组。

# 结论

`Array.from`方法适用于将可迭代和不可迭代的类似数组的对象转换成数组，并同时将每个条目映射到新值。

这很好，因为我们不必创建一个中间数组和调用它的`map`,就像我们必须使用 spread 操作符一样。

如果我们的数组条目很长，那么我们应该把它们写在各自的行中。

# **简明英语团队的笔记**

你知道我们有四种出版物吗？给他们一个关注来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们还推出了一个 YouTube，希望你能通过 [**订阅我们的英语频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) 来支持我们**

**一如既往,“简明英语”希望帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****