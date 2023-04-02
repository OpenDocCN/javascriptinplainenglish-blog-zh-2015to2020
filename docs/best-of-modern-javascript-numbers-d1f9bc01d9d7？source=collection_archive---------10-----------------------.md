# 现代 JavaScript 的精华——数字

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-numbers-d1f9bc01d9d7?source=collection_archive---------10----------------------->

![](img/87fd15f098acd93a8d19dec874c0d1da.png)

Photo by [Thomas S.](https://unsplash.com/@photosbythomas?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 的核心特性。

# 新的`Number`属性

ES6 引入了一些新的数字属性。

`Number`的新静态属性包括用于比较浮点数舍入误差的`Number.EPISLON`。

`Number.isInteger`检查`num`是否为整数。

如果我们写:

```
Number.isInteger(2.05)
```

然后返回`false`。

如果我们写:

```
Number.isInteger(2)
```

然后返回`true`。

这也适用于负数。

还有一个`isSafeInteger`方法可以确定 JavaScript 整数是否安全。

安全整数是可以在有符号的 53 位范围内表示而不损失精度的整数。

例如，如果我们写:

```
Number.isSafeInteger(2)
```

然后返回`true`。

还有一个`Number.MIN_SAFE_INTEGER`，它有安全整数的最小值。

还有用于获取安全整数的最大值的`Number.MAX_SAFE_INTEGER`值。

ES6 还附带了`Number.isNaN`方法来检查`num`是否是值`NaN`。

与`isNaN`不同，在进行检查之前，它不会将其参数强制转换为一个数字。

所以如果我们写:

```
isNaN('foo')
```

它返回`true`。

另一方面，如果我们写:

```
Number.isNaN('foo')
```

然后返回`false`。

`Number`对象也有`Number.isFinite`、`Number.parseFloat`和`Number.parseInt`方法。

`Number.isFinite`检查一个数是否是有限的。

`Number.parseFloat`将非数字转换为浮点数。

并且`Number.parseInt`将非数字转换为整数。

这些与它们的全球对等物大部分相同。

# `Math`方法

ES6 也给`Math`对象增加了新的方法。

方法返回一个数字的符号。

如果数字为负，则返回-1。

如果数字为 0，则返回 0。

如果数字是正数，它返回 1。

例如，我们可以写:

```
Math.sign(-10)
```

并得到-1。

如果我们写:

```
Math.sign(0)
```

我们得到 0。

如果我们有:

```
Math.sign(3)
```

我们得到 1。

方法删除一个数字的小数部分。

所以如果我们有:

```
Math.trunc(2.1)
```

或者

```
Math.trunc(2.9)
```

我们得到 2。

如果我们有:

```
Math.trunc(-2.1)
```

或者:

```
Math.trunc(-2.9)
```

我们得到-2。

`Math.log10`方法计算以 10 为基数的对数。

例如，我们可以写:

```
Math.log10(1000)
```

得到 3。

`Math.hypot`计算其参数的平方数的平方根。

例如，如果我们有:

```
Math.hypot(1, 1)
```

我们得到`1.4142135623730951`。

# 整数文字

ES5 引入了十六进制整数。

所以我们可以写:

```
0x1F
```

得到十进制的 31。

ES6 引入了更多种类的数字。

二进制文字就是其中之一。

例如，如果我们写:

```
0b111
```

然后我们得到十进制的 7。

它还引入了八进制文字。

例如，我们可以写:

```
0o101
```

得到十进制的 65。

我们可以对基数不是 10 的数字使用`toString`方法。

例如，我们可以写:

```
8..toString(8)
```

我们得到十进制的 10。

参数是基数，或者说是数字的基数。

要调用一个带有数字文字的方法，我们必须包含一个额外的点。

![](img/27e770255896b4ebf0a1b41326ab6619.png)

Photo by [Andrew Buchanan](https://unsplash.com/@photoart2018?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

数字可以用变量表示，并用 ES6 或更高版本进行转换。

## 简单英语的 JavaScript

你知道我们有三份出版物和一个 YouTube 频道吗？在 [**plainenglish.io**](https://plainenglish.io/) 找到所有内容的链接！