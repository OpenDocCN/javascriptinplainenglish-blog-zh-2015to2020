# ES2016 的最佳特性

> 原文：<https://javascript.plainenglish.io/best-features-of-es2016-6b7a61fac242?source=collection_archive---------5----------------------->

![](img/65142b9a5d69414349f74aaf2469f6b3.png)

Photo by [Fahim](https://unsplash.com/@fahimhasan999?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将了解 ES2016 的最佳特性。

# `Array.prototype.includes`

ES2016 中增加了`includes`数组实例方法。

它让我们检查数组中是否存在某个项目。

例如，我们可以写:

```
[1, 2, 3].includes(1)
```

然后那个返回`true`。

如果我们有:

```
[1, 2, 3].includes(6)
```

然后返回`false`。

它接受我们想要检查的项目，并返回一个布尔值。

`includes`类似于`indexOf`。

所以:

```
arr.includes(x)
```

基本上与以下内容相同:

```
arr.indexOf(x) >= 0
```

主要区别是`includes`可以检查`NaN`，而`indexOf`不能。

所以如果我们写:

```
[NaN].includes(NaN)
```

然后返回`true`。

但是如果我们有:

```
[NaN].indexOf(NaN)
```

然后返回-1。

`includes`不区分+0 和-0。

所以如果我们写:

```
[-0].includes(+0)
```

然后返回`true`。

# 取幂运算符(`**`)

ES2016 中添加了求幂运算符。

它让我们不用调用`Math.pow`就能做取幂运算。

例如，如果我们有:

```
3 ** 2
```

我们得到 9。

所以:

```
x ** y
```

做的事情和:

```
Math.pow(x, y)
```

我们可以把`**`和`=`一起用。

所以我们可以写:

```
let num = 3;
num **= 2;
```

那么`num`就是 9。

## 优先

取幂运算符的绑定非常强。

所以如果我们有:

```
2**2 * 2
```

那就和:

```
(2**2) * 2
```

它的结合力比`*`更强。

并且`*`比`**`结合得更牢固。

![](img/11bd0b47b40ff33af594c5ee362585e5.png)

Photo by [Tadeusz Lakota](https://unsplash.com/@tadekl?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

ES2016 是对 ES2015 的一个小更新，es 2015 是一个大版本。

它给了每个人一年的休息时间。

新特性包括数组实例`includes`方法和取幂运算符。