# 现代 JavaScript 的精华—默认参数

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-default-parameters-a40d7d2691c1?source=collection_archive---------15----------------------->

![](img/82ee2ce36bcf02cb2bba21fb9722194b.png)

Photo by [Sharad Bhat](https://unsplash.com/@sharadmbhat?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究如何使用参数。

# 默认参数值

从 ES6 开始，我们可以在 JavaScript 函数中设置默认参数值。

如果参数值为`undefined`，则设置默认值。

例如，如果我们有一个函数:

```
function foo(x, y = 0) {
  return [x, y];
}
```

如果我们写为`foo(1, 3);`，那么我们得到`[1, 3]`。

如果我们把它写成`foo(1);`，那么我们就得到了`[1, 0]`。

如果我们通过写`foo();`来调用它，那么我们得到`[undefined, 0]`。

# 休息参数

Rest 参数让我们以数组的形式获取剩余的参数。

例如，e 如果我们有:

```
function bar(pattern, ...args) {
  return {
    pattern,
    args
  };
}
```

如果我们通过写`format(3, 2, 1);`来调用它，我们得到:

```
{ pattern: 3, args: [2, 1] }
```

# 解构参数

我们可以将对象参数析构为变量。

例如，我们可以写:

```
function baz({
  a = 0,
  b = -1,
  c = 1
} = {}) {
  console.log(a, b, c);
  //...
}
```

我们的`baz`函数接受一个具有属性`a`、`b`和`c`的对象。

我们将它们的默认值分别设置为 0、-1 和 1。

如果参数没有传入，那么我们将它设置为一个对象，该对象将设置这些默认值。

所以如果我们写:

```
baz({
  a: 10,
  b: 30,
  c: 2
});
```

那么`a`是 10，`b`是 30，`c`是 2。

如果我们写:

```
baz({
  a: 3
});
```

那么`a`为 3，`b`为-1，`c`为 1。

如果我们写下:

```
baz({});
```

或者

```
baz();
```

那么`a`为 0，`b`为-1，`c`为 1。

每当缺少任何值时，它将被替换为默认值。

# 传播算子

spread 操作符允许我们将可迭代对象扩展到函数调用的参数中。

例如:

```
Math.max(2, 3, 4, 6)
```

与以下内容相同:

```
Math.max(...[2, 3, 4, 6])
```

我们还可以传播一些论点:

```
Math.max(2, ...[3, 4], 6)
```

在数组中，我们可以使用 spread 运算符来转换数组元素中的可迭代值。

例如，我们可以写:

```
[1, ...[2, 3], 4]
```

并获得:

```
[1, 2, 3, 4]
```

# 参数默认值

我们可以为参数指定默认值。

例如，我们可以写:

```
function foo(x, y = 0) {
  return [x, y];
}
```

如果我们在没有第二个参数的情况下调用它，那么默认值将被设置为`y`的值。

如果我们调用`foo(1)`，那么我们得到`[1, 0]`。

如果我们调用`foo()`，我们得到`[undefined, 0]`。

默认值仅在需要时计算

我们可以从这个例子中看出这一点:

```
const log = console.log;function foo(x = log('x'), y = log('y')) {
  return [x, y];
}
```

如果我们调用`foo(1)`，那么我们得到`'x'`。

如果我们调用`foo()`，那么我们会记录`'x'`和`'y'`。

![](img/5a9929848649294f2f61eb99d5ca5463.png)

Photo by [Matt Hardy](https://unsplash.com/@matthardy?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以给函数添加默认的参数值，这样当它们未被定义时，它们就被赋值为它们的值。

## **简单英语的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**