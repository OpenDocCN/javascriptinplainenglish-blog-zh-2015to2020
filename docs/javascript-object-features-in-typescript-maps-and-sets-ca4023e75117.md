# TypeScript 中的 JavaScript 对象功能—映射和集合

> 原文：<https://javascript.plainenglish.io/javascript-object-features-in-typescript-maps-and-sets-ca4023e75117?source=collection_archive---------7----------------------->

![](img/74b1b3d375fadada6f5c6126f3cfddaa.png)

Photo by [Dan Gold](https://unsplash.com/@danielcgold?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

TypeScript 是 JavaScript 的自然扩展，在许多项目中取代了 JavaScript。

然而，并不是每个人都知道它实际上是如何工作的。

在本文中，我们将了解如何使用映射来存储键值对，以及如何使用集合来存储没有重复的项目。

# 用映射存储键值对

我们可以用映射存储键值对。

map 和 object literals 之间的区别在于，我们可以拥有除字符串和符号之外的键。

我们可以用 JavaScript 内置的`Map`构造函数定义一个 map。

例如，我们可以编写以下代码来实现这一点:

```
const data = new Map();
const key = {
  foo: 'bar'
};
data.set("foo", "bar");
data.set(key, 2);
```

正如我们所看到的，我们有一个带有字符串键和对象键的映射。

`set`方法用给定的键存储一个值。

要通过键获取值，我们可以使用`get`方法，将键作为参数。

还有一个`keys`方法来获得一个带有映射键的迭代器。

`values`方法返回地图的值。

`entries`方法返回一个带有映射的键值对的迭代器。

这是地图的默认迭代器。

# 对地图键使用符号

符号可用于`Map`和目标键。

例如，我们可以写:

```
const data = new Map();
const symbol = Symbol();data.set("foo", "bar");
data.set(symbol, 2);
```

创建的每个符号都是不同的，所以我们必须把它存储在一个变量中，这样我们以后就可以用符号键得到值。

这对于非原语键也是一样的。

因此，我们可以有多个名称相同但引用不同的符号。

例如，我们可以写:

```
const symbol1 = Symbol('foo');
const symbol2 = Symbol('foo');
```

他们有相同的名字但是不同。

所以:

```
symbol1 === symbol2
```

会返回`false`。

我们可以用给定的符号键通过写:

```
const value = data.get(symbol);
```

![](img/0612201945e9cbecd6442fcced4713b7.png)

Photo by [Robin Stickel](https://unsplash.com/@robinstickel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 通过索引存储数据

我们可以用一个`Set`实例存储没有重复的数据。

例如，我们可以写:

```
const set = new Set([1, 2, 3]);
```

然后我们得到一个包含这些返回值的集合。

集合不能有重复，所以如果我们写:

```
const set = new Set([1, 2, 3, 1]);
```

我们仍然得到一个包含 1、2 和 3 的集合，因为只保存了某个事物的第一个实例。

集合具有返回集合大小的`size`属性。

集合实例有一些有用的方法，可以从中获取项、添加/移除项以及遍历这些项。

`add`将我们要添加到集合中的值作为参数。

`entries`按照条目添加的顺序返回所有条目的迭代器。

`has`返回`true`是一个设定有指定值的集合。

`forEach`接受在集合的每个项目上运行的回调。

# 结论

我们可以使用映射来存储键值对。与对象不同，它可以存储不是字符串或符号的键。

集合可以用来存储没有重复的数据。

它们都是可迭代的对象，我们可以调用一个方法从它们返回迭代器。