# 使用 TypeScript —文本类型、对象类型和类型别名

> 原文：<https://javascript.plainenglish.io/using-typescript-literal-types-object-types-and-type-aliases-58cf9ff994f5?source=collection_archive---------2----------------------->

![](img/76b9c8daf79a12c1015a9ce32932d251.png)

Photo by [emy](https://unsplash.com/@grimnoire?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

TypeScript 是 JavaScript 的自然扩展，在许多项目中取代了 JavaScript。

然而，并不是每个人都知道它实际上是如何工作的。

在本文中，我们将了解如何定义和使用文字值类型、对象类型和类型别名。

# 文字值类型

文字值类型是具有一组特定值的数据类型，并且只允许指定的值。

这让我们可以将一组值视为不同的类型。

例如，我们可以写:

```
let val: 1 | 2 = 1;
```

然后我们有可以取值为 1 或 2 的`val`。

如果我们分配了一些我们没有明确允许的东西，那么我们得到:

```
let val: 1 | 2 = 100;
```

然后我们得到错误“类型‘100’不可赋给类型‘1 | 2’”。ts(2322)'

# 函数中的文字值类型

我们可以在函数中使用文字类型。

这让我们可以限制一个参数，只允许我们设置几种值。

例如，我们可以写:

```
const totalPrice = (quantity: 1 | 2, price: number) => {
  return quantity * price;
};
```

那我们只能给`quantity`传 1 或 2。

# 在文字值类型中混合值类型

我们可以将值类型缩小到文字类型。

例如，我们可以写:

```
const randomNum = () => {
  return Math.floor(Math.random() * 3) as 1 | 2 | 3;
};
```

然后我们将`randomNum`返回值的类型从`number`缩小到`1 | 2 | 3`。

`randomNum`只能返回 1、2 或 3。

# 具有文本值类型的重载

我们可以在函数重载中使用文字类型。

例如，我们可以写:

```
function foo(val: 1): "foo";
function foo(val: 2): "bar" | "baz";
function foo(val: number): string {
  switch (val) {
    case 1: {
      return "foo";
    }
    case 2: {
      return "bar";
    }
    default: {
      return "baz";
    }
  }
}
```

由于过载，我们可以采用不同的值。

它可以根据我们传递给`foo`的`val`的值返回值。

# 键入别名

为了避免重复定义同一类型，TypeScript 具有类型别名功能。

我们可以将类型组合赋给一个别名，这样我们就可以在整个代码中使用它们。

例如，我们可以写:

```
type combo = 1 | 2 | 3;
```

`type`操作符让我们用我们想要的名字定义一个类型别名。

在我们的例子中，我们的类型别名是`combo`。

# 目标

JavaScript 对象是属性的集合，可以使用文字语法创建，或者由构造函数或类返回。

对象一旦被创建就可以被修改。

我们可以添加或删除属性，并接收不同类型的值。

为了给对象提供类型特性，TypeScript 让我们指定对象的结构。

# 对象形状类型注释

定义对象形状的一种方法是使用对象形状类型注释。

我们定义对象中的属性和每个属性的类型。

例如，我们可以写:

```
const obj: { foo: number } = { foo: 1 };
```

上面的代码定义了对象`obj`，它的属性`foo`是一个数字。

然后，我们从右侧分配具有这种属性和值的东西。

![](img/c2c626c8837b2870abe536811a6efbd0.png)

Photo by [Thanos Pal](https://unsplash.com/@thanospal?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 形状类型如何匹配

对象必须用给定的数据类型定义形状中的所有属性，以匹配对象形状类型批注。

如果属性结构或数据类型不匹配，我们将得到一个错误。

# 可选属性

我们可以用一个`?`使一个对象类型的属性可选。

例如，我们可以写:

```
const obj: { foo: number; bar?: sting } = { foo: 1 };
```

那么`bar`是可选的，我们不需要给它赋值。

# 结论

我们可以定义文字类型，将参数或变量的值限制为几个值。

同样，我们可以用属性和类型来定义对象类型。

最后，我们可以定义类型别名，这样我们就不必到处重复相同的数据类型注释。

## 简单英语的 JavaScript

你知道我们有四种出版物吗？通过 [**plainenglish.io**](https://plainenglish.io/) 找到他们——通过关注我们的出版物和 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达爱意吧！**