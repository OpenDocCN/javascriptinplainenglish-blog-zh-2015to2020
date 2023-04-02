# TypeScript 最佳实践—间距、数组和类成员

> 原文：<https://javascript.plainenglish.io/typescript-best-practices-spacing-arrays-and-class-members-dac5fdabd950?source=collection_archive---------8----------------------->

![](img/f8715937638179dbe1079a1d687789aa.png)

Photo by [Kaizen Nguyễn](https://unsplash.com/@kaizennguyen?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

TypeScript 是一个简单易学的 JavaScript 扩展。编写运行并执行某些操作的程序很容易。然而，很难考虑所有的用例并编写健壮的类型脚本代码。

在本文中，我们将研究使用 TypeScript 编写代码时要遵循的最佳实践，包括保持一致的大括号样式和间距。

此外，我们应该在一个类中只有一个成员有一个给定的名字。

我们也应该避免数组构造函数。

无用的括号也应该删除。

# 我们应该有一致的支架样式

对于需要用括号括起来的东西，我们应该保持一致的括号样式。

例如，如果我们有一个接口，我们可以写:

```
interface Foo {
  name: string;
}
```

这也适用于类、枚举、类型别名和任何其他块。

# 逗号间距一致

参数列表中的逗号间距应该一致。

例如，我们可以写:

```
interface Foo {
  foo(val: string, bar: string): string;
}
```

列表中逗号后面有空格。

# 默认参数应该是最后一个参数

在 JavaScript 中，我们可以将默认参数放在函数签名的任何地方。

因此，为了保持一致，我们应该将它们放在最后，这样我们就不必看定义就知道它们在哪里了。

例如，不写:

```
function f(a = 0, b: number) {}
```

我们写道:

```
function f(a, b: number = 0) {}
```

# 删除函数标识符和它们的调用之间的空格

我们应该删除函数名和左括号之间的空格。

例如，不写:

```
alert ('hi');
```

我们写道:

```
alert('hi');
```

# 在我们的代码中有一致的缩进

两个空格的缩进非常好，因为它们在所有操作系统中都是一样的。

我们可以通过将制表符转换为两个空格来节省打字。

同样，我们应该在操作数之间留一个空格。

例如，不写:

```
if (a) {
 b=c;
}
```

我们写道:

```
if (a) {
  b = c;
}
```

# 变量声明中的初始化

我们可以在变量声明中进行初始化。

例如，我们可以写:

```
let x;
```

或者:

```
let x = 1;
```

它们在我们的代码中都很好。

# 关键词前后的间距

我们应该在大多数关键词后留有空格。

例如，我们可以在`if`、`for`、`while`、`do`、`else`、`switch`、`case`或`new`后面加一个空格。

我们可以写:

```
if (bar) {
    *// ...*
} else {
    *// ...*
}
```

或者:

```
for (const a of arr) {
  //..
}
```

或者:

```
while (foo){
  //...
}
```

或者:

```
do {
  //...
} while (foo)
```

或者:

```
switch (foo){
  case 1: {
    //...
    break;
  }
  //...
}
```

一个空格使我们的代码可读性更好。

# 不要使用数组构造函数

`Array`构造函数不是很有用。

此外，它有两个版本。

如果我们传入 1 个参数，那么我们将得到一个数组，其中包含我们传入构造函数的空条目数。

另一方面，如果我们传入多个参数，那么它会返回一个数组，其中包含我们传入的所有参数。

为了使我们的生活更容易，我们应该使用数组文字。

例如，不写:“

```
const arr = Array(1);
```

或者:

```
const arr = new Array(1);
```

或者:

```
const arr = new Array(1, 2, 3);
```

我们写道:

```
const arr = [1, 2, 3];
```

![](img/9690358d87241746036122c0cbd42d76.png)

Photo by [Omar Lopez](https://unsplash.com/@omarlopez1?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 没有重复的类成员

我们不应该有两个同名的类成员，因为第二个会覆盖第一个。

例如，不写:

```
class Foo {
  bar() { return 1; }
  bar() { return 2; }
}
```

我们写道:

```
class Foo {
  bar() { return 1; }
}
```

或者:

```
class Foo {
  bar() { return 2; }
}
```

# 没有空函数

空函数没用。

因此，我们不应该写它们。

而不是写:

```
functuion foo(){}
```

我们应该移除它。

# 没有无用的括号

有些括号是无用的，而另一些括号对于分隔代码是有用的。

一些无用的括号包括:

```
for (const a in (b)){
  //...
}
```

或者:

```
for (const a of (b)){
  //...
}
```

相反，我们应该写:

```
for (const a in b){
  //...
}
```

或者:

```
for (const a of b){
  //...
}
```

另外，`typeof (a);`中的括号也不是很有用。

相反，我们应该写`typeof a;`

# 结论

我们应该在代码中保持一致的间距。

此外，我们不应该有重复的类成员。

还应该避免数组构造函数。

无用的括号也应该删除。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**