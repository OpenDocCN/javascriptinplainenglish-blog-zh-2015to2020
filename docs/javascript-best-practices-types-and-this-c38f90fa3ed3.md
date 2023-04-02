# JavaScript 最佳实践—类型和这个

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-types-and-this-c38f90fa3ed3?source=collection_archive---------12----------------------->

![](img/7193a540991306838f6136ea15d4eb83.png)

Photo by [Thomas Lipke](https://unsplash.com/@t_lipke?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究一些在编写 JavaScript 代码时应该遵循的最佳实践。

# 当心自动类型转换

我们应该意识到自动数据类型转换。

例如，我们可以给我们想要的变量赋值。

我们可以这样写:

```
let text = "james";
text = 5;
```

在 JavaScript 中很容易改变变量的类型。

但是，我们可以使用类似 TypeScript 和 Flow 的语言扩展来限制变量的数据类型。

# 不要声明不必要的变量

如果我们没有使用一个变量，那么我们应该删除它。

声明的变量越多，占用的空间就越大。

例如，不要写:

```
const topBar = sidebar.querySelector('#topbar');
const paragraph = bar.querySelector('p');
paragraph.textContent = 'bar';
```

我们没有使用`topBar`变量，所以我们不应该删除它。

我们可以只写；

```
bar.querySelector('p').textContent = 'bar';
```

# 使用参数默认值

为了确保参数总是有值，我们应该设置参数的值。

例如，我们可以写:

```
function logNumber(num = 100) {
  console.log(num);
}
```

这比老方法要短得多，老方法是:

```
function logNumber(num) {
  if (num === undefined) {
    num = 25;
  }
  console.log(num);
}
```

使用默认参数，我们不必直接给参数赋值。

# 默认结束开关

如果我们有`switch`语句，我们应该用一个`default`子句结束它们，这样如果没有值匹配，我们就可以做一些事情。

例如，我们写道:

```
switch(type) {
  case 1:
    // something
  case 2:
    // something else
  default:
    // do something
}
```

在`default`子句中，我们可以处理错误或执行一些默认操作。

# eval()是错误的

`eval`绝对不好。

从字符串运行代码有很大的安全风险。

而且代码无法优化，很难调试。

# 不使用新对象()

`Object`构造者并没有给我们带来多少好处。

只是时间更长了。

我们可以用`{}`代替`new Object()`。

同样的建议也适用于原始值构造函数。

例如，不使用`new String()`，我们写`''`。

我们不使用`new Number()`，而是使用 numver 文字。

我们用`true`或`false`代替`new Boolean()`。

我们不使用`Array`构造函数，而是使用`[]`。

然而，用给定数量的空槽创建 artays 仍然很方便。

对于 regex，我们使用 regex 文字，如`/abc/`而不是`new RegExp()`。

我们使用`function(){}`或`() => {}`来创建函数，而不是使用`new Function()`来创建函数。

没有构造函数，代码会更短。

在原始构造函数的情况下，如果我们用`typeof`检查，类型将是数据的实际类型而不是`'object'`。

也稍微快一点。

# 注释我们的代码

我们可以对我们的代码进行注释，以便解释代码中没有描述的东西。

# 当有意义时，使用快捷符号

在许多情况下，我们可以使用快捷键。

例如，不要写:

```
let level;
if (score > 200){
  level = 2;
} else {
  level = 1;
}
```

我们可以写:

```
let level = (score > 200) ? 2 : 1;
```

我们使用三元运算符来检查条件并相应地赋值。

而不是写:

```
let price;
if (discount){
  price = discount;
} else {
  price = 120;
}
```

我们写道:

```
let price = discount || 120;
```

如果`discount`是真实的，那么它将被返回并分配。否则，返回 120 并赋值。

# 每个任务有一个功能，使我们的代码模块化

我们的代码应该被分成更小的部分，这样我们可以更容易地使用它们。

函数应该做一件事，这样我们就不会和它们混淆。

这也使得测试和调试更加容易。

出于同样的目的，我们的代码应该模块化。

我们可以把它们分成模块。

# 了解一下这个

`this`是一个需要理解的重要键盘。根据它们所处的位置，它可以呈现不同的值。

在一个方法的对象里面，`this`就是这个对象。

如果它在一个类中，那么它就是类实例。

如果它在原型函数中，那么它就是构造函数实例。

我们不应该知道这些。

![](img/bbd945e6e51972708cc4e7393e16491b.png)

Photo by [Andre Iv](https://unsplash.com/@ndvn?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该使用像参数默认值和默认`switch`情况这样的特性。

避免无用的构造函数。

我们还应该学习`this`，类型转换等等。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果是这样，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**