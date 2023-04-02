# 删除 JavaScript 数组中的重复对象

> 原文：<https://javascript.plainenglish.io/removing-duplicate-objects-in-a-javascript-array-b05258bcded?source=collection_archive---------10----------------------->

![](img/c2ed61ced0f7816cc4433769a0fd7029.png)

Photo by [Jen Theodore](https://unsplash.com/@jentheodore?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

删除数组中的重复对象可能需要一些努力。

这不像从数组中删除重复的原始值那么简单。

在本文中，我们将研究如何从数组中移除重复的对象。

# 消除数组中的重复值

删除重复值很简单。

我们只是把它转换成一个集合，然后把它传播回一个数组。

例如，我们可以写:

```
const arr = ['foo', 'bar', 'foo'];
const uniqueArr = [...new Set(arr)];
```

然后我们得到`[“foo”, “bar”]`作为`uniqueArr`的值。

这不适用于对象，因为只有引用相同的对象才被认为是相同的。

# 创建没有重复的新数组

删除重复条目的一种方法是创建一个没有重复条目的新数组。

例如，如果我们有以下数组:

```
const persons = [{
    firstName: 'jane',
    lastName: 'snith',
    age: 20
  },
  {
    firstName: 'bob',
    lastName: 'jones',
    age: 33,
  },
  {
    firstName: 'jane',
    lastName: 'snith',
    age: 20
  },
];
```

然后，我们可以用唯一的条目检查数组中每个对象的存在。

如果它不存在，我们把它放入数组中，并赋予唯一的值。

为此，我们可以编写以下代码:

```
const uniquePersons = [];for (const p of persons) {
  if (!contains(uniquePersons, p)) {
    uniquePersons.push(p);
  }
}function contains(arr, person) {
  return arr.find(
    (a) => a.firstName === person.firstName && a.lastName === person.lastName && a.age === person.age);
}
```

我们用`uniquePersons`数组来保存数组条目和来自`persons`数组的唯一条目。

如果该项目存在于`arr`数组中，则`contains`函数返回。

`person`是我们想要检查的来自`persons`的对象。

我们调用`arr.find`，条件是我们要返回来检查`person`的属性以确定存在。

在 for-of 循环中，我们只调用`uniquePersons`上的`push`来添加`persons`中不存在的项目。

最后，`uniquePersons`是:

```
[
  {
    "firstName": "jane",
    "lastName": "snith",
    "age": 20
  },
  {
    "firstName": "bob",
    "lastName": "jones",
    "age": 33
  }
]
```

# `Using filter()`

从数组中删除重复对象的另一种方法是使用`filter`方法来检查该对象是否是该对象的第一个实例。

为此，我们可以写:

```
const uniquePersons = persons.filter(
  (m, index, ms) => getFirstIndex(ms, m) === index);function getFirstIndex(arr, person) {
  return arr.findIndex(
    (a) => a.firstName === person.firstName && a.lastName === person.lastName && a.age === person.age);
}
```

鉴于我们有

```
const persons = [{
    firstName: 'jane',
    lastName: 'snith',
    age: 20
  },
  {
    firstName: 'bob',
    lastName: 'jones',
    age: 33,
  },
  {
    firstName: 'jane',
    lastName: 'snith',
    age: 20
  },
];
```

作为`persons`的值。

我们创建了接受一个`arr`数组和`person`对象的`getFirstIndex`方法。

在函数内部，我们调用`findIndex`来返回数组条目的第一个索引，给定值为`person.firstName`、`person.lastName`和`person.age`。

然后在`filter`回调中，我们可以调用`getFirstIndex`来比较`index`和返回的索引。

如果它们不同，那么我们知道这个对象是重复的，所以它将被丢弃。

最后，我们得到同样的结果。

# 创建一个地图并将其转换回数组

另一种删除条目的方法是从数组创建一个映射，然后将其转换回数组。

例如，我们可以写:

```
const map = new Map();for (const p of persons) {
  map.set(JSON.stringify(p), p);
}const uniquePersons = [...map.values()]
```

假设`person`与其他例子相同。

我们只是用`set`方法将物品添加到地图中。如果键为值，则为对象的字符串版本，非字符串版本为值。

由于字符串可以和`===`进行比较，所以重复的会被覆盖。

然后，我们可以通过使用`map.values()`并使用 spread 操作符来将这些项扩展回数组。

![](img/0f64cab6a759bca7837c585f19c2ee1f.png)

Photo by [Alexander Schimmeck](https://unsplash.com/@alschim?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

删除重复的对象比删除重复的原始值需要更多的工作。

数组方法和映射使这变得更容易。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**