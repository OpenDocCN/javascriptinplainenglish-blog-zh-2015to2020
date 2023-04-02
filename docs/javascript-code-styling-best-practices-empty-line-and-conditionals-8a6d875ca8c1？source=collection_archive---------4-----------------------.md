# JavaScript 代码样式最佳实践—空行和条件

> 原文：<https://javascript.plainenglish.io/javascript-code-styling-best-practices-empty-line-and-conditionals-8a6d875ca8c1?source=collection_archive---------4----------------------->

![](img/b7b97edfd546efe4f19278b16032c5e9.png)

Photo by [Keith Johnston](https://unsplash.com/@acfb5071?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。编写可以运行但有错误的代码很容易。

在这篇文章中，我们将看看如何清理空行并以正确的方式编写条件句。

# 没有多个空行

我们的代码不应该有太多的空行。如果我们有两个以上的表达或陈述，那么可能太多了。

多一行空行只会占用页面上额外的空间，对我们没有任何好处。

因此，我们应该从代码中删除多余的空行。如果我们需要，一个空行就足够了。

# 否定条件

被否定的条件可能会让一些人感到困惑。如果我们有双重否定，那就更是如此。

例如，如果我们有一个与一个`else`块组合的`if`语句，那么我们应该尽可能在括号之间放置正条件，而不是负条件。

例如，不用编写下面的代码:

```
if (!a) {
  foo();
} else {
  bar();
}
```

我们可以改为写:

```
if (a) {
  bar();
} else {
  foo();
}
```

之前没有否定运算的条件表达式比有否定表达式的条件表达式更容易阅读。

双重否定更让读者困惑。例如，如果我们有以下代码:

```
if (!(a !== b)) {
  bar();
} else {
  foo();
}
```

那就更混乱了。相反，我们应该通过编写下面的代码去掉`a !== b`表达式前的否定运算符:

```
if (a === b) {
  bar();
} else {
  foo();
}
```

正如我们所看到的，上面的条件表达式更容易阅读，也更简短。因此，过度使用求反运算符没有任何好处。

我们还减少了三元表达式中求反运算符的使用。例如，不用编写下面的代码:

```
const foo = !(a !== b) ? bar() : foo();
```

其中有一个双重否定条件表达式，我们应该改为:

```
const foo = (a === b) ? bar() : foo();
```

与`if`语句的情况一样，代码更短，更容易阅读。

然而，我们可以将两个求反操作符配对在一起，将一些东西转换成布尔运算。

例如，如果我们有以下变量:

```
const foo = 1;
```

然后，我们可以通过编写以下表达式将其转换为布尔值:

```
const bar = !!foo;
```

那么`bar`将会是`true`，因为`foo`已经被转换为布尔值，因为 1 是真的。

![](img/9df91eab0cefa83538435dbc1af6937f.png)

Photo by [Karen Ciocca](https://unsplash.com/@kciocca?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 没有嵌套的三元表达式

嵌套的三元表达式很难读懂。它们也很长。很难读懂，因为它们的嵌套不是用括号分隔的。

这并不好，因为为了正确地读取嵌套的三元表达式，我们必须在头脑中放入括号。

否则，它们可能会以错误的顺序读取，从而导致三元表达式的解释不正确。

例如，如果我们有以下三元表达式:

```
const foo = a ? b ? c ? 'foo' : 'bar' : 'baz' : 'qux';
```

然后，我们必须在头脑中放入括号，以便正确阅读。我们要这样读它:

```
const foo = a ? (b ? (c ? 'foo' : 'bar') : 'baz') : 'qux';
```

这使得理解表达式变得更容易了，因为我们知道它是由内向外求值的。

首先，`(c ? ‘foo’ : ‘bar’)`是 evalauted。然后用`(c ? ‘foo’ : ‘bar’)`的返回值对`(b ? (c ? ‘foo’ : ‘bar’) : ‘baz’)`求值。

然后用`(b ? (c ? ‘foo’ : ‘bar’) : ‘baz’)`的返回值对整个表达式求值。

正如我们所看到的，嵌套的三元运算符正在消耗我们的大脑。因此，我们应该避免使用它们，坚持使用非嵌套的三元表达式，如:

```
const foo = a ? 'foo' : 'bar';
```

# 结论

多空行没用。这只是占用空间，他们没有我们的代码更容易阅读。

我们不应该有过多的否定算子。否定对我们的大脑来说很难，所以我们应该尽可能地避免它们。

同样，如果我们有双重否定，那么我们应该把它们转换成非否定的条件表达式。

最后，三元表达式不应该嵌套。嵌套的三元表达式很难读懂。

# **简明英语团队的笔记**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页 [**plainenglish.io**](https://plainenglish.io/) 找到所有这些内容——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**