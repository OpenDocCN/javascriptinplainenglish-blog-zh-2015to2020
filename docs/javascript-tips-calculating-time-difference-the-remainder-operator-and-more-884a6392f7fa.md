# JavaScript 技巧——计算时差、余数运算符等等

> 原文：<https://javascript.plainenglish.io/javascript-tips-calculating-time-difference-the-remainder-operator-and-more-884a6392f7fa?source=collection_archive---------8----------------------->

![](img/64d1e69aefa17ef0b95a873d3613e9a6.png)

Photo by [Dawid Zawiła](https://unsplash.com/@davealmine?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# 获取两个日期时间之间的时间差，并将其格式化为 HH:mm:ss 格式

要获得两个日期时间之间的时间差，我们可以使用 moment.js 中的`diff`方法

这样，我们就不用自己做计算了，

例如，我们可以写:

```
const earlierDateTime = "04/09/2020 14:00:00";
const laterDateTime = "04/09/2020 16:20:30";const difference = moment(laterDateTime, "DD/MM/YYYY HH:mm:ss").diff(moment(earlierDateTime, "DD/MM/YYYY HH:mm:ss"))const diff = moment.utc(difference).format("HH:mm:ss");
```

我们调用`diff`得到`laterDateTime`和`earlierDateTime`的差值，并将差值转换为 UTC。

这将以`HH:mm:ss`格式输出差异。

我们得到`‘02:20:30'`作为`diff`的值。

如果两个日期时间之间的差异比今天长，那么我们必须进行一些额外的计算。

例如，我们可以写:

```
const earlierDateTime = "04/09/2020 14:00:00";
const laterDateTime = "14/09/2020 16:20:30";
const ms = moment(laterDateTime, "DD/MM/YYYY HH:mm:ss").diff(moment(earlierDateTime, "DD/MM/YYYY HH:mm:ss"));const d = moment.duration(ms);const diff = `${Math.floor(d.asHours())}${moment.utc(ms).format(":mm:ss")}`;
console.log(diff);
```

我们用`diff`方法计算两个日期之间的分和秒。

然后我们调用`duration`方法得到持续时间。

然后我们可以在 duration `d`对象上使用`asHours`方法来获得经过的小时数。

我们使用`utc`方法来格式化分和秒。

# string.charAt(x) vs string[x]

从字符串中获取字符的`charAt`和括号符号是相同的。

几乎所有现代浏览器都支持它们。

例如:

```
"foo bar"[2]
```

与以下内容相同:

```
"foo bar".charAt(2)
```

# 检查 JavaScript 对象是否是 DOM 对象

要检查一个对象是否是 DOM 对象，我们可以写:

```
typeof Node === "object" ? obj instanceof Node : 
  obj && typeof obj === "object" && typeof obj.nodeType === "number" && typeof obj.nodeName === "string"
```

我们检查`Node`对象是否存在。

如果是，那么我们用`instanceof`操作符检查`obj`对象。

否则，我们必须检查一些属性是否存在。

我们检查`nodeType`属性，`nodeName`是一个字符串。

为了检查一个元素，我们可以写:

```
typeof HTMLElement === "object" ? obj instanceof HTMLElement : 
  typeof obj === "object" && obj !== null && obj.nodeType === 1 && typeof obj.nodeName === "string"
```

我们检查浏览器中是否存在`HTMLElement`对象。

如果有，我们就使用`instanceof`操作符。

否则，我们检查`obj`是否是一个对象，并检查`nodeType`和`nodeName`的属性。

1 表示该节点是一个 HTML 元素。

# 获取字符串的长度

我们可以使用字符串的`length`属性来获取它的长度。

例如，我们可以写:

```
const str = 'foo';
const length = str.length;
```

# 获取数字的小数部分

为了得到一个数字的小数部分，我们可以使用`%`操作符。

我们得到这个数除以 1 的余数。

例如，我们可以写:

```
2.555 % 1
```

然后我们得到:

```
0.5550000000000002
```

已退回。

我们也可以将它转换成一个字符串，并对其调用`split`:

```
(2.555).toString().split(".");
```

小数部分将在返回数组的索引 1 中。

# 加载页面时运行函数

当页面被加载时，我们可以通过将函数设置为`window.onload`属性的值来运行函数。

例如，我们可以写:

```
window.onload = () => {
  console.log('page loaded');
}
```

我们将该函数设置为`onload`的值，这样它将在页面加载时运行。

# 确定一个数是否是奇数

我们可以用余数运算符来确定一个数是否是奇数。

例如，我们可以写:

```
x % 2 !== 0
```

如果`x`不能被 2 整除，那么我们知道它是奇数。

![](img/45f227a548dd76aab33a6c669a75f6b9.png)

Photo by [Charlie Hammond](https://unsplash.com/@charlieh?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用 moment.js 将时间跨度格式化为 HH:mm:ss 格式。

我们可以使用`%`运算符来确定一个数字是否是奇数。

大多数时候，我们可以使用内置的构造函数来检查一个对象是否是 DOM 节点或元素。

得到一个数的小数部分可以通过得到除以 1 的余数来完成，或者将它转换成一个字符串并拆分它。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**