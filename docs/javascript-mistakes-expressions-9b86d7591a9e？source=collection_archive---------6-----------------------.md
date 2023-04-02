# JavaScript 错误—表达式

> 原文：<https://javascript.plainenglish.io/javascript-mistakes-expressions-9b86d7591a9e?source=collection_archive---------6----------------------->

![](img/bdc6c75fc5804579b5f8879d03afefdf.png)

Photo by [Church of the King](https://unsplash.com/@cotk_photo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。编写可以运行但有错误的代码很容易。

在本文中，我们将看到一些令人困惑的表达式，这些表达式不应该用 JavaScript 代码编写。

# 令人困惑的多行表达式

JavaScript 具有自动分号插入(ASI)特性，可以自动添加分号。

因此，我们可以省略分号，仍然有一个有效的 JavaScript 代码。然而，这并不意味着它们对用户来说容易阅读。

在 JavaScript 中，换行符总是以分号结束语句，除非:

*   该语句有未闭合的括号、数组文字、对象文字或以其他方式结束，这不是结束语句的有效方式
*   线条是`--`或`++`
*   是`for`、`while`、`do`、`if`或者`else`没有`(`
*   接下来的几行以只能在两个操作数之间找到的算术或其他二元运算符开始

有些情况下，多行表达式中的新行看起来像是在结束一个语句，但实际上不是。例如，以下不是多行的，但实际上是一个表达式:

```
let b = 3
let a = b
(1 || 2).c();
```

我们将得到“b 不是函数”消息，因为最后两行被解释为:

```
let a = b(1 || 2).c();
```

另一个例子如下:

```
let addNumber = ()=>{}
let foo = 'bar'
[1, 2, 3].forEach(addNumber);
```

上面的代码会给我们带来语法错误。因此，我们应该在每个语句的末尾加上分号，这样开发人员或 JavaScript 解释人员就不会混淆或出错。

# return、throw、continue 和 break 语句后无法访问的代码

`return`、`throw`、`break`和`continue`之后的不可达代码是无用的，因为这些语句无条件地退出代码块。

因此，我们不应该有那些永远不会在这些行之后运行的代码。

例如，以下函数具有不可访问的代码:

```
const fn = () => {
  let x = 1;
  return x;
  x = 2;
}
```

`x = 2`不可达，因为它在`return`语句之后。

我们不应该编写的其他代码包括:

```
while(true) {
    break;
    console.log("done");
}const fn = () => {
  throw new Error("error");
  console.log("done");
}
```

因此，我们应该写:

```
const fn = () => {
  let x = 1;
  return x;
}
```

# finally 块中的控制流语句

在 JavaScript 中，当添加到`try...catch`块之后时，`finally`块总是在`try...catch`块结束之前运行。因此，每当`finally`中有`return`、`throw`、`break`、`continue`时，`try...catch`中的就会被覆盖。

例如，如果我们有:

```
let x = (() => {
  try {
    return 'foo';
  } catch (err) {
    return 'bar';
  } finally {
    return 'baz';
  }
})();
```

那么`x`将是`'baz'`，因为`finally`块的`return`语句在`try...catch`块的语句之前运行。

因此，我们不应该在 finally 块中添加流控制语句，因为这会使`try...catch`中的语句变得无用。我们应该这样写:

```
let x = (() => {
  try {
    return 'foo';
  } catch (err) {
    return 'bar';
  } finally {
    console.log('baz');
  }
})();
```

以便`try...catch`中的`return`语句有机会运行。

![](img/f944924e628ab47f4f3c471cd06ec3d8.png)

Photo by [Gabriel Silvério](https://unsplash.com/@gabrielsilverio?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 对关系运算符的左操作数求反

否定左操作数并不总是否定 JavaScript 中的整个运算表达式。

例如:

```
!prop in object
```

仅否定`prop`和:

```
!foo instanceof C
```

仅否定`foo`。

因此，`!prop in object`实际上是`true in object`或`false in object`，取决于`prop`的真度。

同样，`!foo instanceof C`实际上是，`true instanceof C`还是`false instanceof C`取决于`foo`的真实度。

因此，我们应该把整个表达式用括号括起来，这样它实际上就否定了整个表达式的返回值，如下所示。因此:

```
!(prop in object)
```

并且:

```
!(foo instanceof C)
```

是我们想要的。

这样，我们就不会对这些表达式的用途产生混淆。

# 结论

用 JavaScript 创建令人困惑的表达式有很多方法。一种方法是省略行尾的分号。省略分号很容易产生歧义。

我们也可以通过编写不可及的表达式来创建无用的代码。它们毫无用处，所以不应该被写出来。这包括在`return`、`break`、`throw`和`continue`之后编写代码。此外，在`finally`中编写这些语句也会使`try...catch`中的语句变得无用。

最后，对`in`和`instanceof`操作符的左操作数求反只是对左操作数求反，而不是对整个表达式求反。

## **用简单英语写的 JavaScript 的一个注释:**

我们已经推出了三种新的出版物！请关注我们的新出版物: [**AI in Plain English**](https://medium.com/ai-in-plain-english) ，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**