# 遍历 JavaScript 数组的许多方法

> 原文：<https://javascript.plainenglish.io/many-ways-to-iterate-through-javascript-arrays-b5b49d2f92b7?source=collection_archive---------5----------------------->

![](img/9c9535fd7912131925e603663862d025.png)

Photo by [Robert Bye](https://unsplash.com/@robertbye?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

使用 JavaScript 有很多方法。例如，有很多方法可以遍历数组的元素。

在本文中，我们将研究几种可以遍历 JavaScript 数组的方法。

# `Array.prototype.forEach`

数组实例的`forEach`方法接受一个回调，该回调接受最多 3 个参数:当前项、索引和原始数组。

索引和原始数组是可选参数。

第二个参数是可选参数，取值为`this`，将在回调中使用。

例如，我们可以如下使用它:

```
const arr = [1, 2, 3];
arr.forEach((a, i, arr) => {
  console.log(a, i, arr)
})
```

在上面的代码中，我们定义了`arr`数组，然后通过使用包含所有 3 个参数的回调函数调用`forEach`来遍历它。

然后我们得到:

```
1 0 "[1,2,3]"
2 1 "[1,2,3]"
3 2 "[1,2,3]"
```

从控制台日志输出。

为了传入在回调中使用的`this`值，我们可以编写以下代码:

```
const arr = [1, 2, 3];
arr.forEach(function(a) {
  console.log(a + this.num);
}, {
  num: 1
})
```

在上面的代码中，我们传入了:

```
{
  num: 1
}
```

作为`forEach`的第 2 个论点。

因此，`this.num`为 1。所以`a + this.num`是`a + 1`，我们得到:

```
2
3
4
```

从控制台日志输出。

注意，一旦调用了`forEach`方法，我们就不能中断它。

# for-in 循环

`for...in`循环适合遍历对象的键以及它的原型的键，沿着原型链一直向上。

它只循环遍历可枚举的属性，并且它们没有任何定义的顺序。

例如，我们可以如下使用它:

```
class Foo {
  constructor() {
    this.a = 1;
  }}
class Bar extends Foo {
  constructor() {
    super();
    this.b = 2;
    this.c = 3;
  }
}const bar = new Bar();
for (const key in bar) {
  console.log(bar);
}
```

在上面的代码中，我们有两个类，带有实例变量`a`的`Foo`类和扩展`Foo`的`Bar`类，后者带有实例变量`b`和`c`。

因此，`Bar`实例会有一个`Foo`实例作为它的原型。

然后，当我们遍历作为`Bar`实例的`bar`的键时，它将记录`Bar`实例及其原型的键。

我们得到了:

```
a
b
c
```

作为控制台日志中记录的值。

![](img/55433c1738e87c29a8d2e5398b497225.png)

Photo by [Oliver Hale](https://unsplash.com/@4themorningshoot?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# for-of 循环

循环让我们可以遍历多种对象。它们包括数组和任何类型的可迭代对象，如映射、集合、`arguments`、DOM 节点列表等。

ES2015 引入了此循环。

任何实现了`Symbol.iterator`方法的东西都可以用`for...of`循环迭代。

与`forEach`方法不同，我们可以编写`break`来打破循环。

例如，我们可以如下使用它:

```
for (const a of [1, 2, 3]) {
  console.log(a);
}
```

然后我们得到:

```
1
2
3
```

从控制台日志输出。

我们还可以迭代其他可迭代的对象，比如集合。例如，我们可以将它与如下集合一起使用:

```
for (const a of new Set([1, 2, 3])) {
  console.log(a);
}
```

然后我们得到同样的结果。

# 常规 for 循环

常规的`for`循环是最古老的循环之一。它有 3 个部分，分别是索引初始化器、运行条件和在每次迭代结束时运行的索引修饰符。

例如，我们可以如下使用它:

```
const arr = [1, 2, 3]
for (let i = 0; i < arr.length; i++) {
  console.log(arr[i]);
}
```

在上面的代码中，我们有了`arr`数组。然后我们创建一个`for`循环来遍历它。

索引初始化器是`let i = 0`，运行条件是`i < arr.length`，所以它一直运行到`i`比`arr`的长度小 1。`i++`是索引修饰符，它在每次迭代时通过将`i`增加 1 来更新`i`。

# 结论

我们可以通过使用`forEach`方法遍历数组，该方法是数组实例的一部分。

用于循环对象关键点的循环是`for...in`循环。

一个更通用的循环是`for...of`循环，它可以循环数组或任何其他类型的可迭代对象。

# **简明英语团队的笔记**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页[**plain English . io**](https://plainenglish.io/)上找到所有这些——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**