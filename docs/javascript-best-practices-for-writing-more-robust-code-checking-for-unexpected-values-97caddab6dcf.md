# 编写更健壮代码的 JavaScript 最佳实践——检查意外值

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-for-writing-more-robust-code-checking-for-unexpected-values-97caddab6dcf?source=collection_archive---------9----------------------->

![](img/94adcc54febfd9d2f0cd63add9fe993a.png)

Photo by [Zdeněk Macháček](https://unsplash.com/@zmachacek?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种简单易学的编程语言。编写运行并执行某些操作的程序很容易。然而，很难考虑所有的用例并编写健壮的 JavaScript 代码。

在本文中，我们将研究检查意外值的方法，包括`null`和`NaN`。

# 检查是否为空

`null`也是一个我们可能会意外遇到的值，所以我们也应该检查一下。

检查`null`有点棘手，因为当`typeof null`返回`'object'`时，我们不能使用`typeof`操作符来检查`null`。

但是，我们仍然可以使用`===`运算符或`Object.is`方法来检查`null`。

我们可以使用`===`操作符来检查`null`，如下所示:

```
if (foo === null) {
  //...
}
```

在上面的代码中，我们只是使用`===`操作符来检查`null`，就像我们检查任何其他值一样。

`===`操作符不进行任何数据类型强制，所以在进行比较之前，它会将值与任何数据类型转换进行比较。

因此，上面的代码将适用于`null`比较，因为在进行比较之前，它不会试图将`null`转换成其他任何东西。

`Object.is`是一个静态对象方法，它接受两个参数，这两个参数接受两个要比较的对象。

如果两个参数都是`null`，它将返回`true`，所以我们可以用它来检查某个东西是否是`null`。

为了用`Object.is`检查`null`，我们可以编写以下代码:

```
if (Object.is(foo, null)) {
  //...
}
```

在上面的代码中，我们用`foo`和`null`调用了`Object.is`。那么如果`foo`是`null`，`Object.is`会返回`true`。

# 检查值是否为 NaN

`NaN`是一个值，如果我们尝试将非数字转换成数字，或者尝试将 0 或非数字除以 0，就会遇到这个值。

检查`NaN`的方法有很多，包括`isNaN`函数、`Number.isNaN`方法和`Object.is`。

`===`和`==`操作符不起作用，因为如果我们使用这些操作符，`NaN`被认为是不同的。因此`NaN === NaN`和`NaN == NaN`都返回`false`。

然而，对于`NaN`，仍然有许多选项需要检查。

全局`isNaN`函数可用于检查一个值是否为`NaN`。在使用此函数检查`NaN`之前完成数据类型强制。

如果我们传入一个东西，如果我们把它转换成一个数字，它就是`NaN`，那么`isNaN`将返回`true`。否则，它将返回`false`。

例如，如果我们有:

```
isNaN(NaN);       
isNaN(undefined);
isNaN({});
```

然后它们都返回`true`，因为如果我们试图将它们转换成一个数字，它们都返回`NaN`。

另一方面，如果我们有:

```
isNaN(true);      
isNaN(null);
```

这些将返回`true`，因为`true`强制为 1，`null`强制为 0，然后与`isNaN`进行比较，检查是否为`NaN`。

同样，以下返回`true`:

```
isNaN('456ABC');
```

因为`parseInt(‘456ABC’);`返回`456`，而`Number('456ABC')`返回`NaN`。

`Number.isNaN`函数检查一个值是否为`NaN`，但在进行比较之前不会尝试将其转换为数字。这是 ES2015 的新方法。

例如，我们可以使用它来比较如下值:

```
Number.isNaN(NaN);        
Number.isNaN(Number.NaN); 
Number.isNaN('foo' / 0);
Number.isNaN(0 / 0);
```

它们都返回`true`，因为我们放入参数的所有表达式都被赋值为`NaN`。

任何不是`NaN`的东西都将返回`false`，所以像这样的表达式:

```
Number.isNaN('foo');
```

除了参数中的数字，还将返回`false`。它只检查值是否为`NaN`，而不尝试先转换一个数字。

同样，我们可以使用`Object.is`方法来检查一个值是否为`NaN`。像`Number.isNaN`一样，在与`NaN`进行相等比较之前，它不会尝试进行任何数据类型强制。

`Object.is`认为`NaN`等于自身，所以我们可以用它来检查`NaN`。

因此，我们可以像使用`isNaN`一样使用它。例如，我们可以写:

```
Object.is(NaN, NaN);
Object.is(Number.NaN, NaN); 
Object.is('foo' / 0, NaN);
Object.is(0 / 0, NaN);
```

将全部返回`true`。当与`NaN` will `Object.is`进行比较时，任何没有被显式评估为`NaN` will 的都将返回`false`。例如:

```
Number.isNaN('foo');
```

返回`false`。

![](img/3c96b16d4213101ffeb15fb557265449.png)

Photo by [Ron Binette](https://unsplash.com/@ronbinette?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

检查`null`和`NaN`各有窍门。除了`Object.is`之外，我们还可以使用`===`来检查`null`。

为了检查`NaN`，我们不能用`===`来检查`NaN`，因为`NaN === NaN`返回`false`。不过我们可以用`Object.is`、`isNaN`或者`Number.isNaN`来做比较。

## **简明英语团队的笔记**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**此外，我们总是有兴趣帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****