# JavaScript 代码样式最佳实践—函数

> 原文：<https://javascript.plainenglish.io/javascript-code-styling-best-practices-functions-8d0f5176c8f7?source=collection_archive---------9----------------------->

![](img/afa3ab75825592a172fb5ce368658d7c.png)

Photo by [Louis Hansel @shotsoflouis](https://unsplash.com/@louishansel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。很容易写出看起来乱七八糟、难以阅读但却能运行的代码。

在这篇文章中，我们将关注一些问题，比如从赋给变量的函数中删除名字，以及函数声明或表达式的一致使用。

# 没有命名的 F `unction`表达式

如果我们有一个 JavaScript 函数表达式，其中我们将一个函数赋给一个变量、常量或属性，那么我们不需要在关键字`function`后给函数添加一个名称。

我们不需要这个名字，因为在我们把它赋给变量、常量或属性之后，我们不能用这个名字来调用或引用函数。

例如，如果我们有:

```
function Foo() {}
Foo.prototype.bar = function foo() {};const foo = new Foo();
foo.bar();
foo.foo();
```

然后`foo.bar`按照我们的预期运行，因为我们创建了`Foo`实例，它的原型中有`bar`实例方法。

但是，`foo.foo`不起作用，因为我们把函数赋给了`Foo.prototype.bar`，也就是说只能用标识符`bar`调用。

用`foo();`调用它也不起作用，因为我们已经将它赋给了一个属性。

这个名字对于阅读代码的人来说很容易混淆。

因此，我们应该删除名称。就是写的少了，我们也没有一个没用的名字把人搞糊涂。

我们可以改为编写以下内容:

```
function Foo() {}
Foo.prototype.bar = function() {};const foo = new Foo();
foo.bar();
```

这样我们就知道只能在`foo`上调用`bar`，这是一个`Foo`实例。

这也适用于对象。如果我们有以下对象:

```
const obj = {
  foo: function bar() {}
}obj.foo();
obj.bar();
```

然后`obj.foo()`像我们预期的那样运行，但是`obj.bar()`会给我们错误‘未捕获类型错误:obj.bar 不是函数’。再一次，我们应该去掉`function`关键字后面的名字。

同样，如果我们有一个如下所示的数组:

```
const arr = [
  function bar() {}
]
```

然后我们调用如下函数:

```
arr[0]();
```

所以还是那句话，名字没用。

如果我们立即调用如下函数表达式:

```
(function bar() {})();
```

那么函数名也是无用的，因为我们不能从外部调用`bar`。因此，如果我们有如下所示的代码:

```
(function bar() {})();bar();
```

我们得到错误“未捕获的引用错误:`bar`未定义”。这意味着这个名字又一次没用了，它只是占据了不必要的空间。

# 一致地使用 F `unction`声明或表达式

在 JavaScript 中，有两种方法来声明函数。一个是函数声明，我们用`function`关键字写函数，并给它一个名字，而不把它赋给变量。

另一种方法是函数表达式，我们传统的函数被定义，然后我们把它赋给一个变量或常数。

两者的区别在于函数声明被提升了，这样在代码中定义它们之前，它们就可以被引用和调用。

例如，我们可以编写如下函数声明:

```
function foo() {
  console.log('foo');
}
```

那么我们可以这样称呼它:

```
foo();function foo() {
  console.log('foo');
}
```

上面的代码将把`'foo'`记录到控制台，因为函数声明被提升，所以它们可以在脚本中的任何地方被调用。

函数表达式编写如下:

```
var foo = function() {
  console.log('foo');
}
```

在定义它们之前，不能引用或调用它们。因此，如果我们试着这样称呼它们:

```
foo();var foo = function() {
  console.log('foo');
}
```

我们会得到一个“`foo`不是函数”的错误，因为`foo`没有定义。

显然，如果我们用`let`或`const`替换`var`，我们会得到错误，因为任何用`let`或`const`声明的东西都不能被引用或调用，除非首先声明它们。

函数表达式更容易理解，因为没有提升。我们只能在他们定义好之后给他们打电话。

这更好，因为它消除了任何与提升混淆。

因此，如果我们选择一致的风格来定义函数，那么函数表达式就是我们要走的路。

此外，箭头函数只适用于函数表达式，因为没有地方放置箭头函数的名称。我们要么把它们赋给一个变量或常数，要么让它们保持匿名。

为了保持一切一致，最好使用如下函数表达式:

```
const foo = function() {
  console.log('foo');
}const bar = () => {
  console.log('bar');
}foo();
bar();
```

然后我们就不用担心吊装的不一致性了。

![](img/42680f6be321401cdbb345dfac637277.png)

Photo by [Louis Hansel @shotsoflouis](https://unsplash.com/@louishansel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

赋给属性、变量或常量的函数不应该有名字，因为它们没用。

函数表达式更好，因为它更一致，因为我们不能在它们被定义之前引用或调用它们。此外，它们还用于为箭头函数指定名称。

# **简明英语笔记**

你知道我们推出了一个 YouTube 频道吗？我们制作的每个视频都旨在教给你一些新的东西。点击此处 查看我们，并确保订阅该频道😎