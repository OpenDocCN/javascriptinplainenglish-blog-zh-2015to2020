# 命名布尔变量:前缀

> 原文：<https://javascript.plainenglish.io/naming-boolean-variables-prefixes-df973d7d7ec3?source=collection_archive---------3----------------------->

![](img/0b76f09952486befa66655b719c005e0.png)

Photo by [Jon Tyson](https://unsplash.com/@jontyson?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

编程最难的部分？命名你的变量。你可能通常会看到以“is”或“has”为前缀的变量，或者你可能只是看到一个普通的旧的描述性名称，因为它只是“工作”。与其浪费无数的时间去决定如何最好地命名你的变量，特别是布尔变量，不如试试看下面的建议是否能帮到你。

# 前缀很好，但不是必需的

命名布尔变量时使用前缀只会增加复杂性。当你可以简单地使用主语，你最关心的事情时，没有必要把“是”或“有”作为前缀。

**前缀**

```
const isPrefixVisible;
const hasVisiblePrefix;
```

**无前缀**

```
const prefixVisible;
const visiblePrefix;
```

上面的例子简单地描述了这样一个事实，即你可以使用不带前缀的主语，但仍然得到相同的结果。你是在形容词之前还是之后最有可能是基于你个人的发展标准，但两者都一样！

使用前缀时，您可能要着手解决的另一个内部问题是，您可能有多种方式来应用前缀。

**前缀**

```
const arePrefixesNeccessary;
const isEveryPrefixNeccessary;
```

**无前缀**

```
const prefixesNeccessary;
const everyPrefixNeccessary;
```

我同意，上面的例子有点奇怪，并且会根据布尔值是一般应用(are)还是显式应用(every)的独特应用而变化，但是它可以帮助您缩小命名选项的范围。

# 自然用法

我喜欢想，不管前缀在不在，人们都可以用不同的方式来阅读 if 语句。当你的变量名更流畅的时候，这就更容易了。

```
// Doesn't roll off the tongue
if(isPrefixVisible)// Rolls off the tongue
if(prefixVisible)
```

这些建议并不意味着创建或破坏代码，也不意味着尝试和实施一种更明确的编写代码的方式。这将令人恼火，也可以理解令人沮丧。相反，当你试图给某个东西命名时，把这些作为一点指导，但是永远记住，在一天结束时，一个笨拙的变量名称可能只是“工作”,而你能想到的其他名称只是稍微奇怪一点。