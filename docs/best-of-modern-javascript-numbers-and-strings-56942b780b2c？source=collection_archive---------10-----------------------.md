# 现代 JavaScript 的精华——数字和字符串

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-numbers-and-strings-56942b780b2c?source=collection_archive---------10----------------------->

![](img/cbca0a3711c923e2f4cf2236a318654e.png)

Photo by [Martin Sanchez](https://unsplash.com/@martinsanchez?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 的核心特性。

# 新`Number Methods`

`Number.parseInt`可以和新的整数文字一起使用。

我们可以传入基数，或者基数来处理第二个参数中的数字。

例如，我们可以写:

```
Number.parseInt('0o8', 8)
```

并得到 0。

`Number.parseInt`也适用于十六进制数字。

例如，我们可以写:

```
Number.parseInt('0xF', 16)
```

得到 15 分。

# 对指数运算和对数运算使用 0 而不是 1

`Math`对象提供了新的方法来让我们计算自然对数。

我们可以用`Math.expm1(x)`法计算`Math.exp(x) — 1`。

所以我们可以写:

```
Math.expm1(10)
```

得到 20000 英镑. 36860.68868868661

还有一个方法是`Math.expm1`的逆，也就是`Math.log1p(x)`。

和`Math.log(1 + x)`一样。

例如，如果我们有:

```
Math.log1p(0)
```

那么我们得到 0。

# 以 2 和 10 为基数的对数

我们有以 2 为底计算对数的`Math.log2(x)`。

例如，我们可以写:

```
Math.log2(8)
```

返回 3。

`Math.log10(x)`方法返回基数为 10 的`x`的对数。

例如，如果我们写:

```
Math.log10(1000)
```

那么我们得到 3。

# 字符串特征

ES6 附带了一些方便的字符串特性。

它附带了模板文字，允许我们将表达式插入到字符串中，而不是将字符串与其他表达式连接起来。

例如，我们可以写:

```
const firstName = 'james';
const lastName = 'smith';
console.log(`Hello ${firstName} ${lastName}!`);
```

然后我们得到`'Hello james smith!’`。

它们由反斜杠而不是单引号或双引号分隔。

与常规字符串不同，它们还可以跨多行。

例如，我们可以写:

```
const multiLine = `
<!DOCTYPE html>
<html lang="en"><head>
 <meta charset="utf-8">
 <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
 <meta name="theme-color" content="#000000">
 <link rel="manifest" href="%PUBLIC_URL%/manifest.json">
 <link rel="shortcut icon" href="%PUBLIC_URL%/favicon.ico">
 <title>React App</title>
</head><body>
 <div id="root"></div> 
</body></html>`;
```

并且 HTML 字符串将跨越多行。

# 码位转义

ES6 允许我们对任何代码点进行转义，即使它们超过了 16 位。

例如，我们可以写:

```
console.log('\u{1F680}');
```

为了转义`1F680`，超出了 16 位的限制。

# 迭代字符串

字符串是可迭代对象。

因此，我们可以用 for-of 循环遍历它们。

例如，我们可以写:

```
for (const ch of 'abc123') {
  console.log(ch);
}
```

然后我们在字符循环时记录它们。

for-of 循环沿代码点边界拆分字符串，因此它返回的字符串可以返回 1 或 2 个字符。

例如，如果我们有:

```
for (const ch of 'x\uD83D\uDC69') {
  console.log(ch.length);
}
```

那么`ch`第一个字符为 1，第二个字符为 2。

我们可以使用 spread 操作符将字符串沿着代码点分割成一个数组，并使用`length`属性来获得长度。

例如，我们可以写:

```
[...'x\uD83D\uDC69'].length
```

我们得到 2。

![](img/da35325897bcffa958c26da842c5e8cf.png)

Photo by [Анна В](https://unsplash.com/@niakris?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

ES6 引入了许多我们可能已经错过的数字和字符串特性。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**