# 如何修剪一个 JavaScript 字符串？

> 原文：<https://javascript.plainenglish.io/how-to-trim-a-javascript-string-fabafcb18c00?source=collection_archive---------9----------------------->

![](img/08914f888d43d21faa48444e6a33c23a.png)

Photo by [Edgar Chaparro](https://unsplash.com/@echaparro?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

修剪琴弦是我们经常要做的事情。

这是因为当空白出现时，我们经常会被绊倒。

我们看不到它们，但如果两个字符串有不同的间距，那么它们被认为是不同的。

因此，我们应该在用它们做任何事情之前修剪它们。

在本文中，我们将看看如何在 JavaScript 中修剪字符串。

# 什么是空白？

空白包括空格、制表符、不间断空格和行尾字符。

它们都由修剪空白的 JavaScript 字符串方法来修剪。

# trimStart

字符串实例是由`trimStart`方法提供的。

它将修剪字符串的开始并返回字符串。

例如，如果我们有:

```
' hello '.trimStart()
```

然后它将字符串修剪到位，我们得到:

```
"hello "
```

# 特里蒙德

同样，有一个`trimEnd`来修剪字符串的末尾并返回字符串。

所以如果我们有:

```
' hello '.trimEnd()
```

我们得到:

```
" hello"
```

# 整齐

还有一个`trim`方法来修剪开始和结束的空格并返回字符串。

例如，我们有:

```
' hello '.trim()
```

那么两端的空白将被适当地修剪掉。

所以我们得到:

```
"hello"
```

# 别名

有一种`trimLeft`方法，和`trimStart`方法一样。

此外，还有`trimRight`方法，与`trimEnd`方法相同。

有别名是因为先介绍`trimLeft`和`trimRight`。

但是为了与其他方法如`padStart`和`padEnd`保持一致，引入了`trimStart`和`trimEnd`。

现在它们共存，但是为了保持一致，我们使用`trimStart`和`trimEnd`。

# 和睦相处

大多数现代浏览器都支持这些方法。

但是，Internet Explorer 只支持`trim`。

如果我们想在 IE 中使用另外 2 个，那么我们需要为它们添加 polyfills。

![](img/9b97098aed988d13898b2020f8b04025.png)

Photo by [NeONBRAND](https://unsplash.com/@neonbrand?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

有几种方法可以修剪字符串中的空白。

除了 Internet Explorer，几乎所有的现代浏览器都可以使用它。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)和 [**找到它们订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**