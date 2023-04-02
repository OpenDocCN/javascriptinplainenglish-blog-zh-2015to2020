# 现代 JavaScript 的精华——数组填充、连接和扩展

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-array-fill-concat-and-spread-3b634d6d3b18?source=collection_archive---------20----------------------->

![](img/967cb82d0da025eacf3ec7788ca2b704.png)

Photo by [Matt Hoffman](https://unsplash.com/@__matthoffman__?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究各种数组操作。

# 创建用值填充的数组

我们可以用`Array.prototype.fill`方法创建一个填充了值的数组。

它用固定值替换包括孔在内的所有数组元素。

例如，我们可以写:

```
const arr = new Array(3).fill('foo');
```

那么`arr`就是`[“foo”, “foo”, “foo”]`。

`new Array(3)`创建一个有 3 个孔的数组，`fill`用`'foo'`替换每个孔。

我们可以通过用一个空数组调用`keys`方法来用升序数字填充数组。

例如，我们可以写:

```
const arr = [...new Array(10).keys()]
```

那么`arr`就是`[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]`。

我们也可以用一些计算值填充数组。

为此，我们可以使用`Array.from`方法。

例如，我们写道:

```
const arr = Array.from(new Array(10), (x, i) => i ** 2)
```

然后我们返回一个包含前 10 个平方数的数组。

所以`arr`就是`[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]`。

为了只用`undefined`填充一个数组，我们可以扩展一个用`Array`构造函数创建的数组。

例如，我们可以写:

```
const arr = [...new Array(10)];
```

那么`arr`就是:

```
[undefined, undefined, undefined, undefined, undefined, undefined, undefined, undefined, undefined, undefined]
```

# 配置由`concat()`展开的对象

我们可以通过`Array.prototype.concat`方法配置哪些对象是可扩展的。

为此，我们可以覆盖`Symbol.isConcatSpreadable`值。

默认情况下，`Array.prototype.concat`方法将一个数组扩展到它所调用的数组中。

例如，我们可以这样使用它:

```
const arr = [3, 4, 5];
const merged = [1, 2].concat(arr, 6);
```

那么`merged`就是`[1, 2, 3, 4, 5, 6]`。

我们将一个数组或一个值传入方法，它将被传入被调用的数组并被返回。

为了改变连接是如何完成的，我们可以定义我们自己的`Symbol.isConcatSpreadable`值来让改变这个行为。

我们可以写:

```
const arr = [3, 4, 5];
arr[Symbol.isConcatSpreadable] = false;const merged = [1, 2].concat(arr, 6);
```

那么`merged`就是:

```
[
  1,
  2,
  [
    3,
    4,
    5
  ],
  6
]
```

# 非数组无分布

非数组值不会扩散到调用`concat`的数组中。

然而，我们可以用`Symbol.isConcatSoreadabke`值来改变这种行为。

例如，我们可以将一个类似数组的对象扩展到我们用`concat`返回的数组中，方法是:

```
const arrayLike = {
  length: 2,
  0: 'c',
  1: 'd'
};
arrayLike[Symbol.isConcatSpreadable] = true;const merged = Array.prototype.concat.call(['a', 'b'], arrayLike, 'e', 'f');
```

我们将`arrayLike[Symbol.isConcatSpreadable]`设置为`true`，这样就可以将带有整数索引的属性扩展到`Array.prototype.concat`返回的数组中。

因此，我们得到的`merged`的值是:

```
[
  "a",
  "b",
  "c",
  "d",
  "e",
  "f"
]
```

# 检测阵列

`Array.prototype.concat`探测阵列的方式与`Array.isArray`相同。

`Array.prototype`是否在原型链中并不重要。

这确保了用于创建`Array`子类的各种黑客仍然可以使用数组检查。

![](img/6625fe72de74b8e1f079b6baee9f26d9.png)

Photo by [freestocks](https://unsplash.com/@freestocks?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以填充和组合数组来操作数组。

`Symbol.isConcatSpreadable`属性允许我们设置一个数组是否可以扩展到另一个数组中。

喜欢这篇文章吗？如果是这样的话，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获得更多类似的内容吧！**