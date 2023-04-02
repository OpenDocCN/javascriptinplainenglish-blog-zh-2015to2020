# 现代 JavaScript 的精华— Iterables

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-iterables-e6150406ba3e?source=collection_archive---------10----------------------->

![](img/7825a4b35a0462fd8f4a8eb3dfe92ad5.png)

Photo by [Woldai Wagner](https://unsplash.com/@xoxxai?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 可迭代对象。

# 可迭代值

JavaScript 有各种类型的可迭代对象。

它们包括数组、字符串、映射、集合和节点列表。

# 支持迭代的构造

有各种各样的构造使得使用 iterable 对象更加容易。

其中之一是析构语法。

我们可以将任何可迭代的对象条目析构为变量。

例如，我们可以写:

```
const [a, b] = new Set(['a', 'b', 'c']);
```

然后我们将`'a'`分配给`a`，将`'b'`分配给`b`。

for-of 循环让我们可以轻松地遍历 iterable 对象中的条目。

例如，我们可以写:

```
for (const x of ['foo', 'bar', 'baz']) {
  console.log(x);
}
```

循环遍历数组。

`Array.from`静态方法让我们从可迭代对象中创建一个数组。

例如，我们可以通过编写以下内容将集合转换为数组:

```
const arr = Array.from(new Set(['a', 'b', 'c']));
```

那么`arr`就是`[‘a’, ‘b’, ‘c’]`。

spread 操作符让我们做与`Array.from`相同的事情

我们可以写:

```
const arr = [...new Set(['a', 'b', 'c'])];
```

将集合的项目分散到数组中。

`Map`和`Set`构造函数让我们从数组中创建地图和集合。

`Map`构造函数将一个带有键值数组的数组作为条目，并将它们转换成一个对象中的一组键值对。

例如，我们可以写:

```
const map = new Map([
  ['foo', 'no'],
  ['bar', 'yes']
]);
```

要创建一个集合，我们可以写:

```
const set = new Set(['a', 'b', 'c']);
```

从数组中创建它。

`Promise.all()`让我们并行运行多个承诺。

例如，我们可以写:

```
Promise.all([
  Promise.resolve(1),
  Promise.resolve(2),
]).then(() => {
  //...
});
```

并行运行数组中的两个承诺。

`Promise.race`解析为完成运行的第一个承诺的值。

例如，我们可以这样使用它:

```
Promise.race([
  Promise.resolve(1),
  Promise.resolve(2),
]).then(() => {
  //...
});
```

我们从承诺集中得到第一个值。

关键字`yield*`让我们从另一个生成器函数中运行生成器函数。

所以我们可以通过写来使用它:

```
yield* gen;
```

# 迭代性

任何使用`Symbol.iterator`方法的对象都是可迭代对象。

该方法必须是一个生成器函数。

例如，我们可以通过编写以下代码从数组中获取该方法:

```
const arr = ['foo', 'bar', 'baz'];
const iter = arr[Symbol.iterator]();
```

然后我们可以通过从返回的迭代器中调用`next`方法来逐个获取条目:

```
iter.next()
```

我们得到:

```
{value: "foo", done: false}
```

当我们第一次运行时。

当我们再次运行时:

```
iter.next()
```

我们得到:

```
{value: "bar", done: false}
```

当我们再次运行它时，我们得到:

```
{value: "baz", done: false}
```

最后，当我们最后一次运行它时，我们得到:

```
{value: undefined, done: true}
```

`value`是从 iterable 对象返回的值。而`done`表示是否有任何值要返回。

`next`返回包装在对象中的每个项目。

![](img/771dd301c591ed3aaaff11581aa4da66.png)

Photo by [Charlotte Coneybeer](https://unsplash.com/@she_sees?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

JavaScript 有各种类型的可迭代对象。

它们都有`Symbol.iterator`属性，这是一个按顺序返回下一个值的方法。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**