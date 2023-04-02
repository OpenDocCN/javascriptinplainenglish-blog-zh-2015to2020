# JavaScript 最佳实践—对象和无效语法

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-objects-and-invalid-syntax-287630a4d26c?source=collection_archive---------9----------------------->

![](img/ccb083bd2f838c61167f1c4d6b672d83.png)

Photo by [Zosia Korcz](https://unsplash.com/@calanthe?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种问题。

在本文中，我们将研究在编写 JavaScript 代码时应该遵循的一些最佳实践。

# 定义 Setter 时，对象必须包含 Getter

定义 setter 时，对象应该有一个 getter。

没有 getter 的 setter 不是很有用，所以我们应该添加一个。

例如，我们写道:

```
const person = {
  set name (value) {
    this._name = value
  },
  get name () {        
    return this._name
  }
}
```

而不是:

```
const person = {
  set name (value) {
    this._name = value
  }
}
```

# 派生类的构造函数必须调用 super

我们应该在派生类中调用`super`。

例如，我们应该写道:

```
class Cat extends Animal {
  constructor () {
    super()          
    this.legs = 4;
  }
}
```

如果在我们的类中设置属性之前不调用`super`的话，我们会得到一个错误，正如我们在例子中所做的那样。

# 使用数组文字代替数组构造函数

`Array`构造函数只会给我们的数组声明增加额外的复杂性。

它做了两件不同的事情。

如果它传递了一个参数，那么它会返回一个空数组，其中包含数组中的空槽数。

否则，它会返回一个带有我们传入的参数的数组。

因此，我们应该只使用数组文字。

例如，我们写道:

```
const nums = [1, 2, 3];
```

而不是:

```
const nums = new Array(1, 2, 3);
```

# 不要使用参数。被调用方和参数。调用方

我们不应该在`arguments`对象中使用任何属性，包括`callee`和`caller`属性。

它们使代码优化变得不可能。此外，它们被弃用，并且在严格模式下被禁止。

因此，我们永远不应该使用它们。

# 不要修改类声明的变量

如果我们有一个类，那么我们就不应该把它分配给别的东西。

这将避免很多混乱。

例如，我们不应该有这样的代码:

```
class Dog {}
Dog = 'woof';
```

这太令人困惑了。

# 避免修改使用常量声明的变量

用新值重新分配`const`变量会给我们一个错误。

因此，我们不应该这样做。

例如，我们不应该:

```
const score = 100;
score = 125;
```

相反，我们可以删除第二行，或者创建一个新的变量，并为其赋值。

# 条件语句中没有常量表达式

常量表达式在条件语句中不是很有用。

这是因为它们总是对同一事物求值，所以有条件语句是没有用的。

例如，我们不应该有这样的代码:

```
if (false) {
  // ...
}
```

块内的代码从未运行过，因为我们有`false`作为条件。

但是，我们可以将它们循环使用:

```
while (true) {
  // ...
}
```

因为我们可能想要无限循环。

# 正则表达式中没有控制字符

regexes 中不应该有控制字符。

它们是不可见的，并且很少在 JavaScript 字符串中使用。

因此，将它们包括在内可能是一个错误。

所以我们不应该有这样的代码:

```
const re = /\x1f/;
```

# 无调试器语句

`debugger`语句用于在我们的代码中添加断点，以便我们可以检查变量。

然而，我们应该记得在使用完它们后删除它们，这样我们的应用在生产时就不会暂停。

# 变量上没有删除运算符

`delete`操作符用于删除属性。

因此，它们不应该用在变量上。

所以我们不应该写:

```
let foo;
delete foo;
```

# 函数签名中没有重复的参数

我们不应该在函数签名中有重复的参数。

这是无效的语法，会给我们带来意想不到的结果。

例如，我们不应该写:

```
function sum (a, a, b) {  
  // ...
}
```

相反，我们写道:

```
function sum (a, b, c) {  
  // ...
}
```

# 类成员中没有重复的名称

我们不应该有两个同名的班级成员。

那会产生意想不到的结果。

例如，我们不应该:

```
class Cat {
  meow() {}
  meow() {}
}
```

我们应该给它们取不同的名字。

![](img/c7b88a8cfd42e6ef9fe8413c3aaa4e6a.png)

Photo by [William Moreland](https://unsplash.com/@relentlessjpg?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们不应该在 JavaScript 代码中以无效的方式使用操作符。

此外，我们不应该在不该出现的地方出现重复的表达式。

任何多余的字符和`debugger` 语句都应该从我们的代码中删除。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**