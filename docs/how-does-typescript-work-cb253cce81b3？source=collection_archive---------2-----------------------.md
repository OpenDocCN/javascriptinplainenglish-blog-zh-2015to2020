# TypeScript 是如何工作的？

> 原文：<https://javascript.plainenglish.io/how-does-typescript-work-cb253cce81b3?source=collection_archive---------2----------------------->

![](img/663fff9a8d7a5324e02902df76c624ee.png)

Photo by [Marvin Meyer](https://unsplash.com/@marvelous?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

TypeScript 是 JavaScript 的自然扩展，在许多项目中取代了 JavaScript。

然而，并不是每个人都知道它实际上是如何工作的。

在本文中，我们将看看 TypeScript 如何让 JavaScript 项目变得更好。

# 什么是 TypeScript？

TypeScript 是 JavaScript 语言的超集，它让我们能够生成安全且可预测的代码。

它生成的代码可以在任何 JavaScript 运行时运行。

它为我们提供了 JavaScript 所缺乏的类型检查。

# 我们应该使用 TypeScript 吗？

TypeScript 不是 JavaScript 项目中所有问题的解决方案。

我们应该知道 TypeScript 有什么用。

TypeScript 通过静态类型专注于开发人员的生产力。

它使得 JavaScript 类型系统更容易使用。

它为我们提供了访问控制关键字，并增强了类构造函数的语法。

我们可以用 JavaScript 或类型脚本专用语法编写类型脚本代码。

所有代码都要经过 TypeScript 编译器，这是为 JavaScript 构建的。

JavaScript 和 TypeScript 的结合为我们提供了两者的灵活组合。

我们可以有选择地应用 TypeScript 特性。

# 生产力功能的限制

编写 TypeScript 主要是编写带有一些额外特性的 JavaScript。

TypeScript 增强了 JavaScript 的类型系统，并像 JavaScript 一样提供了灵活的类型。

如果我们理解了 JavaScript 的类型系统，那么我们就理解了为什么 TypeScript 的类型系统是这样的。

TypeScript 不仅为我们提供了静态类型系统，还提供了更多的动态类型，如联合类型、交集类型、文字类型等等。

# JavaScript 版本特性

旧的 JavaScript 运行时不支持许多普遍支持的现代特性。

因此，我们需要一个类似 TypeScript 编译器的编译器来将代码转换成我们的目标 JavaScript 运行时。

编译器在转换成更兼容的 JavaScript 代码方面做得很好。

# JavaScript 的数据类型

为了理解 TypeScript，我们必须理解 JavaScript 的数据类型。

如果我们不理解 JavaScript 的类型系统，那么我们使用 TypeScript id 的效率不会很高。

# JavaScript 带来的困惑

JavaScript 中数据存储的基础是变量。

我们可以用`let`或者`const`来声明变量。

`let`变量可以重新分配，而`const`变量不能。

# JavaScript 类型

JavaScript 有明确的系统类型。它的规则在整个语言中是一致的。

最基本的 JavaScript 数据类型是原始值和`object`复合类型。

原语有`number`、`string`、`boolean`、`symbol`、`null`、`undefined.`

`object`是复合型。

`number`是用于表示所有数值的数据类型。JavaScript 不区分整数和浮点值。

`string`是一种用于表示文本数据的类型。

`boolean`可以是`true`也可以是`false`

`symbol`表示唯一的常量值，就像对象中的键。

`null`表示不存在的引用。

`undefined`由已定义但尚未赋值的变量使用。

`object`是用于表示复合值的类型，由单个属性和值组成。

# 使用原始数据类型

在 JavaScript 中，我们不明确写出数据类型。

当我们给变量赋值时，它会自己计算出变量的类型。

例如，我们写道:

```
let foo = "bar";
```

声明一个字符串变量。

如果我们需要检查类型，我们写:

```
typeof foo
```

返回变量的类型。

`typeof`是标识值类型并返回`'string'`的运算符。

`typeof null`返回`'object'`，这不是正确的行为，但是它已经在所有运行时和代码中实现了，所以我们不能使用`typeof`来检查`null`。

# 类型强制

JavaScript 在执行某些操作时会对其变量进行数据类型强制。

它产生一致的结果，我们只需要知道它是如何工作的。

如果我们使用`==`操作符来比较对象，那么类型强制将在比较操作完成之前完成。

例如，我们可能有:

```
let applePrice = 1;
let orangePrice = '1';
if (applePrice == orangePrice) {
  //...
}
```

那么在比较之前，两者都将被转换成数字。

当我们写道:

```
let totalPrice = applePrice + orangePrice;
```

那么两者在串联之前都会被转换成字符串，这很可能不是我们想要的。

![](img/56e783eb8d38d6a12782ed6213f0648f.png)

Photo by [Brooke Cagle](https://unsplash.com/@brookecagle?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 避免无意的类型强制

为了使我们的生活更容易，我们应该采取措施避免无意的数据类型强制。

为此，我们可以使用`===`操作符进行比较，并在进行连接之前先显式转换类型。

# 结论

TypeScript 的工作方式是向 JavaScript 添加增强的语法，然后在 TypeScript 编译器完成自己的检查后将其转换为 JavaScript。

它没有改变 JavaScript 的类型系统。相反，它增加了更多的检查。