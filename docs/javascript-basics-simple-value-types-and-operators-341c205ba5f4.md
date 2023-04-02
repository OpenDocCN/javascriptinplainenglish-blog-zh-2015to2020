# JavaScript 基础——简单的值类型和运算符

> 原文：<https://javascript.plainenglish.io/javascript-basics-simple-value-types-and-operators-341c205ba5f4?source=collection_archive---------7----------------------->

![](img/a3eed872e7ab8eb2121ca0866fdfe388.png)

Photo by [Dan Gold](https://unsplash.com/@danielcgold?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是世界上最流行的编程语言之一。为了有效地使用它，我们必须了解它的基本知识。

在本文中，我们将研究基本的值类型和操作符。

# 价值观念

值是 JavaScript 程序中最基本的数据类型。

它们以比特的形式存储在计算机的内存中。

我们必须存储值，以便在程序的后面使用它们。

# 数字

数字是最基本的数据类型。它们存储简单的数值。

浮点类型和整数类型没有区别。

它们都存储在一种数据类型中。

JavaScript 中的数字是 64 位。这意味着有 64 个二进制数字或 2 的 64 次方个数字。

一些位用于存储小数点。

这意味着它实际上是 9 千万亿个数字，位置或负数。

我们对数字做的主要事情是算术。

我们可以用运算符对数字进行运算。

例如，我们可以写:

```
1 + 3
```

或者

```
2 * 3
```

`+`和`*`是运算符。`+`是加法，`*`是乘法。

我们可以用括号区分操作的优先级。

例如，我们可以写:

```
(1 + 3) * 2
```

将加法置于乘法之上。

如果没有括号，乘法会先进行。

减法用`-`运算符完成，除法用`/`运算符完成。

当算术表达式中没有括号时，运算符的优先级由数学规则决定。

`*`和`/`优先于`+`和`-`。

`%`代表余数运算符。

`1 % 2`表示我们得到的是 1 除以 2 的余数。

因此，`1 % 2`返回 1。

# 特殊号码

JavaScript rhat 中有一些特殊的值被认为是数字，但是它们的行为不像普通的数字。

`Infinity`和`-Infinity`分别代表正负无穷大值。

`Infinity — 2`还是`Infinity`。

我们不应该太相信这些价值观。

`NaN`代表“不是一个数字”。然而，它仍然是 number 类型。

当我们试图计算`0 / 0`、`Infinity — Infinity`或任何不会产生有意义结果的数值运算时，我们会得到这个结果。

# 用线串

字符串是另一种基本的数据类型。它用来表示文本。

它们是用引号括起来的。

我们可以用单引号、双引号或反引号来括住一个字符串。

只要开始和结束定界符匹配，我们可以使用它们中的任何一个。

例如，我们可以写:

```
'hello world'
```

或者:

```
"hello world"
```

或者:

```
`hello world`
```

如果在引用的文本中发现反斜杠字符，那么它用于转义该字符。

它不会被显示，而是用来写特殊字符，如换行符或制表符。

例如，我们可以写:

```
'hello \n'
```

其中`\n`是换行符。

字符串也以一系列位的形式存储在计算机中。

它们被编码成 Unicode 字符集，然后存储在计算机上。

除了英语字符之外，Unicode 几乎拥有我们需要的任何语言的所有字符，如希腊语、日语、数字。

字符串不能整除，乘 pr 减，但是`+`可以用在字符串上进行串联。

例如，我们可以写:

```
'hello' + 'world'
```

双引号中的字符串的行为与单引号中的字符串相同。

模板文字可以做更多的技巧。它们是用反勾括起来的字符串。

我们可以将表达式嵌入其中。例如，我们可以写:

```
`half of 50 is ${50 / 2}`
```

然后我们得到:

```
`half of 50 is 25`
```

作为最后的结果。

# 一元运算符

一元运算符对一个操作数进行运算。

其中之一是用于检查值的类型的`typeof`操作符。

例如，我们可以写:

```
typeof 10
```

为了得到`'number'`和:

```
typeof 'y'
```

去找`'string'.`

![](img/643d3182dfba684a4fae25f2f43a4a5f.png)

Photo by [Merve Aydın](https://unsplash.com/@viledaaa?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 二元运算符

接受两个操作数的运算符称为二元运算符。它们就像算术运算符一样。

# 结论

数字和字符串是基本的数据类型。可以存储大量的数字。

我们可以与各种运营商合作。它们是用 Unicode 编码的，所以我们可以存储很多字符。

一元运算符接受一个操作数。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**