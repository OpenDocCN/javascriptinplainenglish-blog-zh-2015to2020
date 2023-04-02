# JavaScript 的奇怪部分——等式

> 原文：<https://javascript.plainenglish.io/weird-parts-of-javascript-equality-a1cce100ad45?source=collection_archive---------10----------------------->

![](img/15263c8c8ae7ff5ed0762fa919cc5260.png)

Photo by [Matt Popovich](https://unsplash.com/@mattpopovich?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种简单易学的编程语言。编写运行并执行某些操作的程序很容易。

然而，它有许多奇怪的部分，我们应该知道。在本文中，我们将看到一些奇怪的比较结果，在我们编写 JavaScript 代码时应该注意这些结果。

# `[]`等于`![]`

`==`操作符以奇怪的方式比较事物，因为在进行任何比较之前，它基于一组复杂的规则进行类型强制。

奇怪的比较结果的一个例子是`[] == ![];`返回`true`。

这是因为`==`运算符将两边都转换成数字，然后进行比较。在这种情况下，在比较之前，两边都转换为 0。

右侧被转换为 0，因为数组是真的，然后当我们对一个真值求反时，它变成了`false`，然后被强制为 0。

在左侧，空数组被强制转换为一个数字，而不是一个布尔值。根据`==`操作符的规则，它被直接转换为 0。

因此，我们得到两边都是 0，因此，根据`==`运算符的标准，我们得到`[]`等于`![]`。

JavaScript 解释器进行比较的步骤如下:

```
+[] == +![];
0 == +false;
0 == 0;
true;
```

# `true`不等于`![]`，但也不等于`[]`

在 JavaScript 中，空数组不等于`true`，空数组的反运算也不等于`true`。

但是，根据`==`运算符，两者都等于`false`。

因此，我们有以下两个表达式返回`false`:

```
true == []; 
true == ![];
```

并且以下表达式均返回`true`:

```
false == []; 
false == ![];
```

`true == [];`是`false`，因为右边直接转换为 0，如前一节所见。

于是`true == [];`变成了`toNumber(true)== toNumber([]);`，变成了`1 == 0`，也就是`false`。

在`true == ![];`中。只有右侧被转换为不同的值。表达式转换成`true == false`，也是`false`。

由于以下转换，`false == [];`表达式返回`true`:

```
toNumber(false) == toNumber([]);
0 == 0;
```

它首先用内置的`toNumber`函数将`false`和`[]`转换成数字。

然后两个值都转换成 0，由于两边都是 0，所以返回`true`。`false`是 falsy，所以换算成 0。根据`toNumber`函数，空数组也被转换为 0。

在与`==`运算符进行比较之前，根据以下过程将`false == ![];`表达式转换为不同的值。

左侧保持不变，但是右侧被转换为`false`，因为它是真值的否定。

所以我们得到`false == false`，也就是`true`。

![](img/27b262f0a1bba3eeff331186f62a6761.png)

Photo by [Lukas Boekhout](https://unsplash.com/@lakus?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 真就是假

在 JavaScript 中，布尔表达式:

```
!!"false" == !!"true";
!!"false" === !!"true";
```

双双返回`true`。

这是因为在第一种情况下，在用`==`操作符进行比较之前，JavaScript 再次将项目转换为不同的值。

在第二种情况下，双重否定会在进行比较之前将它们都转换为布尔型。在进行比较之前,`===`操作符本身不做任何数据类型强制。

在`!!”false” == !!”true”;`表达式中，右边的`'true'`被转换为`NaN`，因为它试图将非数字内容的字符串转换为数字。那么双重否定会把它转换成`false`，因为`NaN`是假的。

左侧的`'false'`转换为内置`ToBoolean`功能的`false`。

因为它们都是`false`，我们得到`!!”false” == !!”true”;`返回`true`。

同样的逻辑，从`false === false`开始，`!!”false” === !!”true”;`因为同样的数据类型被双重否定所强制。

# 结论

JavaScript 的数据类型强制做了奇怪的事情。从上面的例子中我们可以看到，`==`进行了自动的数据类型强制，这做了很多我们没有预料到的事情。

通过使用`===`进行比较，可以避免自动数据类型强制，所以我们应该这样做。

像`!!`这样的手动数据类型强制也有它的问题，但是它是不可避免的，所以我们必须记住规则，这样我们就不会犯错误。

## **简明英语笔记**

你知道我们有四种出版物吗？给他们一个关注来表达爱意吧:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)，[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**此外，我们总是有兴趣帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****