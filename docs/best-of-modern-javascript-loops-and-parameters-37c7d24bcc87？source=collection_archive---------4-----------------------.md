# 现代 JavaScript 的精华——循环和参数

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-loops-and-parameters-37c7d24bcc87?source=collection_archive---------4----------------------->

![](img/d18e4953be3a3c3dd39e94ed769c86ee.png)

Photo by [Eddie Tsy](https://unsplash.com/@eddietsy?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 变量。

# 循环头中的`let`和`const`

我们可以在 for、for-in 和 for-of 循环的循环头中使用`let`和`const`。

使用 for 循环，我们可以编写:

```
const arr = [];
for (let i = 0; i < 3; i++) {
  arr.push(i);
}
```

我们可以写使用`let`使`i`只在块中可用。

这比`var`要好，后者使循环在整个函数内部可用:

```
const arr = [];
for (var i = 0; i < 3; i++) {
  arr.push(i);
}
```

`const`的工作原理和`var`一样，但是我们不能改变`i`的值。

例如，我们不能写:

```
for (const i = 0; i < 3; i++) {
  console.log(i);
}
```

因为我们改变了`i`的值。

我们还可以使用 for-of 循环从循环中获取项目。

例如，我们可以写:

```
const arr = [];
for (let i of [0, 1, 2]) {
  arr.push(i);
}
```

来获取值。

我们也可以使用`const`,因为我们没有改变数组入口变量:

```
const arr = [];
for (const i of [0, 1, 2]) {
  arr.push(i);
}
```

`var`变量将使变量在循环外可用:

```
const arr = [];
for (var i of [0, 1, 2]) {
  arr.push(() => i);
}
```

for-in 循环的工作方式类似于 for-of 循环。

# 作为变量的参数

我们可以在块中写参数 we `let`变量，如果它们不与参数重叠的话。

例如，我们不能写:

```
function func(arg) {
  let arg;
}
```

因为两个`args`具有相同的范围。

我们将得到“未捕获的语法错误:标识符“arg”已经被声明为“错误”。

然而，我们可以写:

```
function func(arg) {
  {
    let arg;
  }
}
```

因为`arg`在挡位。

但是，对于`var`，两者都有效:

```
function func(arg) {
  var arg;
}function func(arg) {
  { 
    var arg;
  }
}
```

它们都不做任何事情，并且它们在参数和块中共享相同的作用域。

如果我们有默认参数，那么它们被当作参数。

例如，我们可以写:

```
function foo(x = 1, y = x) {
  return [x, y];
}
```

由于`x`在`y`之前定义。

但是我们不能写:

```
function bar(x = y, y = 2) {
  return [x, y];
}
```

因为当我们将`y`分配给`x`时，它还没有被定义。

因此，我们将默认参数视为`let`变量。

他们看不到身体的范围。

例如，如果我们有:

```
const foo = 'bar';function bar(func = x => foo) {
  const foo = 'baz';
  console.log(func()); 
}bar()
```

然后我们得到`bar`。

函数参数中的函数从外部取`foo`并返回。

`bar`里面的`foo`有自己的作用域。

![](img/151db1c92916138468349e6d77f6da9e.png)

Photo by [Nick Fewings](https://unsplash.com/@jannerboy62?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以在循环头和函数中使用`let`和`const`。

默认参数也被视为`let`变量。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**