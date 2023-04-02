# JavaScript 中的克隆与 Ruby 中的克隆

> 原文：<https://javascript.plainenglish.io/cloning-in-javascript-v-cloning-in-ruby-c63061eeda14?source=collection_archive---------4----------------------->

## 在学习 JavaScript 时，细微的实现差异经常会让我用不同的方式处理问题。

![](img/37932c701b073b190cdd855c8859b845.png)

Photo by Grace Madeline on Unsplash

## 我是从 Ruby 的优势来学习编程的，Ruby 有一个清晰的内置方法来制作数组的浅层副本。

在学习 JavaScript 时，细微的实现差异经常会让我用不同的方式处理问题。在 JS 中，没有被称为`clone` 或`dup`的函数，甚至没有专门为创建浅层副本而创建的`initialize_copy`。有这样的 Ruby 方法。然而，认为没有这样的功能是天真的。

我开始依靠对克隆阵列的操作来解决一个问题。然后，我采用了不同的方法，因为我没有立即发现如何克隆阵列。用其他方法解决问题后，我找到了克隆阵列所需的工具。下面简单描述一下我的旅程。

## **任务**

启动学校的课程让我创建自己版本的 JS `unshift`函数。目的是让学生熟悉内置 JavaScript 函数的实现。

`[unshift](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/unshift)` 通过在数组开头插入新值来修改数组。新值的索引是`0`，并且所有其他数组值的索引比它们之前的大 1。

```
var array1 = [1, 2, 3];console.log(array1.unshift(4, 5));
// expected output: 5console.log(array1);
// expected output: Array [4, 5, 1, 2, 3]
```

该实现意味着使用带有`for`的基本迭代来完成。没有巧妙解决的空间。这不是一个面向对象的解决方案，所以新函数是用一个要修改的数组参数调用的，而不是像 JS 内置版本那样在数组对象本身上调用。

我最初的实施过程是:

1.  克隆输入数组
2.  使用括号符号使插入值成为输入数组的第一个值
3.  迭代克隆的数组
4.  使迭代中的每个对象成为输入数组的后续索引
5.  返回数组的`length`

不幸的是，我在第一步就被打断了。

## **为什么是克隆？**

由于变量作为指针的概念，如果我简单地将原始数组赋给一个新变量，两个变量将指向同一个数组。修改存储数组的新变量也会修改原始数组。

```
var arr = [1, 2, 3];
var arr1 = arr;
arr1[0] = 'New Value';arr1;                  // ['New Value', 2, 3]
arr;                   // ['New Value', 2, 3]
```

副本允许您修改包含与原始数组具有相同值的元素的数组，而无需修改原始数组。

我想复制，因为我最初的方法意味着修改复制数组的第一个索引(即索引 0)，然后迭代原始数组，并在复制数组的后续索引中讲述其元素。

我没有立即推断如何复制。

因此，我对原始数组进行迭代，将元素值存储在临时变量中，并在每次迭代中重新赋值:

```
var count = [1, 2, 3];function unshift(arr, insertionValue) {
  var currentValue;
  var newValue = insertionValue;
  var iterations = arr.length;
  for (var i = 0; i <= iterations; i += 1) {
    currentValue = arr[i];
    arr[i] = newValue;
    newValue = currentValue;
  }
  return arr.length;
}console.log(unshift(count, 0));      // 4
console.log(count);                  // [ 0, 1, 2, 3 ]
```

这更接近于 JS 函数的镜像，因为它返回原始的修改后的数组，而不是一个副本。

## **问题仍然是，‘我如何克隆？’**

虽然问题解决了，但是一个 JS 开发者用什么方法论来获得浅层副本呢？

同名的 Ruby 方法:`[slice](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)`。

当 Ruby 开发人员只想复制数组的一部分时，可能会想到这种方法。然而，JavaScript 提供了`slice`，而不是创建一个冗余的方法，本质上用另一个名字做同样的事情。要获得数组一部分的副本，请指示起始和结束索引。要复制整个数组，从索引 0 开始。

```
var arr = [1, 2, 3];
var arr1 = arr.slice(0);  // arr.slice(0, 3) would also work
arr1[0] = 'New Value';arr1;                     // ['New Value', 2, 3]
arr;                      // [1, 2, 3]
```

对于 Ruby 开发者来说，这是非常有效的，尽管不够直观。

## **惯用的红宝石诉恩……**

Ruby 的设计意图是简单易读([‘惯用 Ruby:编写漂亮的代码](https://medium.com/the-renaissance-developer/idiomatic-ruby-1b5fa1445098)’)。这意味着开发人员的生产力优先于语言优化。

JavaScript 最初在 10 天内实现，只是为了在浏览器中提供脚本功能。它不一定是为了清晰而开发的。

这并不是说 JavaScript 开发人员不容易理解从零开始“切片”的意图。从一个角度来看，“如果功能已经存在，为什么还要在另一个功能中复制它？”。那不是干代码吗。

Ruby 反驳:更清楚了。如果一个方法只打算做一件事，那么在阅读另一个开发人员的代码时，更容易发现他们的意图。

这难道没有反映出“在一个系统中，每一项知识都必须有一个单一的、明确的、权威的表示”吗，正如在 [*《实用程序员*](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) 中对 DRY 的定义所述？

让读者来评判吧。