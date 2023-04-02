# 多种方法展平 JavaScript 数组；有不止一种方法来皮肤🙀。

> 原文：<https://javascript.plainenglish.io/multiple-ways-to-flatten-a-javascript-array-theres-more-than-one-way-to-skin-a-e5dc2762e8e6?source=collection_archive---------9----------------------->

## 让我们看看如何在 JavaScript 中展平一个`array`。尤其是如何展平`integers`的任意嵌套`arrays`的一个`array`。

![](img/97637915f942e87de0eb8b7c75230b7d.png)

Image by [Ulrike Leone](https://pixabay.com/users/ulleo-1834854/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=1903316) from [Pixabay](https://pixabay.com/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=1903316)

# Array.flat()

[Mozilla](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference) doc 说 [**Array.flat()**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat) 方法通过递归地将所有子数组串联到您指定的深度来生成一个新数组。

简单地说，如果你有一个数组的数组(也许里面有更多的数组)， **flat()** 将帮助你把所有的条目连接到一个数组中🤗。

**flat(** [*【深度】*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat) **)** 接受递归深度作为参数。但是，这是可选的，默认情况下设置为 1。

flat() or flat(1) both are the same

## 深度嵌套数组

如果你不知道你给定的数组嵌套有多深，不要害怕！你已经`**Infinity**`去营救🦸.了

`**Infinity**`

**flat()** 也会处理数组中的空槽，**但不会删除** `**null**` ***、*** `**undefined**`或`**""**` 。嗯，🤔我们该如何处理？继续读这篇文章，你可能会找到一种方法😉。

Flat omits empty slots

# Array.toString()或模板文字

你可以把一个`array`转换成一个`string`，然后用`","`把它分割成一个新的`string`，然后再把它转换回一个`array`，这将会给你一个扁平的数组。

Flattening by stringifying an array

# `Ways to handle null`，`undefined or ""`

如前所述，使用 **flat()** 将从数组中移除空槽，但不会移除`null`、`undefined`或`""`。使用 **toString()** 你可能需要做更多的操作。但是不管你选择什么，我们都可以使用[](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)**与 [**布尔**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean) 函数。这将允许我们忽略不真实的项目。**

**Handling `null`, `undefined or ""`**

# **一些其他的选择，如果你正在处理一个层次的深度数组。**

**Other options**

**还有其他方法可以达到同样的目标，如果你考虑加入 **Array.reduce()，**我建议先看这个视频…，也许**减少减少？****

**Is reduce bad?**

## **感谢阅读😀**