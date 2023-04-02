# 现代 JavaScript 的精华——解构

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-destructuring-805a93ae803a?source=collection_archive---------9----------------------->

![](img/c1447cbdbe112b0c8ce4cd517318751a.png)

Photo by [Johann Siemens](https://unsplash.com/@johannsiemens?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 析构。

# 数组析构

我们可以根据数组的位置将数组分解成变量。

例如，如果我们有:

```
const arr = ['a', 'b'];
const [x, y] = arr;
```

那么`x`就是`'a'`，`y`就是`'b'`。

还会处理返回值。

例如，我们可以写:

```
const [all, areaCode, officeCode, number] =
/^(\d\d\d)-(\d\d\d)-(\d\d\d\d)$/
.exec('555-555-1212');
```

我们将匹配的模式分解成它们自己的变量。

`exec`返回一个数组，其中包含整个字符串和由模式匹配的部分，这样我们就可以给变量赋值。

# 析构可以用在哪里？

析构可以用在很多地方。

例如，它们可以在数组中使用:

```
const [x] = ['a'];
let [x] = ['a'];
var [x] = ['a'];
```

我们将数组条目赋给`x`变量。

我们也可以析构参数。

例如，我们可以写:

```
function f([x]) { 
  //...
}
f(['a']);
```

而`x`就是`'a'`。

此外，我们可以用 for-of 循环进行析构:

```
const arr = ['a', 'b'];
for (const [index, element] of arr.entries()) {
  console.log(index, element);
}
```

我们调用`arr.entries`来获得一个数组，该数组包含一组索引和作为条目的元素。

# 解构的模式

如果我们想破坏一个对象或区域，我们需要源和目标。

右边是源，左边是目标。

例如，我们可以写:

```
const obj = {
  a: [{
    foo: 'bar',
    bar: 'abc'
  }, {}],
  b: true
};
const {
  a: [{
    foo: f
  }]
} = obj;
```

我们用一个带有`foo`属性的对象重构了带有`a`数组的嵌套对象。

`bar`在右侧不存在，所以我们将其设为默认值`'abc'`。

`b`设置为`true`，因为右侧不存在。

我们不需要把所有东西都赋给它自己的变量。

我们可以只挑我们需要的。

例如，我们可以写:

```
const { x } = { x: 1, y: 3 };
```

我们刚刚销毁了`x`。

所以`x`是 1。

我们可以对数组做同样的事情:

```
const [x, y] = ['a', 'b', 'c'];
```

那么`x`就是`'a'`而`y`就是`'b'`。

# 对象析构将值强制传递给对象

我们可以对原始值使用析构。

例如，我们可以写:

```
const {
  length: len
} = 'foo';
```

而`len`是 3，因为它获得了字符串的`length`属性。

我们也可以通过书写来解构数字:

```
const {
  toString: s
} = 123;
```

然后我们得到`Number.prototype`的`toString`方法作为`s`的值。

# 无法对值进行对象析构

在某些情况下，值可能不会被析构。

如果值不能用`ToObject`转换成对象，那么它就不能被析构。

例如，如果我们有:

```
const { prop: x } = undefined; 
const { prop: y } = null;
```

然后我们在两种情况下都得到 TypeError，因为左边的那些属性不存在。

![](img/685b0eedc02b96d84451813529348dd3.png)

Photo by [Todd Quackenbush](https://unsplash.com/@toddquackenbush?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

析构可以用在很多地方。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**