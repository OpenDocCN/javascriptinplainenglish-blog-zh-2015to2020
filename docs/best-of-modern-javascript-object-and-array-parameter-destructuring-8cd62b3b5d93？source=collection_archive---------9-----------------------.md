# 现代 JavaScript 的精华—对象和数组参数析构

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-object-and-array-parameter-destructuring-8cd62b3b5d93?source=collection_archive---------9----------------------->

![](img/9eeb6f82a81ad83fe365729be10a11b7.png)

Photo by [Pierre Bamin](https://unsplash.com/@bamin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究如何处理对象和数组参数的析构。

# 对象和名称参数

我们可以将对象作为参数传入，并将它们析构为变量。

这样，我们可以让一个对象参数有多个属性，我们可以把它们变成变量。

现在我们不需要在函数中有很多参数。

例如，我们可以写:

```
function foo({
  a = 1,
  b = 2,
  c = 3
}) {
  console.log(a, b, c)
}
```

然后我们有一个带有属性`a`、`b`和`c`的对象参数。

我们将它们的默认值分别设置为 1、2 和 3。

这样，我们可以传入一个具有这些属性的对象，它们将被析构。

否则，我们设置默认值。

例如，我们可以写:

```
foo({
  a: 2,
});
```

那么`a`和`b`是 2 而`c`是 3。

`a`被传入，但`b`和`c`被设置为默认值。

这比我们在 ES5 或更早版本中拥有的要短得多:

```
function foo(props) {
  props = props || {};
  var a = props.a || 0;
  var b = props.b || -1;
  var c = props.c || 1;
  console.log(a, b, c)
}
```

我们有一个对象参数`props`。

如果它是 falsy，那么我们把它设置为一个对象。

然后我们把它的属性赋给变量。

如果它们是假的，我们就指定默认值，而不是只有当它们是`undefined`时才指定。

正如我们所看到的，这要长得多，我们可能不想返回所有 falsy 值的默认值。

# 解构数组

我们可以在参数中析构数组。

例如，我们可以写:

```
const arr = [
  ['foo', 3],
  ['bar', 19]
];
arr.forEach(([word, count]) => {
  console.log(word, count);
});
```

然后我们有了以数组作为条目的`arr`数组。

我们用数组析构了回调，然后我们可以使用嵌套的条目作为变量。

此外，我们可以通过将它们转换成数组并调用`map`方法来使用它们来转换地图。

我们可以写:

```
const map = new Map([
  [1, 'a'],
  [2, 'b'],
  [3, 'c'],
]);const newMap = new Map(
  [...map]
  .map(([k, v]) => [k * 2, v])
);
```

我们有一张地图，上面有阵列。

然后，我们通过将现有地图扩展到一个数组来创建一个新地图。

然后我们调用`map`来返回新条目。

spread 操作符将把它转换成一个数组，其中的条目是每个条目的键和值的数组。

因此，我们可以以同样的方式使用析构赋值。

我们可以用一系列的承诺做同样的事情。

例如，我们可以写:

```
const promises = [
  Promise.resolve(1),
  Promise.resolve(2),
  Promise.resolve(3),
];Promise.all(promises)
  .then(([foo, bar, baz]) => {
    console.log(foo, bar, baz);
  });
```

我们在`then`中解构了数组参数。

然后我们在控制台日志中得到被析构的变量。

它们具有所有已解析的值。

![](img/4bf7e56531dda1c3bfe00da1a8debced.png)

Photo by [Mike Meyers](https://unsplash.com/@mike_meyers?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以析构对象和数组参数，以将参数中的属性和数组条目赋给变量。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果是这样，通过 [**订阅解码得到更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**