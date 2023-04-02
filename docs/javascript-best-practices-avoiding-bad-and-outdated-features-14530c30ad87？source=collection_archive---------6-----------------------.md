# JavaScript 最佳实践——避免不好的和过时的特性

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-avoiding-bad-and-outdated-features-14530c30ad87?source=collection_archive---------6----------------------->

![](img/0ea6a3a0144023ea9afd70766ab685a1.png)

Photo by [Abhishek Sanwa Limbu](https://unsplash.com/@abhishek_sanwa?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种简单易学的编程语言。编写运行并执行某些操作的程序很容易。然而，很难考虑所有的用例并编写健壮的 JavaScript 代码。

在本文中，我们将探讨定义和使用函数的最佳方式。

# 空捕捉块

空的 catch 块什么也不做。至少我们应该用注释来解释为什么这个块是空白的。

所以我们不应该让它空白。

# Switch 语句失败

砖块通常不应该有贯穿物。如果我们添加它们，我们应该说为什么我们有它们，这样就没有人会混淆这是一个错误。

例如，我们应该写:

```
switch (val) {
  case 1:
  case 2:
    foo();
    // fall through
  case 3:
    bar();
    break;
  default:
    baz(val);
}
```

# 默认情况

我们应该有`default`案例。

这样，任何不匹配任何案例的值都会得到处理。

`default`语句必须是最后一个，这样它只在没有匹配时运行。

# 这

`this`应该只在构造函数和方法中使用。

箭头功能不应该有自己的`this`。

`this`绝不应该指代全局对象。

# 平等检查

我们应该总是分别使用`===`或`!==`进行相等和不相等的比较。

否则，在检查比较之前，我们可能会得到我们不想要的数据类型强制。

# 需要强制的例外情况

我们可能想同时使用`==`来比较`null`和`undefined`。

例如，我们可以写:

```
if (value == null) {
  //...
}
```

# 随着

我们不应该使用`with`关键字。当我们在一个`with`块中定义一个变量时，它创建了一个新的作用域并导致混乱。

在 ES5 严格模式下也是不允许的。

# 动态代码评估

我们不应该分别使用`eval`或`Function`构造函数来运行代码或创建新函数。

# 自动分号插入

分号应该由程序员而不是 JavaScript 解释器来添加。

如果我们让解释器添加它，那么它可能在意想不到的地方。

# 非标准特征

不在 W3C 规范第 4 阶段的特性不应该被使用。

此外，在使用我们的代码之前，我们必须考虑浏览器支持什么。

像`WeakMap.clear`这样已经被移除的旧功能不应该被使用。

# 基本类型的包装对象

不要将`new`与`Boolean`、`Number`、`String`、`Symbol`等原始对象包装器一起使用。

不用`new`也要用。

这样，它们返回原始值而不是对象。

例如，我们写道:

```
const foo = Boolean(1);
```

而不是:

```
const foo = new Boolean(1);
```

# 修改内置对象

永远不要修改内置对象。

像`Array`或`String`原型这样的对象不应该添加新方法。

我们也不应该修改这些对象中的方法。

另外，不要给全局对象添加符号。

如果我们想要额外的功能，我们应该编写自己的代码或使用标准的 polyfill。

![](img/552cddc15c2bd15e766aa7756f15044e.png)

Photo by [Roberto Nickson](https://unsplash.com/@rpnickson?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 调用构造函数时省略`()`

我们应该在调用构造函数时添加括号。

例如，我们应该写:

```
new Foo();
```

而不是:

```
new Foo;
```

即使这是允许的，我们也不应该做。

# 结论

JavaScript 中有一些特性并不意味着我们应该使用它。

像`eval`和`Function`构造函数这样过时的特性不应该被使用。

包装对象不应该用来创建原始值。

此外，我们应该在空的 catch 块和`switch` case 失败中有一个注释。