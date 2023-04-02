# 基本的 JavaScript 面试练习

> 原文：<https://javascript.plainenglish.io/basic-javascript-interview-exercises-c009032aacfc?source=collection_archive---------1----------------------->

![](img/1a7e71ec0747259d2dda81f36e7e496e.png)

Photo by [Anupam Mahapatra](https://unsplash.com/@mister_a?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了得到一份前端开发人员的工作，我们需要搞定编码面试。

在这篇文章中，我们将看看一些每个人都应该知道的快速热身问题。

# **写一个反转字符串的函数**

我们可以使用 JavaScript 的字符串和数组方法轻松地反转一个字符串。

为此，我们可以编写以下函数:

```
const reverseString = (str) => str.split('').reverse().join('');
```

该函数的工作原理是将字符串拆分成一个字符数组，然后反转该数组，再将它连接起来。

我们使用了 JavaScript 字符串的`split`方法和数组的`reverse`和`join`方法。

# 写一个从列表中过滤出数字的函数。

我们可以用`isNaN`函数和数组的`filter`方法来实现。

为了解决这个问题，我们可以编写以下代码:

```
const removeNums = (arr) => arr.filter(a => isNaN(+a));
```

在`removeNums`函数中，我们传入一个数组`arr`，然后用一个返回`isNaN(+a)`的回调函数对其调用`filter`，如果当我们试图将一个项目转换为一个数字时，该回调函数是`true`。

# 写一个函数，在一个未排序的列表中寻找一个元素。

我们可以使用数组的`findIndex` 方法来查找未排序列表中的元素。

我们编写以下函数:

```
const search = (arr, searchItem) => arr.findIndex(a => a === searchItem);
```

去做搜索。`findIndex`方法返回它在回调中找到的满足条件的第一个项目的索引。

我们可以如下使用它:

```
console.log(search(['foo', 'bar', 1], 'foo'));
```

搜索字符串`'foo'`。然后我们应该得到 0，因为`'foo'`是数组中的第一个条目。

# 写一个展示闭包用法的函数。

闭包是一个返回函数的函数。它通常用于将一些数据隐藏在外部函数中，以防止它们从外部暴露出来。

我们可以这样写:

```
const multiply = (first) => {
  let a = first;
  return (b) => {
    return a * b;
  };
}
```

那么我们可以这样称呼它:

```
console.log(multiply(2)(3));
```

也就是说我们有 6 个。

# 什么是承诺？写一个返回承诺的函数。

承诺是一段不确定时间内运行的异步代码。

它可以有待定、已履行或已拒绝状态。实现意味着成功，拒绝意味着失败。

承诺的一个例子是 Fetch API。它返回一个承诺，我们可以用它来返回一个承诺。

例如，我们可以写:

```
const getJoke = async () => {
  const res = await fetch('http://api.icndb.com/jokes/random')
  const joke = await res.json();
  return joke;
}
```

从查克·诺里斯 API 得到一个笑话。

![](img/8de08e8cb50f9924b400617cac9e7ea6.png)

Photo by [Jonathan Borba](https://unsplash.com/@jonathanborba?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 写一个函数来展平一个项目列表。

我们可以使用 array `flat`方法来展平一个项目数组。

例如，我们可以写:

```
const flatten = arr => arr.flat(Infinity);
```

将数组的所有级别展平为一个级别。

我们还可以循环遍历数组中的每一项，并将嵌套的数组归入一个层次。

例如，我们可以编写以下内容:

```
const flatten = (arr = []) => {
  let result = [];
  for (let item of arr) {
    if (Array.isArray(item)) {
      result = result.concat(flatten(item));
    } else {
      result = result.concat(item);
    }
  }
  return result;
}
```

展平数组。

代码通过遍历数组的每个条目来工作。然后在条目中找到数组，`flatten`调用自己，然后将它附加到`result`。否则，它只是将该项添加到`result`数组中。

在它通过所有关卡后，它返回`result`。

# **编写一个函数，接受两个数** `**a**` **和** `**b**` **，并返回两个数** `**a**` **和** `**b**` **的除法以及它们对** `**a**` **和** `**b**` **的取模。**

我们可以通过使用`/`操作符将`a`和`b`相除，并使用`%`操作符找出`a`除以`b`的余数。

为此，我们可以编写以下函数:

```
const divMod = (a, b) => {
  if (b !== 0) {
    return [a / b, a % b];
  }
  return [0, 0];
}
```

我们检查除数是否为零，然后我们执行前面提到的操作，并返回数组中的计算项。

# 结论

我们可以尽可能多地使用数组和字符串方法，让我们的生活变得更轻松。

它们也比我们从头开始实现的更高效。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物，表达对它们的爱:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****