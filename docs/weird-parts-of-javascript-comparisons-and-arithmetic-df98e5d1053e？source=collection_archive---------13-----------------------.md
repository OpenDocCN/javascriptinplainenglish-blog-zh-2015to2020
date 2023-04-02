# JavaScript 奇怪的部分——比较和算术

> 原文：<https://javascript.plainenglish.io/weird-parts-of-javascript-comparisons-and-arithmetic-df98e5d1053e?source=collection_archive---------13----------------------->

![](img/8fd1925af5e64401be1565f9080cb465.png)

Photo by [Museums Victoria](https://unsplash.com/@museumsvictoria?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种编程语言，由于它的过去，它有很多奇怪的行为。它未经询问就对我们正在做的事情做了许多假设。

在本文中，我们将看看 JavaScript 比较和奇怪的算术运算的奇怪部分。

# 比较 3 个数字

如果我们有一连串的比较运算符，它们总是很奇怪。

例如，如果我们有`1 < 2 < 3`，它返回`true`。但是如果我们有`3 > 2 > 1`，它返回`false`。这很奇怪，尽管它看起来应该是`true`。

这是因为它们都是从左向右解释的。因此，如果我们有`1 < 2 < 3`，那么它被解释为`true < 3`，因为 1 小于 2。然后`true`被强制为 1，所以解释为`1 < 3`。

因此，我们有了表达式`1 < 2 < 3`返回`true`。

另一方面，如果我们有表达式`3 > 2 > 1`。然后再一次从左向右解读。因此，我们得到比`3 > 2`返回`true`，因为 3 大于 2。然后表达式`true`被强制为 1。

因此，我们得到`1 > 1`，它返回`false`。

为了避免这样的意外结果，我们应该避免以这种方式链接比较操作符。

# 有趣的算术

JavaScript 的算术运算符几乎可以应用于一个对象。唯一的问题是，我们会得到奇怪的结果，这些结果可能是我们不期望或不想要的。

例如，如果我们有`1 — 1`，我们会得到预期的 0。如果我们有`1 + 1`，我们得到 2，这也是我们所期望的。

如果我们用算术运算混合不同数据类型的操作数，事情会变得很奇怪。

例如，如果我们有`'1' — 1`，我们得到 0。这是因为字符串表示 1 在被`-`操作符使用之前会自动转换成一个数字。

然而，如果我们有`‘1’ + 1`，我们得到`'11'`，因为`+`符号被认为是字符串连接操作符，而不是加法操作符。因此，数字 1 被转换为 1 的字符串表示形式并连接在一起。

如果我们尝试对对象和数组使用`+`操作符，我们会得到更多意想不到的结果。

例如，`[] + []`返回一个空字符串。出于某种原因，JavaScript 强制空数组为空字符串并将它们连接在一起。

同样，`{} + []`返回 0。第一个表达式被解释为空块，而不是空对象。因此，它实际上相当于`+[]`，然后被解释为`Number([])`。

然后解读为`Number([].toString())`。然后我们得到`Number('')`，它返回 0。

表达式`[] + {}`返回`‘[object Object]’`，因为现在它们都被解释为字符串。

空数组被转换为空字符串，现在`{}`被解释为空对象，所以当我们将它转换为字符串时，它被转换为`‘[object Object]’`。

所以我们得到`'' + ‘[object Object]’`，也就是`‘[object Object]’`。

其他奇怪的表达式包括`‘222’ — -’110'`，它返回数字 332，因为它们都被转换为数字。字符串`'222'`被转换成一个数字。

然后我们有了`-`符号，然后我们有了一元`-`操作符，它将字符串`'110'`转换为`-110`。

所以表达式解释为`222 + 110`，是 332。

对于二元`+`运算，以下是可能性及其假设运算的列表:

```
Number  + Number  => addition
Boolean + Number  => addition
Boolean + Boolean => addition
Number  + String  => concatenation
String  + Boolean => concatenation
String  + String  => concatenation
```

![](img/c8a0cb7d0d7a825b6652408b31cb0b68.png)

Photo by [Stephen Broome](https://unsplash.com/@stephenbroome?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

在 JavaScript 中，比较操作链总是从左到右进行解释。所以我们可能会得到意想不到的结果，如果我们有一个链在一起。

由于 JavaScript 的类型强制，如果我们试图使用`+`操作符将 2 个值合并为 1，我们会得到意想不到的有趣结果。

JavaScript 解释器对我们试图组合的内容做了很多假设，并以他们自己的方式进行转换，因为我们试图用`+`操作符进行组合。

这是因为`+`操作符可以是加法和连接操作符。

## **简明英语团队的笔记**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**此外，我们总是有兴趣帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****