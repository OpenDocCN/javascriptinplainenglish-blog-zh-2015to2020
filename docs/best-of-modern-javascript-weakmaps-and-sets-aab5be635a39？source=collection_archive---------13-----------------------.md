# 现代 JavaScript 的精华——弱映射和集合

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-weakmaps-and-sets-aab5be635a39?source=collection_archive---------13----------------------->

![](img/65abb8fe9e03435d2a4bf4d8f57b6d90.png)

Photo by [William Bayreuther](https://unsplash.com/@wbayreuther?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究弱映射。

# WeakMap API

武器地图有 4 种方法。

`WeakMap`构造函数接受一个键值对数组。

键必须是对象。

`WeakMap.prototype.get`方法获取键并返回给定键的值。

`WeakMap.prototype.set`将键和值作为参数，并返回带有新条目的 WeakMap。

`WeakMap.prototype.has`将键作为参数，如果具有给定键的项目存在，则返回`true`，否则返回`false`。

`WeakMap.prototype.delete`获取带有要删除项目的键，如果删除了项目，则返回`true`，如果没有找到项目，则返回`false`。

# 一组

ES5 没有固定的数据结构。

替代方法是使用对象的键来存储元素。

或者我们可以将元素存储在一个数组中。

我们通过`indexOf`检查它是否包含一个元素，或者用`filter`等删除重复的元素。

`indexOf`找不到值`NaN`。

ES6 有`Set`构造函数来创建一个集合。

速度很快，处理`NaN`正确。

我们可以通过编写以下内容来创建一个集合:

```
const set = new Set();
set.add('foo');
```

我们用`Set`结构创建一个集合，并调用`add`来添加一个条目。

`has`实例方法检查该项是否存在。

我们可以写:

```
const hasFoo = set.has('foo');
```

`hasFoo`是`true`，因为我们有一个值为`'foo'`的条目。

`delete`实例让我们删除项目。

例如，我们可以写:

```
const deletedFoo = set.delete('foo');
```

它将密钥作为参数。如果项目被删除，它返回`true`。

属性有集合中的项目数。

所以如果我们有:

```
const set = new Set();
set.add('foo');const size = set.size;
```

而`size`是 1。

实例方法让我们从集合中删除所有的条目。

例如，我们可以写:

```
set.clear();
```

# 设置布景

我们可以通过将一个数组传入`Set`构造函数来获取集合。

例如，我们可以写:

```
const set = new Set(['foo', 'bar', 'baz']);
```

我们也可以使用`add`方法来添加项目:

```
const set = new Set().add('foo').add('bar').add('baz');
```

`Set`构造函数有零个或一个参数。

如果没有传入任何参数，则创建一个空集。

如果传入一个参数，那么这个参数必须是一个 iterable 对象。

# 比较集合元素

我们可以用`has`方法来比较集合元素。

该算法的工作原理类似于`===`，但`NaN`等于它本身

例如，如果我们写:

```
const set = new Set([NaN]);
```

然后我们可以检查是否有:

```
consrt hasNaN = set.has(NaN);
```

第二次添加相同的元素没有效果。

例如，我们可以写:

```
const set = new Set();
set.add('bar');
set.add('bar');const size = set.size;
```

在我们添加了两次`'bar'`后，`size`仍然是一个。

两个对象永远不会被认为是相等的。

这种行为是改不了的。

所以如果我们写:

```
const set = new Set();
set.add({});
set.add({});const size = set.size;
```

那么`size`就是 2。

![](img/b930c67f771d218026f8fdf2366b7958.png)

Photo by [T L](https://unsplash.com/@onelast?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

WeakMaps 有一个比地图简单得多的 API。

集合不能有重复的值，但是对象永远不会被认为是相同的，即使它们有相同的内容。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**