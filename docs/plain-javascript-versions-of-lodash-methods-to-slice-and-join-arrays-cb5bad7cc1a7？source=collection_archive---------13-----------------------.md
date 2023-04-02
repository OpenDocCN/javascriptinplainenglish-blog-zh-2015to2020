# 用于对数组进行切片和连接的 Lodash 方法的纯 JavaScript 版本

> 原文：<https://javascript.plainenglish.io/plain-javascript-versions-of-lodash-methods-to-slice-and-join-arrays-cb5bad7cc1a7?source=collection_archive---------13----------------------->

![](img/99dd54c76735d5a8e1df30ffd8106930.png)

Photo by [Josh Bean](https://unsplash.com/@jtbean?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Lodash 是一个非常有用的实用程序库，它让我们可以很容易地处理对象和数组。

然而，现在 JavaScript 标准库正在追赶 Lodash 这样的库，我们可以用简单的方法实现许多功能。

在本文中，我们将研究如何用简单的 JavaScript 代码替换`takeWhile`、`union`、`unionBy`和`unionWith`。

# `takeWhile`

`takeWhile`与`takeRightWhile`相反。它返回一个数组的切片，其元素从开始处开始，直到带有给定数组条目的`predicate`函数返回`true`。

我们可以很容易地通过`slice`自己实现，如下所示:

```
const takeWhile = (arr, predicate) => {
  let takenArr = [];
  for (const a of arr) {
    if (!predicate(a)) {
      takenArr.push(a);
    } else {
      return takenArr;
    }
  }
  return takenArr;
}
```

在上面的代码中，我们使用`for...of`循环遍历条目，如果`predicate(a)`返回`true`，则返回`takenArr`。

如果循环结束，我们也返回`takenArr`，如果`predicate(a)`从未返回`true`。

当我们这样称呼它时:

```
const result = takeWhile([30, 40, 50], a => a > 40);
```

我们得到`result`是`[30, 40]`，因为我们指定如果一个元素大于 40，我们就停止。

# `union`

`union`方法使用 SameValueZero 等式比较，从多个数组中按顺序返回具有唯一值的新数组。

我们可以通过 spread 运算符自己实现，如下所示:

```
const union = (...arrs) => {
  return arrs.reduce((arr1, arr2) => [...new Set([...arr1, ...arr2])])
}
```

在上面的代码中，我们在`arrs`上调用了`reduce`，并使用了一个回调函数来返回用 spread 运算符扩展的两个数组的唯一值。然后，新数组被转换为一个集合，然后使用扩展运算符转换回一个数组。

我们可以这样称呼它:

```
const result = union([30, 40, 50], [30, 60]);
```

`result`是`[30, 40, 50, 60]`，因为先前数组的值被保留，然后不在累积数组中的后面的值被添加。

# `unionBy`

`unionBy`方法返回一个数组，每个数组中有唯一的项。这与`union`的区别在于`unionBy`允许我们在进行唯一性比较之前传递一个映射值的方法。

我们可以通过以下方式实施:

```
const unionBy = (iteratee, ...arrs) => {
  let unionArr = [];  
  for (const arr of arrs) {
    const mapped = arr.map(iteratee);
    const unionMapped = unionArr.map(iteratee);
    const uniques = mapped.filter(a => !unionMapped.includes(a));
    unionArr = [...unionArr, ...uniques];
  }
  return unionArr;
}
```

在上面的代码中，我们将`iteratee`作为第一个参数，而不是最后一个，这样我们就可以使用 rest 操作符和`arrs`来返回在`iteratee`函数之后传入的数组参数的数组。

然后我们有一个`for...of`循环来遍历条目，在循环内部，我们用`iteratee`函数映射`arr`条目。

此外，我们用相同的函数映射了`unionArr`数组，这样我们就可以使用`includes`方法和`filter`来获得在用`iteratee`转换值后进行比较后还不是`unionArr`的条目。

那么当我们这样称呼它的时候:

```
const result = unionBy(Math.floor, [30.1, 40, 50], [30.2, 60]);
```

我们得到`result`的`[30, 40, 50, 60]`，因为在进行比较之前，我们用`Math.floor`映射了所有数组中的值。

![](img/a412c9a67e32648fa8c614c56890ab35.png)

Photo by [Clay Banks](https://unsplash.com/@claybanks?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# `unionWith`

`unionWith`类似于`union`,除了它接受一个比较器来比较元素并选择第一个唯一的元素。

我们可以如下实现自己的`unionWith`功能:

```
const unionWith = (comparator, ...arrs) => {
  let unionArr = [];
  for (const arr of arrs) {
    const uniques = arr.filter(a => !unionArr.find(u => comparator(a, u)));
    unionArr = [...unionArr, ...uniques];
  }
  return unionArr;
}
```

在上面的代码中，我们用一个`for...of`循环遍历了`arrs`。在循环内部，我们调用了`arr`上的`filter`来返回一个包含不在`unionArr`数组中的值的数组，并将该数组赋给`uniques`常量。

然后，我们扩展现有的`unionArr`数组和`uniques`数组的值，然后将新数组分配给`unionArr`。

最后，我们返回`unionArr`。那么当我们这样称呼它的时候:

```
const result = unionWith((a, b) => Object.is(a, b), [30, 40, 50], [30, 60]);
```

我们得到`result`是`[30, 40, 50, 60]`，因为我们将这些值与`Object.is`进行了比较，以确定它们是否唯一。

# 结论

`union`、`unionBy`和`unionWith`方法可以通过 spread 运算符和使用 array 的`filter`方法来实现。

`takeWhile`可以通过`for...of`循环实现，并在每次迭代中检查给定的谓词。

## 简明英语笔记

你知道我们有四种出版物吗？给他们一个关注来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**此外，我们总是有兴趣帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****