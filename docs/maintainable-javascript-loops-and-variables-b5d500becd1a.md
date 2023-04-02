# 可维护的 JavaScript——循环和变量

> 原文：<https://javascript.plainenglish.io/maintainable-javascript-loops-and-variables-b5d500becd1a?source=collection_archive---------4----------------------->

![](img/2b3b2a46dcdd3eae1090cb2082b65a69.png)

Photo by [Kevin Oetiker](https://unsplash.com/@kevinoetiker?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果想继续使用代码，创建可维护的 JavaScript 代码很重要。

在本文中，我们将通过查看各种块语句和声明变量来了解创建可维护 JavaScript 代码的基础。

# for-in 循环

for-in 循环用于迭代对象的属性。

该循环遍历每个属性，并在变量中返回属性名，而不是定义控制条件。

例如，我们可以这样使用它:

```
for (const prop in object) {
  console.log(prop, object[prop]);
}
```

`object`是对象，`prop`是属性键。

for-in 循环获取对象本身及其所有原型的所有属性。

所以我们最终可能会得到意想不到的结果。

因此，我们想使用`hasOwnProperty`方法来检查属性是否在方法中。

例如，我们可以写:

```
for (const prop in object) {
  if (object.hasOwnProperty(prop)) {
    console.log(prop, object[prop]);
  }
}
```

有些风格指南，比如道格拉斯·克洛克福特的，需要在 for-in 循环中使用`hasOwnProperty`。

当我们在没有使用`hasOwnProperty`方法的情况下使用 for-in 循环时，ESLint 和 JSHint 都会警告我们。

一个常见的错误是使用 for-in 循环来迭代数组。

例如，我们不应该使用 for-in 循环来遍历数组:

```
const values = [1, 2, 3, 4, 5];for (const i in values) {
  console.log(values[i]);
}
```

这是因为订单没有保证。

这不是它的预期目的。

for-in 循环用于处理对象及其原型的关键点。

# for-of 循环

for-of 循环让我们遍历一个 iterable 对象。

这是我们应该在数组中循环的循环。

例如，我们可以这样使用它:

```
const values = [1, 2, 3, 4, 5];for (const val of values) {
  console.log(val);
}
```

`val`是数组的入口。

我们可以遍历任何可迭代的对象，所以我们可以遍历地图和集合。

例如，我们可以写:

```
const values = new Set([1, 2, 3, 4, 5]);for (const val of values) {
  console.log(val);
}
```

并且:

```
const values = new Map([
  [1, 'foo'],
  [2, 'bar'],
  [3, 'baz']
]);for (const val of values) {
  console.log(val);
}
```

我们可以破坏地图的键:

```
const values = new Map([
  [1, 'foo'],
  [2, 'bar'],
  [3, 'baz']
]);for (const [key, val] of values) {
  console.log(key, val);
}
```

这是很多像 ESLint 和 Airbnb 这样的风格指南推荐的循环。

# 变量声明

变量声明可以用`var`、`let`或`const`关键字来完成。

`var`是声明变量的老办法。

我们应该使用`let`和`const`，因为它们是块范围的，只有在定义后才可用。

`const`变量也不能被重新分配。

例如，不写:

```
function doWork() {
  var result = 20 + value;
  var value = 30;
  return result;
}
```

我们写道:

```
function doWork() {
  const value = 30;
  const result = 20 + value;
  return result;
}
```

我们尽可能多地使用`const`,这样变量就不会被意外地重新分配。

![](img/08560dce37009d73aa74a3d709f92f3e.png)

Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

for-in 循环仅用于遍历对象的键。

for-of 循环用于遍历任何可迭代对象。

另外，`var`不应该再用于声明变量。