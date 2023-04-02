# 用普通 JavaScript 实现的 Lodash 方法——展平数组和搜索

> 原文：<https://javascript.plainenglish.io/lodash-methods-implemented-with-plain-javascript-flattening-arrays-and-searching-6e40c8f78d5c?source=collection_archive---------8----------------------->

![](img/42f4bb2304de0197864a781daee79d05.png)

Photo by [Darren Nunis](https://unsplash.com/@dnunis?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Lodash 是一个非常有用的工具库，让我们可以轻松地处理对象和数组。

然而，现在 JavaScript 标准库正在赶上诸如 Lodash 之类的库，我们可以用简单的方法实现许多功能。

在本文中，我们将看看如何用`flattenDeep`和`flattenDepth`来展平递归数组，并用`fromPair`将数组键值对转换成对象。

# `flattenDeep`

洛达什的`flattenDeep`递归展平数组。现在 JavaScript 有了一个用于展平数组的`flat`实例方法，我们不再需要这个方法了。

唯一更好的是，它返回一个新数组，而不是修改原来的数组。

例如，我们可以如下使用它来创建我们自己的`flattenDeep`方法；

```
const flattenDeep = arr => arr.flat(Infinity)
```

传入`Infinity`递归变平。

那么我们可以这样称呼它:

```
const flattened = flattenDeep([1, [2],
  [
    [3]
  ], 4, 5
])
```

然后我们得到`flattened`就是`[1, 2, 3, 4, 5]`。更难的方法是我们自己从头开始实施。但是，我们可以使用 spread 运算符使其更短，如下所示:

```
const flattenDeep = arr => {
  let flattened = [];
  for (const a of arr) {
    if (Array.isArray(a)) {
      flattened = [...flattened, ...a];
      flattened = [...flattenDeep(flattened)]
    } else {
      flattened.push(a);
    }
  }
  return flattened;
}
```

在上面的代码中，我们递归地调用了自己的`flattenDeep`函数。只有当条目是数组时，我们才递归调用它。

因此，它们都给我们带来了相同的结果。

# 平坦深度

方法递归地展平一个数组直到给定的深度级别。

正如我们从上面的`flattenDeep`例子中看到的，JavaScript 的内置`flat`方法使用一个参数来指定要展平的深度。

因此，我们可以实现自己的`flattenDepth`功能，如下所示:

```
const flattenDepth = (arr, depth) => arr.flat(depth)
```

我们只是用自己的`depth`调用`flat`。因此，当我们这样称呼它时:

```
const flattened = flattenDepth([1, [2],
  [
    [3]
  ], 4, 5
], 1)
```

我们得到的`flattened`是:

```
[
  1,
  2,
  [
    3
  ],
  4,
  5
]
```

因为我们指定将给定的数组展平一级深度。

如果我们想自己实现`flattenDepth`，我们可以实现类似于我们从头开始实现`flattenDeep`的东西:

```
const flattenDepth = (arr, depth, flattenedDepth = 0) => {
  let flattened = [];
  for (const a of arr) {
    if (Array.isArray(a)) {
      flattened = [...flattened, ...a];
      if (depth < flattenedDepth) {
        flattened = [...flattenDepth(flattened, flattenedDepth + 1)]
      }
    } else {
      flattened.push(a);
    }
  }
  return flattened;
}
```

在上面的代码中，我们有一个额外的`flattenDepth`参数，它被设置为 0，这样我们就可以跟踪被展平的数组的深度。

我们只在

然后我们可以在递归调用`flattenedDepth`时将`flattenedDepth`加 1。

因此，我们得到了与用数组的`flat`方法实现的`flattenDepth`相同的结果。

![](img/8a3bcb835e7b880fb5ada528cf68e9ff.png)

Photo by [🇸🇮 Janko Ferlič](https://unsplash.com/@itfeelslikefilm?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# `fromPairs`

`fromPairs`方法转换返回一个对象，该对象具有一个数组条目，其中键作为第一个条目，值作为第二个条目。

JavaScript 已经有一个`Object.fromEntries`做同样的事情，我们可以如下使用它:

```
const obj = Object.fromEntries([
  ['a', 1],
  ['b', 2]
])
```

那么我们可以得出`obj`对象是:

```
{
  "a": 1,
  "b": 2
}
```

`Object.fromEntries`返回与`fromPairs`相同的结果，所以我们肯定不再需要`fromPairs`了。

# 索引 Of

Lodash `indexOf`方法与普通 JavaScript 的`indexOf`方法相同。Lodash `indexOf`可以使用一个开始索引来搜索一个项目，plain `indexOf`方法也使用这个索引。

例如，我们可以使用普通的`indexOf`方法来实现`indexOf`方法，如下所示:

```
const indexOf = (arr, value, start) => arr.indexOf(value, start)
```

正如我们所看到的，我们只是使用了`indexOf`方法的所有参数。唯一的区别是在`arr`上调用了`indexOf`，这是一个数组实例。

# 结论

我们可以调用我们自己的数组展平函数或者数组实例的`flat`方法，这两个方法都会展平一个数组。

`Object.fromEntries`方法取代了 Lodash 中的`fromPair`方法。

最后，数组实例的`indexOf`方法替换了 Lodash 的`indexOf`方法。

## **简明英语团队的笔记**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**此外，我们总是有兴趣帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****