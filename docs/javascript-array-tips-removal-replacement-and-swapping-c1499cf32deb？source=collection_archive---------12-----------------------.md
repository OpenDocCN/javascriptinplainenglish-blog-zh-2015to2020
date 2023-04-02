# MJavaScript 数组技巧—移除、替换和交换

> 原文：<https://javascript.plainenglish.io/javascript-array-tips-removal-replacement-and-swapping-c1499cf32deb?source=collection_archive---------12----------------------->

![](img/85647f0b9a2a5ff350bf26a91689d061.png)

Photo by [Joshua J. Cotten](https://unsplash.com/@jcotten?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在这篇文章中，我们将看看编写 JavaScript 应用程序时常见问题的一些解决方案。

# 用 JavaScript 初始化数组的长度

我们可以使用`Array`构造函数创建一个给定长度的空数组。

例如，我们可以写:

```
const arr = new Array(4);
```

创建给定大小的新数组。

然而，不需要这样做，因为 JavaScript 数组是动态调整大小的。

我们也可以用一个值填充每个槽。

例如，我们可以写:

```
const arr = [...new Array(6)].map(x => 0);
```

创建一个包含 6 个零的数组。

我们也可以写:

```
const arr = (new Array(6)).fill(0);
```

做同样的事情。

# 将数组元素从一个数组位置移动到另一个位置

要将数组元素从一个位置移动到另一个位置，我们可以使用`splice`方法。

例如，我们可以写:

```
const move = (arr, fromIndex, toIndex) => {
  const element = arr[fromIndex];
  arr.splice(fromIndex, 1);
  arr.splice(toIndex, 0, element);
}
```

`splice`将一个数组项移动到位。

我们用`fromIndex`得到元素。

然后我们用第一个`splice`调用从`arr`中移除该元素。

1 表示我们从`fromIndex`开始从`arr`删除一个项目。

然后，我们通过用`toIndex`和 0 调用`splice`来插入新元素，以在不删除任何内容的情况下插入元素。

`element`是我们要插入到`toIndex`中的值。

# 检查某个数组索引处是否存在值

我们可以检查具有给定索引的数组条目是否为`undefined`来进行检查。

例如，我们可以写:

```
if (typeof array[index] !== 'undefined') {
  //...
}
```

如果存在任何带有给定`index`的`array`条目。

# 将数组转换为对象

要将一个数组转换成一个对象，我们可以使用`Object.assign`方法或 spread 操作符。

例如，我们可以写:

```
const obj = Object.assign({}, ['a', 'b', 'c']);
```

然后我们得到一个对象，索引作为键，条目作为值。

所以`obj`是:

```
{ 0: "a", 1: "b", 2: "c" }
```

我们可以用 spread 操作符做同样的事情。

例如，我们可以写:

```
const obj = { ...['a', 'b', 'c'] };
```

我们得到了同样的结果。

# 将数组拆分成块

我们可以用 for 循环和`slice`方法将数组分割成块。

例如，我们可以写:

```
let tempArray;
const chunk = 10;
for (let i = 0; i < arr.length; i += chunk) {
  tempArray = array.slice(i, i + chunk);
  //...
}
```

我们有一个 for 循环来遍历`arr`的条目，并增加`chunk`，这是块的大小。

然后在循环体中，我们调用`slice`来获取数组的块。

每个组块有 10 个条目。

# 将集合转换为数组

我们可以用`Array.from`方法或 spread 操作符将集合转换成数组。

例如，我们可以写:

```
let array = Array.from(someSet);
```

或者:

```
let array = [...someSet];
```

# 从数组中移除最后一项

我们可以用`splice`方法从数组中移除最后一项。

例如，我们可以写:

```
arr.splice(-1, 1);
```

-1 是最后一个条目的索引。

1 意味着我们从那个索引开始移除一个数组条目。

它会就地移除。

或者，我们可以使用`pop`方法来做同样的事情。

例如，我们可以写:

```
const arr = ['foo', 'bar', 'baz'];
const popped = arr.pop();
```

我们调用`pop`将`'baz'`从`arr`数组中移除。

移除也在原位完成。

我们也可以起诉`slice`做同样的事情。

它的好处是返回一个没有最后一个条目的新数组。

要使用它，我们可以写:

```
const arr = ['foo', 'bar', 'baz'];
const newArr = arr.slice(0, -1);
```

`newArr`是没有`'baz'`的数组。

`slice`的自变量是起始和结束索引。

不包括结束索引。

所以-1 将包括倒数第二个元素。

0 是起始索引。

![](img/1609409877dc0330b93df95e35a81829.png)

Photo by [Vivi Bzk](https://unsplash.com/@vivibzk?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

有许多方法可以删除一个元素。

此外，我们可以创建一个数组，用不同的方式填充数据。

还有一种简单的方法来移动数组元素。

数组可以转换成对象，反之亦然。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**