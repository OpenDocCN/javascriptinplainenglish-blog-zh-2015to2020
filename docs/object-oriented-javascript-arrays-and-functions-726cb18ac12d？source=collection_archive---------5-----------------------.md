# 面向对象的 JavaScript——数组和函数

> 原文：<https://javascript.plainenglish.io/object-oriented-javascript-arrays-and-functions-726cb18ac12d?source=collection_archive---------5----------------------->

![](img/78b3b8da970b51ee65c8e3ff6d010b0b.png)

Photo by [Mike Benna](https://unsplash.com/@mbenna?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在这篇文章中，我们将看看数组和函数。

# ES6 阵列方法

ES6 增加了更多有用的数组方法。

他们提供了以前由第三方库如下划线和 Lodash 提供的。

# 数组. from

`Array.from`方法让我们将可迭代的对象和不可迭代的类似数组的对象转换成一个数组。

例如，我们可以用它将从`document.querySelectorAll`返回的节点列表转换成数组。

我们可以写:

```
const divs = document.querySelectorAll('div');
console.log(Array.from(divs));
```

然后我们得到一个数组。

我们也可以将有数字索引和一个`length`属性的对象转换成一个数组。

例如，我们可以写:

```
const obj = {
  0: 'a',
  1: 'b',
  length: 2
};
console.log(Array.from(obj));
```

我们得到了:

```
["a", "b"]
```

# 使用 Array.of 创建数组

我们可以使用`Array.of`方法创建一个新的数组。

例如，我们可以用:

```
let arr = Array.of(1, "2", {
  obj: "3"
})
```

然后我们得到:

```
[
  1,
  "2",
  {
    "obj": "3"
  }
]
```

它接受一个或多个参数，并返回一个包含这些参数的数组。

# ES6 数组.原型方法

数组实例都添加了更多的方法。

它们包括以下方法:"

*   `Array.prototype.entries()`
*   `Array.prototype.values()`
*   `Array.prorotype.keys()`

`entries`返回键值对数组。

`values`返回一个数值数组。

返回一个键数组。

所以我们可以写:

```
let arr = ['a', 'b', 'c']
for (const index of arr.keys()) {
  console.log(index)
}for (const value of arr.values()) {
  console.log(value)
}for (const [index, value] of arr.entries()) {
  console.log(index, value)
}
```

我们用`keys`方法记录索引。

我们用`values`方法记录这些值。

用`entries`方法将`index`和`value`连接起来。

它还添加了`find`和`findIndex`方法，让我们找到匹配给定条件的数组的第一个条目。

`find`返回匹配的条目。

并且`findIndex`返回匹配条目的索引。

例如，我们可以写:

```
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
console.log(numbers.find(n => n > 3));
console.log(numbers.findIndex(n => n > 3));
```

然后我们从控制台日志中分别得到 4 和 3。

# 功能

`Function`构造函数可以用来创建一个函数

例如，我们可以写:

```
const foo = new Function(
  'a, b, c, d',
  'return arguments;'
);
```

第一个参数是形参，第二个是函数体。

使用它不是一个好主意，因为我们用字符串创建函数，这意味着会有安全和性能问题。

函数对象有`constrictor`和`length`属性。

`constructor`属性有创建函数的构造函数，应该是`Function`构造函数。

而`length`属性有函数期望的参数个数。

所以如果我们有:

```
function foo(a, b, c) {
  return true;
}
console.log(foo.length);
console.log(foo.constructor);
```

我们分别得到 3 和`ƒ Function() { [native code] }`。

# 结论

我们可以用`Array.from`和`Array.from`方法创建数组。

此外，我们可以用各种方法遍历数组。

`Function`构造函数不应该用来创建函数。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**