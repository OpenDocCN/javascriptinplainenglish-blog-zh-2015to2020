# 类型脚本枚举介绍

> 原文：<https://javascript.plainenglish.io/introduction-to-typescript-enums-2bf306d0dbb5?source=collection_archive---------2----------------------->

## 我们看看如何在 TypeScript 中定义和使用枚举

![](img/e2ffeb760db4993710b0470b58faffb6.png)

Photo by [Zdeněk Macháček](https://unsplash.com/@zmachacek?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果我们想在 JavaScript 中定义常量，我们可以使用`const`关键字。使用 TypeScript，我们有另一种方法来定义一组常量，称为枚举。枚举让我们定义一个命名常量的列表。对于定义一个可以取几个可能值的实体来说，这很方便。TypeScript 同时提供数字和基于字符串的枚举。

# 数字枚举

TypeScript 具有 JavaScript 中不可用的枚举类型。枚举类型是一种数据类型，它有一组命名值，称为该类型的元素、成员、枚举数或枚举数。它们是标识符，就像语言中的常量一样。在 TypeScript 中，数值枚举有一个与之关联的相应索引。默认情况下，成员从索引 0 开始，但可以更改为从我们喜欢的任何索引开始，后续成员的索引将从该起始编号开始递增。例如，我们可以编写以下代码，在 TypeScript 中定义一个简单的枚举:

```
enum Fruit { Orange, Apple, Grape };
```

我们可以像访问任何其他属性一样通过访问成员来使用枚举。例如，在`Fruit`枚举中，我们可以接受如下代码中的成员:

```
console.log(Fruit.Orange);
console.log(Fruit.Apple);
console.log(Fruit.Grape);
```

那么上面代码中的`console.log`应该会得到 0，因为我们没有为枚举指定起始索引。我们可以用类似下面的代码指定枚举的起始索引:

```
enum Fruit { Orange = 1, Apple, Grape };
console.log(Fruit.Orange);
console.log(Fruit.Apple);
console.log(Fruit.Grape);
```

然后，我们从每个`console.log`语句中按顺序记录以下内容:

```
1
2
3
```

我们可以为每个成员指定相同的索引，但这不是很有用:

```
enum Fruit { Orange = 1, Apple = 1, Grape };
console.log(Fruit.Orange);
console.log(Fruit.Apple);
console.log(Fruit.Grape);
```

然后我们得到:

```
1
1
2
```

来自`console.log`。正如我们所看到的，我们指定了索引，但是我们想改变它。我们甚至可以有负指数:

```
enum Fruit { Orange = -1, Apple, Grape };
console.log(Fruit.Orange);
console.log(Fruit.Apple);
console.log(Fruit.Grape);
```

然后我们得到:

```
-1
0
1
```

从`console.log`开始。要通过索引获取枚举成员，我们可以像通过索引访问数组条目一样使用括号符号。例如，我们可以编写以下代码:

```
enum Fruit { Orange, Apple, Grape };
console.log(Fruit[0]);
console.log(Fruit[1]);
console.log(Fruit[2]);
```

然后我们得到:

```
Orange
Apple
Grape
```

数值枚举可以将计算值分配给其成员。例如，我们可以编写一个函数来获取每个枚举成员的值，如下面的代码所示:

```
const getValue = () => 2;enum Fruit {
  Orange = getValue(),
  Apple = getValue(),
  Grape = getValue()
};
```

注意，我们为每个成员分配了一个返回值。如果我们不像下面的代码那样对它们都这样做:

```
const getValue = () => 2;enum Fruit {
  Orange = getValue(),
  Apple = getValue(),
  Grape
};
```

那么 TypeScript 编译器不会编译代码，并会给出一个错误“枚举成员必须有初始值设定项。(1061)".我们可以在一个枚举中混合常量和计算值，因此我们可以编写如下代码:

```
const getValue = () => 2;enum Fruit {
  Orange = getValue(),
  Apple = 3,
  Grape = getValue()
};
```

# 字符串枚举

TypeScript 枚举成员也可以有字符串值。我们可以将每个成员的值设置为字符串，方法是将字符串分配给它们，如下面的代码所示:

```
enum Fruit {
  Orange = 'Orange',
  Apple = 'Apple',
  Grape = 'Grape'
};
```

然而，与数值枚举不同，我们不能给它们赋值。例如，如果我们有以下内容:

```
const getValue = () => 'Orange';enum Fruit {
  Orange = getValue(),
  Apple = 'Apple',
  Grape = 'Grape'
};
```

然后，我们会得到 TypeScript 编译器错误消息“在具有字符串值成员的枚举中不允许计算值。(2553)"因为字符串值枚举不允许计算值。字符串枚举不像数值枚举那样具有自动递增的行为，因为它们没有数值，但是枚举成员的值要清楚得多，因为每个值都是有意义的值，任何阅读它的人都清楚。

在单个枚举中，我们可以让一些成员具有数值，而让其他成员具有字符串值，如下面的代码所示:

```
enum Fruit {
  Orange = 2,
  Apple = 'Apple',
  Grape = 'Grape'
};
```

然而，这比对所有枚举成员使用单一类型的值更令人困惑，因此不建议对一个枚举的不同成员使用混合值类型。

# 计算成员和常量成员

每个枚举成员都有一个与之关联的值，该值可以是常量，也可以是计算值。如果枚举中的第一个成员没有被显式赋值，则该枚举成员是常量，这意味着默认情况下它被赋值为 0。如果没有为它分配显式值，并且前面的枚举成员是一个数值常量，也可以将其视为常量，这意味着它将具有前面成员的值加 1。例如，如果我们有:

```
enum Fruit { Orange = 1, Apple, Grape };
```

那么`Apple`和`Grape`都是常量成员，因为它们会被自动分别赋值为 2 和 3。如果每个成员都被赋予了字符串值，那么它们也被视为常量。此外，如果枚举引用先前定义的枚举成员，该成员可以来自相同或不同的枚举。分配给常量枚举的任何操作的返回值，如用括号将枚举表达式括起来，对枚举表达式(如`+`、`-`、`~`)进行一元算术或位运算，或者对枚举表达式(如`-`、`*`、`/`、`%`、`<<`、`>>`、`>>>`、`&`、`|`、`^`)进行二元算术或位运算，都被视为常量枚举表达式。

例如，下面的枚举是一个具有常量枚举表达式的枚举:

```
enum Fruit {
  Orange = 1 + 2,
  Apple =  1 + 3,
  Grape = 1 + 4
};
```

表达式是常量，因为它们是从任何变量或函数的返回值计算出来的。每个成员都有根据数值计算的值，而不是分配给变量或从函数返回的数字。

以下也是具有常数成员的枚举的示例:

```
enum Fruit {
  Orange = 1 + 2,
  Apple =  1 + 3,
  Grape = Orange + Apple
};
```

包括最后一个成员在内的所有成员都是常数，因为`Grape`的值是根据常数`Orange`和`Apple`计算出来的。两个操作数都是常量值的按位运算也被视为常量，如下面的代码所示:

```
enum Fruit {
  Orange = 1 | 2,
  Apple =  1 + 3,
  Grape = 'abc'.length
};
```

上面没有描述的任何内容都被认为是计算值。例如，如果我们有:

```
enum Fruit {
  Orange = 1 + 2,
  Apple =  1 + 3,
  Grape = 'abc'.length
};
```

那么`Grape`就是一个计算成员，因为我们分配给`Grape`的表达式不是从任何常量成员中计算出来的，它涉及到从一个对象中获取一个属性，而这个属性不是从一个常量值中计算出来的。

如果我们想在 JavaScript 中定义常量，我们可以使用`const`关键字。使用 TypeScript，我们有另一种方法来定义一组常量，称为枚举。枚举让我们定义一个命名常量的列表。对于定义一个可以取几个可能值的实体来说，这很方便。TypeScript 同时提供数字和基于字符串的枚举。TypeScript 允许枚举成员具有数值和字符串值。它们也可以根据其他枚举成员的值或我们希望分配的任何其他表达式来计算。常数枚举是从作为操作数的实际数值或分配给成员的实际值计算出来的。所有其他值都是计算成员值。