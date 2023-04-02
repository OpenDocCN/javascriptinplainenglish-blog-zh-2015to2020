# 使用类型脚本—元组和枚举

> 原文：<https://javascript.plainenglish.io/using-typescript-tuples-and-enums-f219c3a6072b?source=collection_archive---------2----------------------->

![](img/300830628eecbca85dce0c992258b424.png)

Photo by [Dan Gold](https://unsplash.com/@danielcgold?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

TypeScript 是 JavaScript 的自然扩展，在许多项目中取代了 JavaScript。

然而，并不是每个人都知道它实际上是如何工作的。

在本文中，我们将看看如何在我们的类型脚本代码中定义和使用元组和枚举。

# 元组

元组是固定长度的数组，每个元素可以有不同的类型。

它是一种特定于 TypeScript 的数据结构，被转换成常规的 JavaScript 数组。

例如，我们可以这样定义它:

```
let person: [string, number] = ["james", 100];
```

元组使用方括号定义，其中每个元素的类型用逗号分隔。

在上面的例子中，我们定义了一个包含 2 个元素的元组，其中第一个元素必须是一个字符串，第二个元素必须是一个数字。

# 处理元组

TypeScript 强制执行我们可以对数组采取的操作。

我们可以将它们与标准的 JavaScript 特性一起使用，因为它们只是用常规数组实现的。

例如，我们可以写:

```
const strings: string[] = person.map(e => e.toString());
```

我们把它上面的`map`叫做常规数组。

元组还可以使用其他数组方法和操作。

# 使用元组类型

元组有一个独特的类型，可以像任何类型一样使用。

这意味着我们可以创建元组数组、带有元组的联合类型以及类型保护来限制元组类型中的值。

例如，我们可以写:

```
let person: [string, number] = ["james", 100];
person.forEach(e => {
  if (typeof e === "number") {
    console.log(`number: ${e}`);
  } else if (typeof e === "string") {
    console.log(`string: ${e}`);
  }
});
```

我们调用了`forEach`方法来遍历条目并显示元组中每个条目的数据类型。

`typeof`操作符让我们检查类型并根据每种类型做一些事情。

# 枚举

枚举 let's 已经创建了一个按名称使用的集合。

它使代码更容易阅读，并确保一致地使用固定的值集。

我们可以如下定义 TypeScript 枚举:

```
enum Fruit {
  APPLE,
  ORANGE,
  GRAPE
}
```

我们使用了`enum`关键字和一堆常量来定义一个枚举。

然后我们可以通过写来使用它:

```
const apple = Fruit.APPLE;
```

默认情况下，枚举值将被映射到一个数字，所以`apple`将是 0，因为`Fruit.APPLE`是 0。

同样地`Fruit.ORANGE`是 1 而`Fruit.GRAPE`是 2。

由于缺省情况下枚举值是 JavaScript `number`值，我们可以给一个数字变量赋值:

```
const apple: number = Fruit.APPLE;
```

而且 TypeScript 编译器不会给我们任何错误。

我们无法比较不同枚举的值。

例如，我们不能写:

```
enum Fruit {
  APPLE,
  ORANGE,
  GRAPE
}enum Gender {
  MALE,
  FEMALE
}const apple: number = Fruit[Gender.FEMALE];
```

我们也可以将自己的值赋给一个枚举，因此我们可以写:

```
enum Fruit {
  APPLE,
  ORANGE = 10,
  GRAPE
}
```

那么`Fruit.ORANGE`就是 10，`Fruit.GRAPE`就是 11。

如果常数在赋有值的之后，它将从设定值开始递增。

# 字符串枚举

默认情况下，TypeScript 枚举有数字值。

但是，我们可以给它分配一个字符串值。

例如，我们可以写:

```
enum Fruit {
  APPLE = "apple",
  ORANGE = "orange",
  GRAPE = "grape"
}
```

一旦我们将一个枚举常量设置为一个字符串，我们就必须将它们都设置为一个字符串。

否则，我们会得到一个错误。

![](img/71608088e2fa174fac9ce9a7815794cb.png)

Photo by [Edgar Castrejon](https://unsplash.com/@edgarraw?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 枚举的局限性

enums 有一些限制。

这是因为枚举功能是用 TypeScript 编译器实现的。

例如，假设我们有:

```
enum Fruit {
  APPLE,
  ORANGE,
  GRAPE
}
```

我们可以通过书写为其赋值:

```
const fruit: Fruit = 100;
```

TypeScript 编译器不会阻止我们为枚举类型的变量赋值。

然而，这不是字符串枚举的问题。

此外，`typeof`运算符无法区分枚举和数值。

例如，如果我们写:

```
enum Fruit {
  APPLE,
  ORANGE,
  GRAPE
}let fruit: Fruit = Fruit.APPLE;
if (typeof fruit === "number") {
  console.log("fruit is a number");
}
```

然后我们得到`'fruit is a number'`日志。

# 结论

我们可以将元组用作固定长度的数组，并且它们可以包含不同类型的数据。

元组可以调用数组方法，因为它们是有一些限制的 JavaScript 数组。

枚举让我们可以在一个保护伞下定义常量值。

它们可以有字符串或数字值。

## 简单英语中的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**