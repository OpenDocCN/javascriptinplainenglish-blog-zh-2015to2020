# JavaScript 最佳实践—生成器和对象属性

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-generators-and-object-properties-9c8a38b426b8?source=collection_archive---------8----------------------->

![](img/ae4f560356b7fe037e1ef06912f3b7be.png)

Photo by [Sebastian Pena Lambarri](https://unsplash.com/@sebaspenalambarri?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。编写可以运行但有错误的代码很容易。

在本文中，我们将研究使用生成器以及定义和使用对象属性时的最佳实践。

# 如果我们想转换到 ES5，就不要使用发电机

生成器代码不能很好地移植到 ES5 代码，所以如果我们打算将构建工件构建到 ES5 代码中，我们可能要三思。

然而，现代 transpilers 不应该是这种情况，因为它们从生成器和异步函数中创建基于自定义闭包的状态机。他们以同样的方式工作。然而，唯一的优点是 transpile 代码更难调试，即使使用源代码映射。

因此，如果我们永远不需要调试 transpiled 代码，那么我们可以继续使用生成器。否则，在我们的代码中使用它们之前，我们应该三思。

# 确保生成器函数签名的间距正确

发生器函数应该适当地间隔开。生成器函数的星号应该紧跟在关键字`function`之后。

例如，我们应该如下定义我们的函数:

```
const foo = function*() {
  yield 1;
}
```

在上面的代码中，我们在关键字`function`后面加了一个星号。星号后面没有空格。

对于函数声明，我们应该编写以下代码:

```
function* foo() {
  yield 1;
}
```

在上面的代码中，我们在星号后面和函数名前面有一个星号。

# 访问属性时使用点标记法

如果对象属性的名称是有效的 JavaScript 标识符，那么我们应该使用点符号来访问对象属性。

它比括号符号短，但作用相同。

例如，代替用括号符号编写以下内容:

```
const obj = {
  foo: 1
}console.log(obj['foo']);
```

我们应该编写以下代码:

```
const obj = {
  foo: 1
}console.log(obj.foo);
```

在第一个例子中，我们使用了更长的括号符号，我们必须通过将一个字符串传入括号来访问`foo`属性。我们必须编写额外的字符来访问`foo`属性。

相反，我们应该写出第二个例子中的内容，即`obj.foo`。

它更短，但功能相同。

# 使用变量访问属性时，使用括号符号`[]`

如果我们想访问一个名字存储在变量中的属性，那么我们应该使用括号符号。

例如，我们可以用下面的代码做到这一点:

```
const obj = {
  foo: 1
}const getProp = (prop) => {
  return obj[prop];
}console.log(getProp('foo'));
```

在上面的代码中，我们有带有`foo`属性的`obj`对象。此外，我们还有`getProp`函数，它接受`prop`参数并返回`obj[prop]`的值。

因为`prop`是一个变量，所以我们必须用括号符号访问属性值，所以没有其他方法可以动态访问属性。

那么在我们例子的最后一行，我们可以如下使用`getProp`:

```
getProp('foo')
```

返回`obj.foo`的值，即 1。

![](img/aeb5d77ffb505e686e53332e42d83f7c.png)

Photo by [William Moreland](https://unsplash.com/@relentlessjpg?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 计算指数时使用指数运算符`**`

指数运算符为我们提供了一种计算指数的更简单的方法。它比从 JavaScript 的第一个版本开始就有的`Math.pow`要短。

从 ES6 开始，幂运算符就可用了。例如，代替用`Math.pow`写下面的内容:

```
const result = Math.pow(2, 5);
```

我们应该写:

```
const result = 2 ** 5;
```

它要短得多，而且我们不再需要调用函数来做取幂运算。

# 结论

如果我们想将代码转换成 ES5 并调试构建的 ES5 代码，那么我们就不应该在代码中使用 JavaScript 生成器。

即使使用源代码地图，调试也很困难。

如果我们在代码中使用了生成器，那么我们应该确保生成器代码的间距适当。`*`的间距应符合一致性和易读性标准。

当访问作为有效 JavaScript 标识符的对象属性时，我们应该使用点符号。否则，我们应该使用括号符号。这包括访问变量和属性名不是有效 JavaScript 标识符的属性。

# **用简单英语写的便条**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页[**plain English . io**](https://plainenglish.io/)找到所有这些内容——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**