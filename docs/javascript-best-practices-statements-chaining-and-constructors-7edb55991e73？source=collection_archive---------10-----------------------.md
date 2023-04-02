# JavaScript 最佳实践—语句、链接和构造函数

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-statements-chaining-and-constructors-7edb55991e73?source=collection_archive---------10----------------------->

![](img/1114c115f5ed75f695dc79b5d755710a.png)

Photo by [Jairo Alzate](https://unsplash.com/@jairoalzate?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了使代码易于阅读和维护，我们应该遵循一些最佳实践。

在这篇文章中，我们将看看我们应该遵循的一些最佳实践，以使每个人的生活更轻松。

# 每行的最大语句数

每行不应该有太多的语句。

例如，我们可以这样写:

```
function foo () { let  bar; let baz; if (condition) { bar = 1; } else { baz = 2; } return true; }
```

相反，我们写道:

```
function foo() {
  let bar;
  let baz;
  if (condition) {
    bar = 1;
  } else {
    baz = 2;
  }
  return true;
}
```

# 要求构造函数名称以大写字母开头

构造函数名称以大写字母开头，以区别于其他标识符。

例如，我们写作；

```
const person = new Person();
```

这是一个被普遍接受的惯例，所以人们会理解它。

# 调用不带参数的构造函数时使用括号

当我们在没有参数的情况下调用构造函数时，我们应该在构造函数中添加括号。

我们可以跳过括号，但我们应该保留它们。

而不是写:

```
const person = new Person;
```

我们写道:

```
const person = new Person();
```

# 变量声明后的空行

我们可以将变量声明组合在一起。

例如，我们可以写:

```
const greet = "hello,",
const name = "james";const foo = 12;
```

我们可以用空行将它们分组，以便于区分。

# `return`语句前的空行

语句前的空行没什么用，所以我们可以跳过它们。

我们可以写:

```
function foo(baz) {
  if (!baz) {
    return;
  }
  return baz;
}
```

并且节省一些空间。

# 方法链中每次调用后换行

如果我们有很长的方法链，我们应该在每次调用后加一个换行符。

例如，我们可以写:

```
$("#p")
  .css("color", "green")
  .slideUp(2000)
  .slideDown(2000);
```

而不是写:

```
$("#p").css("color", "green").slideUp(2000).slideDown(2000);
```

# 不要使用警报进行调试

我们不应该使用`alert`进行调试。

调试有更好的选择，比如`console`和`debugger`。

但是，它们可以用于显示预期的警报。

# `Array`建造者

`Array`构造函数适合创建空数组。

但是，应该避免它能做的其他事情，而应该支持数组文字。

我们可以通过编写以下内容来创建一个空数组:

```
Array(10)
```

然后我们得到一个有 10 个空槽的数组。

然后我们可以用`fill`填充它们:

```
Array(10).fill().map((_, i) => i);
```

我们调用`fill`和`map`用内容填充空数组。

# 按位运算符

由于大多数人不知道 JavaScript 按位操作符，我们可能想要检查它们，这样它们就不会被错误地使用。

`||`和`&&`看起来像`|`和`&`位运算符。

# 呼叫者/被呼叫者的使用

我们不应该使用`arguments.caller`和`arguments.callee`来获取函数的调用者和被调用者。

它不能在严格模式下使用。

# case/default 子句中的词法声明

我们应该使用`case`和`defaukt`块，这样我们就可以在不同的块中用相同的名称声明块范围变量。

例如，不写:

```
switch (foo) {
  case 1:
    let x = 1;
    break;
  case 2:
    const y = 2;
    break;
  case 3:
    function f() {}
    break;
  default:
    class C {}
}
```

我们写道:

```
switch (foo) {
  case 1: {
    let x = 1;
    break;
  }
  case 2: {
    const x = 2;
    break;
  }
  case 3: {
    function f() {}
    break;
  }
  default: {
    class C {}
  }
}
```

我们可以在不同的块中声明`x`,因为它们是块范围的。

# 修改类声明的变量

我们不应该修改类声明的值。

所以我们不应该写:

```
class A { }
A = 0;
```

我们不会得到一个错误，但它会使人迷惑。

# 无法与-0 进行比较

`===`不区分 0 和-0。

所以比较它们不会得到我们期望的结果。

例如，不要写:

```
if (x === -0) {
  doSomething()
}
```

我们要用`Object.is`来比较，它确实区分了+0 和-0:

```
if (Object.is(x, -0)) {
  doSomething();
}
```

![](img/ecb2fcf9e3902d1589598e12f795baa1.png)

Photo by [Dan Gold](https://unsplash.com/@danielcgold?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们不应该在一行中有太多的语句。

`arguments`对象中的任何东西都不应该被使用。

按位运算符很容易被误认为布尔运算符。

使用`Object.is`与-0 进行比较。

应该总是用括号调用构造函数。