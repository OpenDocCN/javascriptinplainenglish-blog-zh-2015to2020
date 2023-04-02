# 使用 TypeScript-Never 和未知类型，从联合中移除 null

> 原文：<https://javascript.plainenglish.io/using-typescript-never-and-unknown-types-removing-null-from-unions-81aa77842725?source=collection_archive---------5----------------------->

![](img/a15625c246122411c5fed89921105806.png)

Photo by [timJ](https://unsplash.com/@the_roaming_platypus?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

TypeScript 是 JavaScript 的自然扩展，在许多项目中取代了 JavaScript。

然而，并不是每个人都知道它实际上是如何工作的。

在本文中，我们将看看`never`类型、`unknown`类型的使用，以及从联合中移除`null`。

# 从不打字

TypeScript 为我们提供了`never`类型，用于类型保护已经处理了一个值的所有可能类型的情况。

一旦处理完所有可能的类型，编译器将只允许给`never` 类型赋值。

例如，如果我们有一个`switch`语句:

```
const getTax = (price: number, format: boolean): string | number => {
  if (typeof price !== "number") {
    return 0;
  }if (format) {
    return (price * 0.2).toFixed(2) as string;
  }
  return (price * 0.2) as number;
};let tax = getTax(100, false);
switch (typeof tax) {
  case "number":
    console.log(`Number: ${tax.toFixed(2)}`);
    break;
  case "string":
    console.log(tax);
    break;
  default:
    let value: never = tax;
    console.log(`Unexpected type for value: ${value}`);
}
```

然后我们将`default`案例的值赋给一个`never`类型的变量，因为我们在前面的案例中处理了字符串和数字。

# 使用未知类型

`unknown`型是`any`的更安全的替代品。

它表明我们不知道值的类型。

我们可以将事物分配给一个`unknown`类型。

例如，我们可以写:

```
let tax: unknown = getTax(100, false);
```

`tax`如果没有类型声明，不能将其赋给另一种类型的变量。

# 可空类型

`null`和`undefined`类型不在 TypeScript 类型系统中。

然而，我们可以通过使用可空类型来创建变量的可空版本。

例如，我们可以写:

```
const getTax = (price: number, format: boolean): string | number | null => {
  if (typeof price !== "number") {
    return null;
  }if (format) {
    return (price * 0.2).toFixed(2) as string;
  }
  return (price * 0.2) as number;
};
```

现在除了`number`或者`string`之外我们还可以返回`null`。

对于参数，我们可以写成:

```
const getTax = (price?: number, format: boolean): string | number | null => {
  if (typeof price !== "number") {
    return null;
  } if (format) {
    return (price * 0.2).toFixed(2) as string;
  }
  return (price * 0.2) as number;
};
```

我们在参数名旁边放了一个`?`，这样我们就可以表明它可能是`null`或`undefined`。

# 限制可空赋值

我们可以通过启用`strictNullChecks`编译器设置来限制`null`或`undefined`的使用。

如果我们将它设置为`true`，那么我们就不能将`null`赋值给不是指定了其他数据类型的变量类型。

如果我们打开它，那么我们将得到'类型' null '不可赋给类型' string | number '。ts(2322)'。

所以我们必须像以前一样将`null`添加到类型联合中。

# 从联合中移除 null

我们可以用非空断言从联合类型中移除`null`。

例如，如果我们写:

```
let tax: string | number = getTax(100, false)!;
```

然后我们确保没有将`null`返回并分配给`tax`。

我们也可以用类型守卫移除`null`。

例如，我们可以写:

```
if (tax !== null) {  
  //..
}
```

在继续之前进行空值检查。

![](img/65ce6b949c1bf92274293f8a08c14e19.png)

Photo by [Joseph Gonzalez](https://unsplash.com/@miracletwentyone?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 明确赋值断言

如果`strictNullChecks`选项被启用，如果变量在赋值前被使用，编译器将报告一个错误。

例如，我们可以将`null`添加到`tax`变量的联合类型中，然后在以后断言它。

我们写道:

```
let tax: string | number | null = getTax(100, false);
```

然后我们可以用`as`或者括号把它缩小到其他语句中我们想要的类型。

# 结论

当我们在条件语句中处理所有其他类型时，会用到`never`类型。

`unknown`比`any`更安全。`unknown`类型的变量不能赋给其他变量。

从联合中移除类型有多种方法。

## **简单英语的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**