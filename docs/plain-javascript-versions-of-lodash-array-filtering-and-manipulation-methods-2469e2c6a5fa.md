# Lodash 数组过滤和操作方法的普通 JavaScript 版本

> 原文：<https://javascript.plainenglish.io/plain-javascript-versions-of-lodash-array-filtering-and-manipulation-methods-2469e2c6a5fa?source=collection_archive---------9----------------------->

![](img/3c1a1ab9a2c1f9f70eab13bab4e6553b.png)

Photo by [Nathan Dumlao](https://unsplash.com/@nate_dumlao?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Lodash 是一个非常有用的工具库，让我们可以轻松地处理对象和数组。

然而，现在 JavaScript 标准库正在赶上诸如 Lodash 之类的库，我们可以用简单的方法实现许多功能。

我们可以用普通的 JavaScript 轻松实现`pullAllBy`、`pullAllWith`、`pullAt`和`remove`方法。

# `pullAllBy`

`pullAllBy`返回一个数组，在用`iteratee`函数转换后，删除与我们想要删除的项目相匹配的给定值。

我们可以如下实现它:

```
const pullAllBy = (arr, values, iteratee) => arr.filter(a => !values.map(iteratee).includes(iteratee(a)))
```

在上面的代码中，在与`includes`进行比较之前，我们调用给定的`iteratee`函数来映射值。同样在`includes`中，我们也调用`iteratee`来做同样的转换，这样就可以正确的比较它们了。

然后我们可以如下使用我们的`pullAllBy`函数:

```
const result = pullAllBy([1, 2.1, 3], [2.2, 3], Math.floor)
```

我们用`result`得到`[1]`。

# `pullAllWith`

Lodash `pullAllWith`方法使用比较器而不是`iteratee`来比较要排除的值，使用比较器而不是`iteratee`在比较之前转换值。

例如，我们可以如下实现它:

```
const pullAllWith = (arr, values, comparator) => arr.filter(a => values.findIndex((v) => comparator(a, v)) === -1)
```

在上面的代码中，我们使用普通 JavaScript 的`findIndex`方法和一个调用`comparator`来比较值的回调函数。

我们调用`filter`来过滤出包含在`values`数组中的条目。

那么当我们这样称呼它的时候:

```
const result = pullAllWith([1, 2, 3], [2, 3], (a, b) => a === b)
```

我们用`result`得到`[1]`。

# `pullAt`

Lodash `pullAt`方法返回一个数组，该数组包含具有给定索引的项目的元素。

它还会删除那些索引中的元素。

同样，我们可以使用`filter`方法过滤出我们想要删除的项目，如下所示:

```
const pullAt = (arr, indexes) => {
  let removedArr = [];
  const originalLength = arr.length
  for (let i = 0; i < originalLength; i++) {
    if (indexes.includes(i)) {
      removedArr.push(arr[i]);
    }
  } for (let i = originalLength - 1; i >= 0; i--) {
    if (indexes.includes(i)) {
      arr.splice(i, 1);
    }
  }
  return removedArr.flat()
}
```

在上面的代码中，我们使用了一个`for`循环来遍历数组，并在`indexes`数组中具有给定索引的项目上调用`arr`上的`splice`。

然后，我们将移除的项目推入到`removedArr`数组中。

然后我们有第二个循环，它反向循环`arr`，这样在我们调用`splice`之前它不会改变`arr`的索引。

在循环中，我们只需调用`splice`来删除给定索引的项目。

最后，我们调用`flat`来展平数组，因为`splice`返回一个数组。

因此，当我们这样称呼它时:

```
const arr = [1, 2, 3]
const result = pullAt(arr, [1, 2])
```

我们看到`result`是`[2, 3]`，因为我们在数组中指定了要移除的索引`[1, 2]`，而`arr`现在是`[1]`，因为它是唯一没有被移除的索引。

![](img/2efc8e4bc541f51178b76df7ad05afee.png)

Photo by [Tim Mossholder](https://unsplash.com/@timmossholder?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# `remove`

`remove`方法根据给定的条件从数组中移除项目，并返回移除的项目。

我们可以用如下的`for`循环自己实现它:

```
const remove = (arr, predicate) => {
  let removedArr = [];
  const originalLength = arr.length
  for (let i = 0; i < originalLength; i++) {
    if (predicate(arr[i])) {
      removedArr.push(arr[i]);
    }
  }
  for (let i = originalLength - 1; i >= 0; i--) {
    if (predicate(arr[i])) {
      arr.splice(i, 1);
    }
  }
  return removedArr.flat();
}
```

在上面的代码中，我们有两个循环，就像使用`pullAt`方法一样。然而，我们不是检查索引，而是检查一个`predicate`。

像`pullAt`一样，我们必须反向循环数组索引，以避免`splice`在循环`arr`的项目时改变索引。

那么当我们调用`remove`时如下:

```
const arr = [1, 2, 3]
const result = remove(arr, a => a > 1)
```

我们得到`result`是`[2, 3]`和`arr`是`[1]`，因为我们指定了`predicate`是`a => a > 1`，所以任何大于 1 的都从原始数组中移除，其余的返回。

# 结论

`pullAt`和`remove`方法非常相似，除了`pullAt`接受一个索引数组而`remove`接受一个给定条件的回调。

`pullAllBy`和`pullAllWith`通过`filter`方法实现。在进行比较之前，`pullAllBy`也需要用`iteratee`函数映射值。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****