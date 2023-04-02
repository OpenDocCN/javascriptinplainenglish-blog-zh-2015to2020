# 有用的 JavaScript 技巧—地图、复选框和存在

> 原文：<https://javascript.plainenglish.io/useful-javascript-tips-maps-checkboxes-and-exisrence-4a0025cfc6c0?source=collection_archive---------12----------------------->

![](img/58df9752e7bd121586f55d03d0179d43.png)

Photo by [Annie Spratt](https://unsplash.com/@anniespratt?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将了解一些应该遵循的技巧，以便更快更好地编写 JavaScript 代码。

# 地图

映射对于存储键值对非常有用。

它是使用`Map`构造函数创建的。

为了创建它，我们写道:

```
const map = new Map();
```

# 向地图添加项目

我们可以通过使用`set`方法向一个`Map`添加项目。

例如，我们可以写:

```
map.set('color', 'brown');
map.set('age', 2);
```

# 通过键从地图中获取项目

我们可以使用`get`方法通过它的键获得一个地图条目。

例如，我们可以写:

```
map.get('color');
map.get('age');
```

# 通过键从地图中删除项目

`Map`实例有`delete`方法通过键移除一个项目。

例如，我们可以写:

```
map.delete('color');
```

# 从地图中删除所有项目

我们可以用`clear`方法删除`Map`中的所有条目。

例如，我们写道:

```
map.clear()
```

# 检查地图是否有带有给定关键字的项目

`has`方法让我们检查具有给定键的项目是否在`Map`实例中。

例如，我们可以写:

```
const hasColor = map.has('color');
```

# 查找地图中项目的数量

属性返回一个`Map`中的项目数。

例如，我们写道:

```
const size = map.size;
```

# 创建带有值的地图

我们可以在构造函数中传递一个带有键值对的数组。

例如，我们可以写:

```
const map = new Map([['color', 'white'], ['breed', 'poodle'], ['age', 2]])
```

第一个条目是键，第二个是每个数组条目中的值。

# 迭代映射键

`Map`实例具有`keys`方法。

它返回一个带有映射键的迭代器，所以我们可以遍历它们。

例如，我们可以写:

```
for (const key of map.keys()) {
  console.log(key)
}
```

# 迭代地图值

我们可以用`values`方法遍历这些值。

例如，我们可以写:

```
for (const val of map.values()) {
  console.log(val)
}
```

# 迭代键值对

`Map`实例有`entries`方法来遍历键值对。

例如，我们可以写:

```
for (const [key, val] of map.entries()) {
  console.log(key, val)
}
```

我们也可以写作；

```
for (const [key, val] of map) {
  console.log(key, val)
}
```

# 将地图转换为数组

我们可以将映射键和值分散到一个数组中。

例如，我们可以写:

```
const keys = [...m.keys()]
```

并且:

```
const values = [...m.values()]
```

# WeakMaps

WeakMaps 是一种特殊的地图，其条目可以被垃圾收集。

此外，我们不能迭代 WeakMap 中的 kets 或值。

我们也不能清除武器地图上的所有物品。

它的大小也无从查起。

但是，我们可以用`get`得到一个带有关键字的项目

我们也可以用`set`给武器地图添加一个物品。

我们可以用`has`检查具有给定键的项目。

我们可以用`delete`从 Weakmap 中删除一个物品。

# 检查是否选中了复选框

我们可以通过`checked`属性发现复选框是否被选中。

例如，我们可以写:

```
document.getElementById('selected').checked
```

查看 ID 为`selected`的复选框是否被选中。

# 检查未定义的对象属性

有几种方法可以检查未定义的对象属性。

我们可以直接把一个属性比作`undefined`:

```
obj.prop === undefined
```

同样，我们可以使用`hasOwnProperty`方法:

```
!obj.hasOwnProperty('prop')
```

它使用参数中给定的名称检查`obj`的自身属性。

同样，我们可以使用`typeof`操作符:

```
typeof obj.prop === 'undefined'
```

# 检查 DOM 元素是否存在

我们可以通过 jQuery 轻松做到这一点。

我们可以选择元素并获得返回列表的长度:

```
$(selector).length
```

我们也可以调用`exists`方法:

```
$(selector).exists()
```

此外，我们导致普通 JavaScript 的`querySelectorAll`方法:

```
document.querySelectorAll(selector).length
```

![](img/ad4618db0dafa6fa0936cc88a38f5767.png)

Photo by [Mike Benna](https://unsplash.com/@mbenna?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

映射对于存储键值对非常有用。

此外，我们可以检查一个元素是否用检查值进行了检查。

我们可以通过选择元素并获得返回列表的长度来判断元素是否存在。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**