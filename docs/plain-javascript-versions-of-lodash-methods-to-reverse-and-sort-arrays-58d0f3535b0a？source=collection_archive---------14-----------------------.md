# Lodash 方法的普通 JavaScript 版本，用于反转和切片数组

> 原文：<https://javascript.plainenglish.io/plain-javascript-versions-of-lodash-methods-to-reverse-and-sort-arrays-58d0f3535b0a?source=collection_archive---------14----------------------->

![](img/8ea583851adc4f69a1abcd13402b1c34.png)

Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Lodash 是一个非常有用的工具库，让我们可以轻松地处理对象和数组。

然而，现在 JavaScript 标准库正在赶上诸如 Lodash 之类的库，我们可以用简单的方法实现许多功能。

在本文中，我们将看看如何用普通的 JavaScript 代码替换`reverse`、`slice`、`tail`、`takrRight`、`takeRightWhile`和`take`方法。

# 反面的

Lodash `reverse`方法反转数组，使第一个元素变成最后一个，第二个变成倒数第二个，依此类推。

这个方法是基于`Arrar.prototype.reverse`的，所以我们不需要使用 Lodash 的`reverse`来反转数组。

我们可以如下实现 Lodash 的`reverse`方法:

```
const reverse = arr => arr.reverse()
```

那么我们可以这样称呼它:

```
const result = reverse([1, 2, 3]);
```

我们得到`result`是`[3, 2, 1]`。

# `slice`

Lodash 的`slice`方法返回从开始索引到结束索引的数组切片。我们可以指定`start`和`end`索引。

它基于`Array.prototype.slice`所以我们不需要使用它。如果我们真的想要 Lodash 的`slice`方法，我们可以如下实现它:

```
const slice = (arr, start, end) => arr.slice(start, end)
```

我们只是用`start`和`end`索引调用了`arr`的`slice`方法。

我们可以这样称呼它:

```
const result = slice([1, 2, 3], 1, 2);
```

我们得到`result`就是`[2]`。

# `tail`

方法返回一个除了第一个元素之外的所有元素的数组。

我们可以如下实现它:

```
const tail = (arr) => arr.slice(1)
```

在上面的代码中，我们只是用 1 调用了`slice`来排除第一个元素

我们可以这样称呼它:

```
const result = tail([30, 40, 50]);
```

那么`result`就是`[40, 50]`。

# `take`

`take`返回一个数组的切片，其中`n`的元素是从开始取的

同样，我们可以用`slice`实现如下:

```
const take = (arr, n) => arr.slice(n)
```

将`n`作为`slice`的第一个参数传递会返回一个移除了第一个`n`条目的数组。

我们可以这样称呼它:

```
const result = take([30, 40, 50], 2);
```

然后我们得到`result`是`[50]`。

# `takeRight`

`takeRight`方法返回一个数组的切片，其中的`n`元素从末尾移除。

同样，我们可以如下使用`slice`:

```
const takeRight = (arr, n) => arr.slice(0, -n)
```

在上面的代码中，我们将 0 和`-n`传递给`slice`，这样我们就可以返回一个数组，其中包含从第一个到第 n 个最后一个条目。

那么当我们这样称呼它的时候:

```
const result = takeRight([30, 40, 50], 2);
```

我们得到`result`是`[30]`。

![](img/197865447ffce0d0d7707f8d5b5866d7.png)

Photo by [Daniel Eledut](https://unsplash.com/@pixtolero2?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# `takeRightWhile`

`takeRightWhile`返回一个新数组，从右边的原始数组中取出条目，直到我们用来通过返回数组来结束方法的给定谓词返回`true`。

我们可以如下实现它:

```
const takeRightWhile = (arr, predicate) => {
  let takenArr = [];
  for (let i = arr.length - 1; i >= 0; i--) {
    if (!predicate(arr[i])) {
      takenArr.push(arr[i]);
    } else {
      return takenArr.reverse();
    }
  }
  return takenArr.reverse();
}
```

在上面的代码中，我们用一个`for`循环反向循环通过`arr`。然后我们在每次迭代中运行`predicate`函数，将每个元素作为参数，看看它是否返回`true`。

如果没有，那么我们将条目推入`takenArr`。否则，我们反转返回`takenArr`,这样我们就有了原始顺序的元素。

那么我们可以这样称呼它:

```
const result = takeRightWhile([30, 40, 50], a => a < 40);
```

并且`result`是`[40, 50]`，因为我们指定如果用`predicate`调用的第一个条目返回`true`，我们就停止。

# 结论

用简单的循环可以很容易地实现`takeRightWhile`方法。我们只需要反向循环遍历条目，而不是向前循环，先从末尾开始获取条目，然后再向开头前进。

`takeRight`、`tail`和`take`方法可以用`slice`实现。

`reverse`和`slice`方法已经内置在普通的 JavaScript 中，无论如何它都有 Lodash 用法，所以我们可以只使用普通的 JavaScript 版本。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****