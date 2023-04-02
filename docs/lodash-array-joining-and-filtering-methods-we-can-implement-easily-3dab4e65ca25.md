# 我们可以轻松实现的 Lodash 数组操作方法

> 原文：<https://javascript.plainenglish.io/lodash-array-joining-and-filtering-methods-we-can-implement-easily-3dab4e65ca25?source=collection_archive---------7----------------------->

![](img/07199122d38492c7947908e6b1478795.png)

Photo by [Lalitphat Phunchuang](https://unsplash.com/@lali_ph?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Lodash 是一个非常有用的工具库，让我们可以轻松地处理对象和数组。

然而，现在 JavaScript 标准库正在赶上诸如 Lodash 之类的库，我们可以用简单的方法实现许多功能。

在本文中，我们将看看如何自己实现一些 Lodash 数组操作方法，包括`intersectionBy`、`last`、`lastIndexOf`、`nth`、`pull`、`pullAll`和`join`。

# `intersectionBy`

Lodash `intersectionBy`方法类似于`intersection`，因为它返回一个包含所有数组中唯一条目的数组，除了我们可以传入自己的回调函数以我们的方式比较条目，而不是使用 SameValueZero 算法进行比较。

我们可以通过在数组实例的`every`方法的回调中调用回调来实现它，如下所示:

```
const intersection = (iteratee, ...arrs) => {
  let vals = [];
  for (const a of arrs[0]) {
    const inAllArrs = arrs.map(arr => arr.map(iteratee)).every((arr) => arr.map(iteratee).includes(iteratee(a)));
    if (inAllArrs) {
      vals.push(a)
    }
  }
  return [...new Set(vals)];
}
```

在上面的代码中，我们从参数中获得了一个`condition`回调函数和一个`arrs`列表。

然后我们得到第一个数组并遍历它。在循环中，我们调用`arrs`上的`every`方法来检查每个数组是否包含第一个数组中的给定项，这是我们想要检查的数组之一。

此外，为了检查项目是否包含在每个数组中，我们必须在检查之前用`iteratee`映射所有条目，包括`includes`方法。

这样，我们就可以在用`iteratee`函数转换后对它们进行比较。

然后，最后，我们将添加的项目转换为一个集合，然后用 spread 操作符将其转换回一个数组，以返回一个包含唯一项目的数组。

# `join`

Lodash `join`方法将一个数组的所有元素转换成一个由分隔符分隔的字符串。

除了在数组实例上调用普通 JavaScript 版本之外，它与普通 JavaScript 数组的`join`方法做同样的事情。Lodash `join`方法将数组作为参数。

例如，我们可以如下实现我们自己的 Lodash `join`方法:

```
const join = (arr, separator) => arr.join(separator)
```

我们可以这样称呼它:

```
const result = join([1, 2, 3], ',')
```

那么我们得到的`result`就是`1,2,3`。

# 最后的

方法返回数组的最后一个元素。我们可以很容易地创建自己的函数来返回它，如下所示:

```
const last = arr => arr[arr.length - 1]
```

那么我们可以这样称呼它:

```
const result = last([1, 2, 3])
```

我们得到`result`是我们期望的 3。

# `lastIndexOf`

Lodash 的`indexOf`方法可以用普通 JavaScript 中的`indexOf`方法轻松实现。

那么我们可以这样实现它:

```
const indexOf = (arr, value, from) => arr.lastIndexOf(value, from)
```

在上面的代码中，我们用我们正在寻找的`value`和我们正在寻找的值调用了`lastIndexOf`方法，从`from`索引开始，一直到开始。

那么我们可以这样称呼它:

```
const result = indexOf([1, 2, 3], 2, 3)
```

而`result`是 1。

# `nth`

Lodash 的`nth`方法返回数组中的第 n 个条目。显然，我们可以通过使用括号符号获得第 n 个条目来轻松实现这一点，如下所示:

```
const nth = (arr, index) => arr[index]
```

在上面的代码中，我们只返回给定数组索引的项。

那么我们可以这样称呼它:

```
const result = nth([1, 2, 3], 2)
```

那么`result`就是 3，因为我们得到了第 3 个元素。

![](img/75aa7f191722c645c155283f5fc0494a.png)

Photo by [Darinka Kievskaya](https://unsplash.com/@darisja?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# `pull`

Lodash `pull`方法返回一个新的数组，数组中的给定值被删除。

我们可以使用`filter`方法轻松实现，如下所示:

```
const pull = (arr, ...values) => arr.filter(a => !values.includes(a))
```

在上面的代码中，我们只是调用了`filter`方法，在回调中，我们像上面一样调用了`includes`方法来排除我们用`values`包含的项目，这是通过传播值创建的。

那么我们可以这样称呼它:

```
const result = pull([1, 2, 3], 2, 3)
```

我们得到`result`是`[1]`。

# pullAll

`pullAll`类似于`pull`，除了它接受一个数组而不是一个参数列表。我们可以如下实现它:

```
const pullAll = (arr, values) => arr.filter(a => !values.includes(a))
```

那么当我们这样称呼它的时候:

```
const result = pullAll([1, 2, 3], [2, 3])
```

我们得到和以前一样的`result`值。

# 结论

`join`和`lastIndexOf`方法内置在普通 JavaScript 中，因此我们可以轻松地使用它们来实现 Lodash `join`和`lastIndexOf`方法。

方法获取一个数组的第 n 个条目。我们可以用括号符号来表示。

`pull`和`pullAll`可以很容易地用`filter`方法实现。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物: [**AI in Plain English**](https://medium.com/ai-in-plain-english) ，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****