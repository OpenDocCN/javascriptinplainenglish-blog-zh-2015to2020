# 在 JavaScript 数组的特定索引中插入一个元素。

> 原文：<https://javascript.plainenglish.io/insert-an-element-in-specific-index-in-javascript-array-9c059e941a67?source=collection_archive---------0----------------------->

## 了解如何在数组的特定索引中插入元素。

![](img/26c7b0482425375cd80f61660a2eb54d.png)

**Image from unsplash (**[**Zdennek**](https://unsplash.com/@zmachacek?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)**)**

我们需要在数组的开头和结尾添加一些元素。

`push(value)` →在数组末尾添加一个元素。

`unshift(value)` →在数组的开头添加一个元素。

要向特定索引添加元素，在`Array`对象中没有可用的方法。但是我们可以在`Array`对象中使用已经可用的`splice`方法来实现这一点。

一个数组从`index 0`开始，所以如果我们想添加一个元素作为数组的第一个元素，那么这个元素的索引就是`0`。如果我们要添加一个元素到`nth`位置，那么索引就是`(n-1) th`索引。

```
**MDN :**    The **splice()** method changes the contents of an array by removing      or replacing existing elements and/or adding new elements, in the original array(which means the source array is modified)
```

`**splice()**` 方法需要三个自变量

1.  ***开始→*** 开始改变数组的索引。
2.  ***deleteCount(可选)→*** 一个整数，表示数组中要从`start`中移除的元素个数。
3.  ***(elem1，elem2 …) →*** 要添加到数组中的元素，从`start`开始。如果不指定任何元素，`splice()`将只从数组中删除元素。

为了将一个元素插入到特定的索引中，我们需要提供如下参数

1.  `start` → `**index**`在哪里插入元素
2.  `deleteCount` → `**0**`(因为我们不需要删除元素)
3.  `elem` →要插入的元素

让我们写函数

```
 function **insertAt(**array, index, ...**elementsArray) {** array.splice(index, 0, ...elements);**}**
```

现在让我们调用函数

```
var num = [1,2,3,6,7,8];/*
                           **arguments** 
 * 1\. **source array - num
 * 2\. index to insert - 3
 * 3\. remaining are elements to insert
*/****insertAt(num, 3, 4, 5);** // [1,2,3,4,5,6,7,8]________________________________________________________________// let's try changing the indexvar num = [1,2,3,6,7,8];insertAt(numbers, 2, 4,5); // [1,2,4,5,3,6,7,8]
```

感谢阅读📖。希望你喜欢这一点，如果你发现任何错别字或错误发送给我一个私人说明📝谢谢🙏 😊。

关注我 [JavaScript Jeep🚙💨](https://medium.com/u/f9ffc26e7e69?source=post_page-----98efbae5e8aa----------------------)。

**请捐款** [**这里**](https://www.paypal.com/paypalme2/jagathishSaravanan) **。你捐款的 80%捐给了需要食物的人🥘。提前感谢。**