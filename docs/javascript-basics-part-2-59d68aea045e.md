# JavaScript 基础知识—第 2 部分

> 原文：<https://javascript.plainenglish.io/javascript-basics-part-2-59d68aea045e?source=collection_archive---------5----------------------->

![](img/fe3365ba1864e2f44c0886f281a0be65.png)

在本文中，我们将研究 JavaScript 的基础知识。这是 JavaScript 基础文章系列的第 2 部分。您可以在此处查看第 1 部分:

[](https://medium.com/@endubueze00/javascript-basics-part-1-adae36124fd1) [## JavaScript 基础知识—第 1 部分

### 在这篇文章中，我们将研究 JavaScript 的基础知识，还将研究 JavaScript 的一些部分，您可能…

medium.com](https://medium.com/@endubueze00/javascript-basics-part-1-adae36124fd1) ![](img/50fb802c795522cba46304acffcc8f17.png)

**单行注释**

在 JavaScript 中，要注释掉一行文本，需要在文本前加两个正斜杠。代码将忽略文本，这有助于您记住代码。

```
// This is a comment
```

![](img/64ad58f4a76f24013be4d693aeaa3a35.png)

**空**

JavaScript Null 是一种原始数据类型。它是有意的没有任何价值。对于布尔运算，它也被视为假值。

```
let val = null;
```

![](img/19be9d2ab57bae165b5c14de4006febd.png)

**琴弦**

字符串是一种基本的数据类型，由任何一组用单引号或双引号括起来的字符(无论是字母、空格还是数字)表示。

在第一个例子中，因为这些数字用单引号括起来，JavaScript 会将它们作为字符串读取。

```
let x = '1234'let y = "Can we eat some porridge?"
```

![](img/b980b020366daf5bf4cbd2bb5b554538.png)

**算术运算符**

以下是 JavaScript 算术运算符:

添加

```
20 + 6
```

减法

```
20 - 6
```

增加

```
20 * 6
```

分开

```
20 / 6
```

模—返回除法余数。

```
20 % 6 // the most 6 can go into 20 is 18 leaving 2 as the remainder
```

![](img/00f9744ca8f438e46a2850205534a077.png)

**多行注释**

单行注释通过在文本前面加两个正斜杠来使用。对于多行注释，请执行以下操作:

```
/*
This is a multi-line text
that is one or more lines more than
a single line text
*/
```

注释以/*开始，以*/结束。

![](img/f2603906b77af884dd5b81b16da1325d.png)

**赋值运算符**

赋值运算符使用算术运算符之一(如上所述)来计算左边右边的值。使用赋值运算符有两种简便方法:

```
let x = 20;x = x + 20;
x += 20; // shorthand way to add 20 to the x variablex = x - 20;
x -= 20;x = x * 20;
x *= 20;
```

![](img/9954258f3e521c56a0d7cf2adfce2919.png)

**字符串插值**

字符串插值允许您使用模板文字在字符串中嵌入或添加占位符。要使用模板文字，需要用反斜杠将字符串括起来，并使用美元符号和花括号嵌入变量或表达式。这里有一个例子:

```
let name = "porridge"`The old lady enjoys eating ${name}.`; // string interpolation
```

这通常比使用字符串连接更简单。

```
"The old lady enjoys eating " + name + ".";
```

我们的 JavaScript 基础知识系列的第 2 部分到此结束。