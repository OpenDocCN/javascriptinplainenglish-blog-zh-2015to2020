# 现代 JavaScript 的精华—新的数据结构

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-new-data-structures-bb7b722c8fd3?source=collection_archive---------9----------------------->

![](img/43ea14a12682638a53d7ad64c2c70348.png)

Photo by [Siora Photography](https://unsplash.com/@siora18?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究如何控制数组、贴图和集合的可扩展性。

# `Symbol.isConcatSpreadable`

`Symbol.isConcatSpreadable`不是 ES6 标准库中任何对象的一部分。

这种机制纯粹是为浏览器 API 和用户代码而存在的。

这意味着默认情况下`Array`的子类是分散的。

`Array`的子类可以通过将`Symbol.isConcatSpreadable`设置为`fale`来防止实例传播。

该属性可以是实例或原型属性。

如果`Symbol.isConcatSpreadable`为`true`，则其他数组状对象按`concat`展开。

这意味着我们可以为一些类似数组的对象打开扩散。

类型化数组不是分散的，它们没有`concat`实例方法。

# 数组索引的范围

对于数组索引范围，ES6 遵循与 ES5 相同的规则。

长度在 0 和`2 ** 32 — 1`之间。

索引在 0 和`2 ** 32 — 1`之间。

字符串和类型化数组的索引范围更大。

范围的上限是因为`2 ** 53 — 1`是 JavaScript 幸灾乐祸点数可以安全表示的最大整数。

# 地图和集合

地图和集合是 ES6 引入的新数据结构。

地图可以有任意值。它们被存储为键值对。

我们可以使用具有`[key, value]`对的条目的`Array`来设置初始数据。

例如，我们可以写:

```
const map = new Map([
  [1, 'foo'],
  [2, 'bar'],
  [3, 'baz'],
]);
```

我们将一个键值对数组传递给`Map`构造函数来创建我们的映射。

集合是唯一元素的集合。

我们可以用`Set`构造函数创建一个集合。

例如，我们可以写:

```
const arr = [2, 3, 5, 6, 6, 6, 2];
const unique = [...new Set(arr)];
```

然后我们得到`unique`是数组`[2, 3, 5, 6]`。

这对于删除数组中的重复元素很有用。

弱映射是不阻止其键被垃圾收集的映射。

这意味着我们在使用它时将不必担心内存泄漏。

# 地图

用 ES5 或更早的版本没有好的方法来创建键值对对象。

唯一的方法是创建一个对象文字，并将其用作映射。

对于 ES6，我们有了`Map`构造函数。

要创建地图，我们可以写:

```
const map = new Map();
map.set('foo', 'bar');
```

我们用`Map`构造函数创建地图。

然后我们调用`set`方法，将键和值作为参数，用条目添加或更新映射。

如果带有该键的条目已经存在，它将被覆盖。

`get`方法让我们在给定键的情况下获取项目。

所以我们可以写:

```
const foo = map.get('foo');
```

我们得到了`'bar'`。

`has`方法让我们检查具有给定键的条目是否存在。

例如，我们可以写:

```
const hasFoo = map.has('foo');
```

那么`hasFoo`就是`true`。

`delete`方法让我们删除带有给定键的条目，

例如，我们可以写:

```
map.delete('foo')
```

用键`'foo'`删除条目。

![](img/cd723a5ab6bdddf8a7985d2fbc616317.png)

Photo by [Srinivasan Venkataraman](https://unsplash.com/@srinii?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以控制一个对象是否可以用带有`Symbol.isConcatSpreadable`属性的`Array.prototype.concat`方法扩展。

映射和集合是 ES6 引入的有用的数据结构。

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**