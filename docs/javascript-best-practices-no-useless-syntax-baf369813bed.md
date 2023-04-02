# JavaScript 最佳实践—没有无用的语法

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-no-useless-syntax-baf369813bed?source=collection_archive---------11----------------------->

![](img/25353fbee3eb633c83b0b8c1d4ab2e9c.png)

Photo by [Toa Heftiba](https://unsplash.com/@heftiba?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了使代码易于阅读和维护，我们应该遵循一些最佳实践。

在这篇文章中，我们将看看我们应该遵循的一些最佳实践，以使每个人的生活更轻松。

# 没有承诺就没有等待

我们应该只在承诺的时候使用`await`。

如果我们将`await`与其他任何东西一起使用，这肯定是一个错误。

例如，不写:

```
async function foo(){
  return 10;
}
```

我们写道:

```
async function foo(){
  const val = await aPromise;
}
```

# 没有逗号运算符

我们不应该使用逗号运算符。

它所做的只是返回列表中的最后一项。

所以与其写"

```
switch (foo) {
  case 1, 2, 3:
    return true;
  case 4, 5:
    return false;
}
```

我们写道:

```
switch (foo) {
  case 3:
    return true;
  case 5:
    return false;
}
```

# 对 if、for、do 或 while 语句使用大括号

我们应该在这些语句中使用花括号，这样我们就可以知道块在哪里开始或结束。

而不是写:

```
if (foo === 'baz')
  foo = 10;
```

我们写道:

```
if (foo === 'baz') {
  foo = 10;
}
```

这样，如果我们在它下面有任何东西，我们就不会误认为它在块内。

# for-in 应该使用 if 语句进行过滤

如果我们写一个 for-in 循环，我们应该用`hasOwnProperty`过滤掉继承的属性。

例如，不写:

```
for (let key in obj) {
  // do something
}
```

我们写道:

```
for (let key in obj) {
  if (obj.hasOwnProperty(key)) {
    // do something
  }
}
```

# 没有函数构造函数

我们不应该使用`Function`构造函数来创建函数。

代码在一个字符串中，这意味着我们不能分析或优化它。

这也是一个安全风险。

所以与其写:

```
let multiply = new Function('a', 'b', 'return a * b');
```

我们写道:

```
let multiply = (a: number, b: number) => a * b
```

# 标签的使用

标签只能用于`do`、`for`、`while`或`switch`语句。

它们与`break`或`continue`一起用于控制回路。

例如，我们写道:

```
A:
  while (foo) {
    if (bar) {
      continue A;
    }
  }
```

我们用`A`标记这个循环。

然后我们在上面用了`continue`，如果`bar`是`true`就运行。

# 不要使用参数属性

我们不应该使用`arguemnt.callee`来获取调用函数的函数。

这使得优化变得不可能。

在严格模式下也是不允许的。

# 没有等待就没有异步

我们不应该不用`await`而用`async`。

如果没有`await`，这意味着我们没有在函数中使用任何承诺。

那就意味着我们不需要它。

所以与其写:

```
async function f() {
  doSomething();
}
```

我们写道:

```
async function f() {
  await makeRequest();
}
```

其中`makeRequest`是一个返回承诺的函数。

# 条件句中没有赋值

在没有比较或其他布尔表达式的条件中赋值可能是错误的。

所以我们应该检查一下。

例如，如果我们有:"

```
if (foo == bar ){
  //...
}
```

我们应该确保它是有效的。

# 没有重复的超级呼叫

在一个子类的`constructor`中，我们只需要调用`super`一次。

如果我们调用它不止一次，我们会得到一个错误。

例如，不写:

```
class Foo extends Bar {
  constructor() {
    super(name);
    super(name);
  }
}
```

我们写道:

```
class Foo extends Bar {
  constructor() {
    super(name);
  }
}
```

# 没有重复的开关盒

在一个`switch`块中，我们不应该有多个具有相同值的`case`语句。

由于短路，只有第一种情况会运行。

例如，代替书写:

```
switch (bar) {
  case 1:
    return 'foo';
  case 1:
    return 'bar';
  case 2:
    return 'baz';
}
```

我们写道:

```
switch (bar) {
  case 1:
    return 'foo';
  case 2:
    return 'baz';
}
```

# 没有重复变量

我们的代码中不应该有重复的变量。

`var`声明可以有 JavaScript 解释器没有选择的副本。

因此，我们要确保不会出现这样的情况:

```
var a = 1;
var a = 2;
```

我们应该去掉其中一个，或者更好，用`let`或`const`代替`var`。

![](img/2d07136f163e8d06fd93c1fcb6b68bea.png)

Photo by [Marius Masalar](https://unsplash.com/@marius?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们不应该有重复的`var`声明或`case`块。

花括号有助于分隔块。

`await`只能与承诺一起使用。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**