# JavaScript 不好的地方

> 原文：<https://javascript.plainenglish.io/the-bad-parts-of-javascript-fdfa37b8cad3?source=collection_archive---------4----------------------->

![](img/65e3fdbf1c6fb386377fded46ada773f.png)

Photo by [Tadeusz Lakota](https://unsplash.com/@tadekl?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是世界上最流行的编程语言之一。它可以做很多事情，并且有一些领先于许多其他语言的功能。

在本文中，我们将看看 JavaScript 中应该避免的不好的部分。

# 全局变量

全局变量是我们绝对应该避免的。

我们可以使用内置的全局变量，如现有的属性`window`、`Array`、`String`等。

然而，我们不应该修改它们或给它们添加任何东西。

此外，我们不应该定义自己的全局变量。

如果我们将自己的成员添加到现有的全局构造函数中，人们会看到意想不到的事情。

如果我们添加新的全局变量，那么可能会导致意外的赋值和命名冲突。

我们不希望这种情况发生。

当程序变得更大时，这些将成为更大的问题。

我们可以用几种方法声明全局变量。我们可以在顶层使用`var`关键字:

```
var foo = 'bar';
```

或者我们可以在`window`对象中创建新的属性:

```
window.foo = 'bar';
```

如果我们不使用严格模式，我们可以写:

```
foo = 'bar';
```

这应该使学习 JavaScript 更容易，但忘记声明变量是我们应该避免的，而不是让人们无意中声明变量。

# 范围

在引入`let`和`const`之前，JavaScript 没有块范围的变量。

但是，既然有了，就要一直用。

块范围的变量数据是一种已经丢失了很长时间的东西。

但是现在我们有了它，我们应该在任何地方使用它们。

所以我们可以写:

```
{
  let a = 1;
  const b = 2;
  //...
}
```

或者:

```
if (foo) {
  let a = 1;
  const b = 2;
  //...
}
```

或者:

```
switch (foo) {
  case 1: {
    let a = 1;
    const b = 2;
    //...
    break;
  }
  //...
}
```

或者:

```
while (foo) {
  let a = 1;
  const b = 2;
  //...
}
```

并且`a`和`b`将总是仅在块内可用。

没有理由不使用它们。

# 分号插入

JavaScript 将尝试在它认为有意义的地方添加分号。

然而，我们不应该依赖于此。

这会导致严重的错误。

例如，如果我们写:

```
return 
{  status: true };
```

在一个函数中，那么这个函数将返回`undefined`。

JavaScript 将此视为:

```
return;
{  status: true };
```

所以我们应该小心不要让 JavaScript 为我们添加分号。

相反，我们写道:

```
return {  
  status: true 
};
```

# 保留字

JavaScript 中有许多不用的保留字。像`enum`、`transient`、`throws`这样的单词都是保留字，但是它们在语言中没有任何特性。

保留字不能用作变量或参数名。当保留字用于对象文字中的键时，它们必须被加上引号。

它们不能与点符号一起使用。

例如，我们必须写:

```
obj['enum'] = 'foo';
```

或者:

```
const obj = {
  'enum': 'foo'
};
```

# 统一码

JavaScript 字符是 16 位。这意味着它被限制为 65536 个字符。

剩下的几百万个字符表示为一对字符。

这意味着，如果我们需要将它们输出到其他地方，我们可能必须将它们转换成正确的形式。

# 类型 of

`typeof`大多只对原始值有用。

例如，如果我们写:

```
typeof 1
```

然后我们得到`'number'`。

`typeof`的问题在于测试`null`和对象的类型。

例如，`typeof null`返回`'object'`，这是不正确的。

所以我们不能用`typeof`来检查`null`。

相反，我们应该使用`===`操作符来检查`null`:

```
foo === null
```

使用`typeof`的另一个问题是当我们测试对象时。

我们不能用它来区分不同种类的物体或`null`。

例如:

```
typeof []
```

并且:

```
typeof {}
```

双双返回`'object'`。

如果我们有`typeof /a/`，我们可能会得到`'object'`或`'function'`，这取决于浏览器。

因此，`typeof`对于检查`null`或任何其他种类的对象都不是很有用。

![](img/549f5ff00874f9d8f2878b4d864c5dd5.png)

Photo by [Ashley Jurius](https://unsplash.com/@ashleyjurius?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该避免使用最新版本但不应该使用的 JavaScript。

这包括声明全局变量和扩充现有的全局变量。

此外，我们应该坚持使用被限制在块中的块范围的变量。

这消除了变量存在之前就被创建的许多混乱。

`typeof`操作符对于检查对象类型也不是很有用。

## **说白了**

通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达一些爱吧！**