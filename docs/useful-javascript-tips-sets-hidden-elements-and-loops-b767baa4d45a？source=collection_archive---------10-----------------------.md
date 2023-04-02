# 有用的 JavaScript 技巧——集合、隐藏元素和循环

> 原文：<https://javascript.plainenglish.io/useful-javascript-tips-sets-hidden-elements-and-loops-b767baa4d45a?source=collection_archive---------10----------------------->

![](img/e219f8af177934e9c6c12e4fabec3047.png)

Photo by [Travis Grossen](https://unsplash.com/@tgrossen?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将了解一些应该遵循的技巧，以便更快更好地编写 JavaScript 代码。

# 设置

集合是用于存储无重复项的有用数据结构。

它是一个 iterable 对象，是用`Set`构造函数创建的。

# 初始化集合

我们可以用`Set`构造函数创建一个集合。

我们写道:

```
const set = new Set()
```

# 向器械包中添加物品

我们可以通过使用`add`方法向集合中添加项目。

所以，我们可以写:

```
set.add('foo');
set.add('bar');
```

# 检查物品是否在器械包中

集合有一个`has`方法来检查一个项目是否在集合中。

例如，我们可以写:

```
set.has('foo');
set.has('bar');
```

# 从集合中删除项目

我们可以调用`delete`来删除一个已设置的项目。

例如，我们可以写:

```
set.delete('foo');
```

# 确定器械包中物品的数量

属性让我们获得一个集合中的项目数。

我们可以写:

```
set.size
```

# 从集合中移除所有项目

方法从集合中移除所有的项目。

例如，我们可以写:

```
set.clear()
```

# 迭代集合中的项目

我们可以使用 for-of 循环来遍历集合中的项目。

例如，我们可以写:

```
for (const k of set.keys()) {
  console.log(k)
}
```

循环通过该组的键。

同样，我们可以使用`values()`方法来做同样的事情:

```
for (const v of set.values()) {
  console.log(v)
}
```

还有返回迭代器的`entries`方法:

```
const i = set.entries();
```

然后我们可以使用`next`方法来获取序列中的下一个条目:

```
i.next()
```

返回的对象具有`value`或`done`属性。

当序列中有下一项时`done`为`false`，否则为`true`。

`value`有来自集合的值。

# 用值初始化集合

我们可以使用`Set`构造函数创建带有值的集合:

```
const set = new Set([1, 2, 3]);
```

# 转换为数组

我们可以用 spread 运算符将集合转换为数组:

```
const arr = [...s.keys()]
```

或者:

```
const arr = [...s.values()]
```

# WeakSet

WeakSet 是一种特殊的`Set`。

项目永远不会在集合中被垃圾收集。

WeakSet 中的项目可以被垃圾回收。

此外，我们不能迭代 WeakSet 中的项目。

我们无法清除武器集中的所有物品。

我们也无法检查它的大小。

但是，它有添加项目的`add`方法。

`has`检查项目是否存在的方法。

`delete`用于删除一个条目。

# 检查元素是否隐藏

我们可以使用 jQuery 检查一个元素是否被隐藏。

例如，我们可以写:

```
$(element).is(":visible");
```

这是对`display: none`的检查。

此外，我们可以写:

```
$(element).is(":hidden");
```

并检查`visibility: hidden`。

我们可以得到隐藏的所有元素:

```
$(element).is(":hidden");
```

我们可以用`visible`选择器来选择:

```
$('element:visible')
```

# 深度克隆对象

我们可以用`JSON.stringify`和`JSON.parse`方法深度克隆一个对象。

例如，我们可以写:

```
const cloned = JSON.parse(JSON.stringify(object));
```

我们可以用它来克隆对象内部带有函数值的`object`。

`Infinity`将被转换为`null`并且`undefined`值将被删除。

此外，正则表达式文字也被删除。

因此，如果我们没有那些将被转换或丢失的值，我们可以使用它。

我们可以使用 Lodash `cloneDeep`方法轻松克隆对象。

它克隆一个对象中的所有东西。

`Object.assign`spread 语法还将所有可枚举的自身属性的值从一个对象复制到另一个对象。

我们可以写:

```
const cloned = Object.assign({}, obj);
```

或者:

```
const cloned = {...obj };
```

# 对于每个循环

对于 for-of 循环来说，遍历数组中的所有项是最容易的，

`Array`实例也有`forEach`方法。

老式的 for 循环也可以。

我们可以写:

```
for (const a of arr){
  console.log(a);
}
```

或者:

```
arr.forEach(a => {
  console.log(a);
})
```

或者:

```
for (let i = 0; i < arr.length; i++){
  console.log(arr[i]);
}
```

![](img/58e4ff3ea68a24d2f25f1ac644400eb3.png)

Photo by [Josephine Baran](https://unsplash.com/@jma1053?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

集合是一种有用的数据结构，用于存储没有重复的数据。

此外，我们可以用 jQuery 检查隐藏的元素。

有很多方法可以迭代数组。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**