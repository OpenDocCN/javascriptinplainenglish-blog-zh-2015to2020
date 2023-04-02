# JavaScript 代码格式—变量声明和赋值

> 原文：<https://javascript.plainenglish.io/javascript-code-formatting-variable-declarations-and-assignments-19da21a8450a?source=collection_archive---------8----------------------->

![](img/9826257af0fc3223eafd2a9251cdee88.png)

Photo by [Lucrezia Carnelos](https://unsplash.com/@ciabattespugnose?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。编写可以运行但有错误的代码很容易。

在本文中，我们将看看声明变量、运算符赋值以及在操作数之间放置换行符的最干净的方法。

# 多个变量一起声明还是分开声明？

多个变量最好分开声明。这样，我们马上就知道它是用`var`、`let`还是`const`声明的。

例如，不用编写下面的代码:

```
let x = 1,
  y = 2;
```

我们写道:

```
let x = 1;
let y = 2;
```

通过这种方式，我们知道`x`和`y`都是用`let`声明的，消除了歧义，也不需要查找 JavaScript 的语法来了解声明了什么。

两个例子都用`let`声明了`x`和`y`，所以我们知道它们都是块范围的变量。

# 变量声明的换行符

如果我们只有多个没有赋值的变量，就不需要在变量声明周围换行。

例如，如果我们有以下代码:

```
let x, y, z;
```

然后它们都用`let`关键字声明。因此，我们不必明确地把它们都写出来。

然而，这可能会给一些人带来困惑，所以当我们在一行中用逗号分隔所有变量时要小心。

因此，我们可能需要分别声明它们，如下所示:

```
let x;
let y;
let z;
```

# 赋值运算符速记

赋值操作符 shorthands 对于在保持代码清晰的同时缩短代码非常有用。

以下是作业速记:

```
x += y    | x = x + y
x -= y    | x = x - y
x *= y    | x = x * y
x /= y    | x = x / y
x %= y    | x = x % y
x <<= y   | x = x << y
x >>= y   | x = x >> y
x >>>= y  | x = x >>> y
x &= y    | x = x & y
x ^= y    | x = x ^ y
x |= y    | x = x | y
```

左边是短手，右边是长手。

前 4 个分别是加法、减法、乘法和除法。第五个是如果余数运算符，其余的是按位运算符。

它们都以左边的变量和右边的值作为操作数来计算和返回值，然后将返回值赋给左边的变量。

正如我们所看到的，与长句相比，短句确实节省了大量的打字时间。

我们没有在长格式表达式中使用两个操作符来执行所需的操作，然后将返回值赋给同一个变量，而是使用一个赋值操作符简写来用新值更新变量。

这为我们节省了大量的输入，并且保持了代码的清晰性。因此，这些是我们应该在代码中使用的简写。

# 操作员一致的换行符样式

对操作者来说，一致的链接中断风格是有意义的，因为这使得我们的代码更整洁，因此更容易阅读。

例如，我们可以编写以下代码，让每个运算符和操作数各占一行:

```
const total = numApples +
  numOranges +
  numGrapes;
```

在上面的代码中，我们有 3 行代码，用`+`操作符连接 3 个表达式。

我们在每一行的末尾都有`+`标志。此外，我们可以在每一行的末尾加上`+`符号。

![](img/dec38b5786b08a01d587a39fd4f3dbe2.png)

Photo by [Roberto Nickson](https://unsplash.com/@rpnickson?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 块内填充

代码块中的填充是代码块体末尾开始处的空行。

我们不需要它们，因为没有它们代码已经足够清晰了。因此，不用编写下面的代码:

```
if (foo) {

  bar();

}
```

我们可以改为写:

```
if (foo) {
  bar();
}
```

在不影响清晰度的前提下节省页面空间。

# 结论

多个变量最好在它们自己的行中声明，以消除任何歧义。

赋值操作符的简写很好，因为它们将更新变量的操作从使用两个操作符减少到一个操作符，而不会降低代码的清晰度。

块内的填充没什么用，只会占用额外的空间，所以我们可以删除它们。

# **简明英语团队的笔记**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页 [**plainenglish.io**](https://plainenglish.io/) 找到所有这些内容——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**