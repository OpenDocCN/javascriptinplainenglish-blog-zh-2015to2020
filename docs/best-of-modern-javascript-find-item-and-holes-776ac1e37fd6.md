# 现代 JavaScript 精华—查找项目和漏洞

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-find-item-and-holes-776ac1e37fd6?source=collection_archive---------6----------------------->

![](img/0cc5f04db9846cb795c468a3ba4b9aa5.png)

Photo by [Artem Maltsev](https://unsplash.com/@art_maltsev?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将看看`Array`和 holes 的一些实例方法。

# array . prototype . find 索引

`findIndex`方法让我们返回找到的条目的索引。

它需要一个回调函数来返回我们正在寻找的条件。

第二个参数是我们在回调中使用的`this`的值。

它返回满足给定条件的第一个元素的索引。

如果没有找到，则返回-1。

例如，我们可以写:

```
const index = [2, -1, 6].findIndex(x => x < 0)
```

那么`index`就是 1。

如果我们写下:

```
const index = [2, 1, 6].findIndex(x => x < 0)
```

那么`index`就是-1。

签名是回拨是`predicate(element, index, array)`。

`element`是被迭代的数组。

`index`是数组的索引。

`array`是它所调用的数组。

# 通过`findIndex()`查找`NaN`

有了`findIndex`，我们就可以找到`NaN`，因为我们可以用`Object.is`与`NaN`进行比较。

例如，我们可以写:

```
const index = [2, NaN, 6].findIndex(x => Object.is(x, NaN))
```

`Object.is`假设`NaN`与自身相同，那么我们可以用它来检查`NaN`。

这与`indexOf`不兼容。

# `Array.prototype.copyWithin()`

方法让我们将数组的一部分复制到另一个位置。

它的签名是`Array.prototype.copyWithin(target: number,
start: number, end = this.length)`。

`target`是要复制到的起始索引。

`start`是要复制的块的起始索引。

并且`end`是要从中复制的块的结束索引。

所以如果我们写:

```
const arr = [1, 2, 3, 4, 5, 6];
arr.copyWithin(2, 0, 2);
```

然后我们得到:

```
[1, 2, 1, 2, 5, 6]
```

作为`arr`的新值。

# `Array.prototype.fill()`

`Array.prototype.fill()`是一个让我们用给定的值填充数组的方法。

它的签名是:

```
Array.prototype.fill(value, start=0, end=this.length)
```

`value`是要填充的值。

`start`是数组填充的起始索引。

`end`是要填充到数组中的结束索引。

例如，我们可以写:

```
const arr = ['foo', 'bar', 'baz', 'qux'];
arr.fill(7, 1, 3)
```

则`arr`为`[“foo”, 7, 7, “qux”]`。

# 阵列中的孔

JavaScript 允许在数组中打孔。

数组中没有关联元素的索引是一个洞。

例如，我们可以写:

```
const arr = ['foo', , 'bar']
```

添加一个有洞的数组。

ES6 治疗`undefined`或`null`元素中的洞。

如果我们打电话:

```
const index = [, 'foo'].findIndex(x => x === undefined);
```

`index`为 0。

如果我们写道:

```
const entries = ['foo', , 'bar'].entries();
```

则`entries`为:

```
[
  [
    0,
    "foo"
  ],
  [
    1,
    null
  ],
  [
    2,
    "bar"
  ]
]
```

对待他们的方式有些不一致。

使用`in`操作器:

```
const arr = ['foo', , 'bar'];
console.log(1 in arr);
```

我们用`arr`记录`false`。

![](img/fcc268646f090058b98699737a67cea5.png)

Photo by [Paweł Czerwiński](https://unsplash.com/@pawel_czerwinski?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

JavaScript 允许在数组中打孔。

此外，还有各种方法来查找带有数组的项。

喜欢这篇文章吗？如果是这样的话，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获得更多类似的内容吧！**