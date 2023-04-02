# 基本 JavaScript 概念前端开发—数组

> 原文：<https://javascript.plainenglish.io/basic-javascript-concepts-front-end-development-arrays-879f248d0db6?source=collection_archive---------6----------------------->

![](img/e6b336080dd3f97028a8e5a34e941315.png)

Photo by [Erol Ahmed](https://unsplash.com/@erol?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果我们想成为一名前端开发人员，那么学习 JavaScript 是必须的。

在本文中，我们将研究 JavaScript 概念，学习如何成为一名高效的前端开发人员。

# 数组方法

JavaScript 数组方法是真正的救星。因此，我们应该尽可能多地使用它们。

这将让我们避免编写大量的循环，相反，我们可以在没有循环的情况下进行许多数组操作。

一些常见的方法如下。

## 地图

`map`是将每个数组条目从一个值映射到另一个值的好方法。

例如，我们可以写:

```
const arr = [1, 2, 3];
const mapped = arr.map(a => a ** 2);
```

上面的代码通过平方每个条目来映射`arr`中的每个条目，然后将映射的值放入一个新数组并返回它。

## **过滤器**

`filter`返回满足我们指定条件的所有条目的数组。

例如，我们可以写:

```
const arr = [1, 2, 3];
const evens = arr.filter(a => a % 2 === 0);
```

在上面的代码中，我们调用了`filter`来返回一个包含了`arr`中所有偶数的数组。

## **减少**

`reduce`返回一个原始值或对象，它以我们想要的方式组合了一个数组的所有条目。

我们可以如下使用它:

```
const arr = [1, 2, 3];
const reduced = arr.reduce((total, a) => total + a, 0);
```

上面的代码将`arr`的所有条目相加，并返回总和。

回调将结果累积为第一个参数，在本例中是`total`，当前条目作为第二个参数处理。

`reduce`的第二个自变量是初始值。

我们应该包括这一点，因为我们的数组中并不总是有足够的数字来进行组合运算。

## **找到**

让我们找到给定条件下数组的第一个条目。

例如，我们可以写:

```
const arr = [1, 2, 3];
const found = arr.find(el => el < 2);
```

上面的代码获取第一个小于 2 的条目并返回它，所以我们得到 1。

## **查找索引**

`findIndex`类似于`find`，但是返回第一个找到的条目的索引，而不是条目本身。

例如，我们可以写:

```
const arr = [1, 2, 3];
const found = arr.findIndex(el => el < 2);
```

我们得到 0，因为 1 在数组的第一个槽中。

## **索引 Of**

`indexOf`让我们找到数组中某个值第一次出现的索引。

它使用`===`进行比较。

例如，我们可以写:

```
const arr = [1, 2, 3];
const found = arr.indexOf(2);
```

因为 2 在`arr`的第二个槽中，所以我们为`found`得到 1。

## **按下**

`push`让我们将新条目推到数组的末尾。它返回添加到数组中的项。

我们可以通过书写来使用它:

```
const arr = [1, 2, 3];
const pushed = arr.push(4);
```

然后我们得到在调用`push`之后`arr`是`[1,2,3,4]`，它返回 4 作为值。

## **弹出**

`pop`让我们删除数组的最后一个条目。

它将返回从数组中移除的项。

例如，如果我们有:

```
const arr = [1, 2, 3, 4, 5];
const popped = arr.pop();
```

那么`arr`现在是`[1,2,3,4]`而`popped`是 5。

![](img/ff48092edac345566c872e5dfd6dafc2.png)

Photo by [Athul Ben](https://unsplash.com/@gudguyben?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## **换档**

`shift`方法让我们移除数组的第一项。

它还会返回被删除的项目。

例如，我们可以写:

```
const arr = [1, 2, 3, 4, 5];
const shifted = arr.shift();
```

然后我们得到`[2, 3, 4, 5]`，因为`arr`和`shifted`的值都是 1。

## **解运**

`unshift`执行与`shift`相反的操作，将一个项目添加到数组的开头。

它返回已经添加的项目。

我们可以如下使用它:

```
const arr = [1, 2, 3, 4, 5];
const unshifted = arr.unshift(6);
```

然后我们得到`[6, 1, 2, 3, 4, 5]`，因为`arr`和`unshifted`的值都是 6。

# 结论

我们可以用数组方法很容易地在数组中添加和删除项目。

此外，我们可以毫不费力地在里面找到物品。

因此，我们应该使用它们，忘记编写循环来做这些数组操作。

# **一张用简单英语写的便条**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页[**plain English . io**](https://plainenglish.io/)上找到所有这些信息——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**