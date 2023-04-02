# 编写更健壮代码的 JavaScript 最佳实践——检查未定义

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-for-writing-more-robust-code-checking-undefined-ba062d492f87?source=collection_archive---------3----------------------->

![](img/a4627b5e31e8e023fa4d240abb9e9d3c.png)

Photo by [Viktor Juric](https://unsplash.com/@zurux?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种简单易学的编程语言。编写运行并执行某些操作的程序很容易。然而，很难考虑所有的用例并编写健壮的 JavaScript 代码。

在本文中，我们将研究检查`undefined`和属性是否存在的方法，以防止 JavaScript 程序崩溃。

# 使用 typeof 运算符检查未定义的

`undefined`是我们经常遇到的返回`undefined`的方法或者试图使用不存在的属性的值。因此，在执行我们的操作之前，我们应该检查某个变量是否为`undefined`。

我们可以通过使用`typeof`操作符或`===`操作符来检查`undefined`。

例如，我们可以用`typeof`操作符来实现，如下所示:

```
if (typeof foo === 'undefined') {
  //...
}
```

在上面的代码中，我们通过对`foo`使用`typeof`操作符来检查`foo`是否为`undefined`，并查看它是否返回`'undefined'`。

如果是，那我们就做点什么。如果`foo`不是`undefined`,我们也可以通过写:

```
if (typeof foo !== 'undefined') {
  //...
}
```

这很重要，因为试图对某些东西做任何操作都会使我们的 JavaScript 应用程序崩溃。

另外，通过检查是否是`undefined`来检查属性是否存在也很重要。如果不是`undefined`，则属性存在。

为了检查对象属性是否存在，我们也可以使用`hasOwnPropety`来查看属性是否作为非继承属性存在。

我们可以检查对象中是否存在属性，如下所示:

```
if (typeof foo.bar !== 'undefined' &&
  typeof foo.bar.baz !== 'undefined') {
  //...
}
```

在上面的代码中，我们通过检查`foo.bar`和`foo.bar.baz`是否都不是`undefined`来检查它们是否存在。如果第一个不存在，那么我们知道`foo.bar`存在，那么我们可以继续检查`foo.bar.baz`是否存在。

我们不能直接跳到第二个表达式，只写`typeof foo.bar.baz !== ‘undefined’`，因为`foo.bar`可能不存在。因此，它可能是`undefined`，所以在进行第二次检查之前，我们还必须检查`foo.bar`是否存在。

如果我们想对`foo.bar.baz`做一些操作，这是很关键的，因为这些属性可能不存在。

# object . prototype . hasownproperty

要检查一个属性是否作为非继承属性存在于对象中，我们也可以在对象实例中使用`hasOwnProperty`方法。

例如，我们可以如下重写我们的属性检查:

```
if (foo.hasOwnProperty('bar') &&
  foo.bar.hasOwnProperty('baz')) {
  //...
}
```

在上面的代码中，我们检查了`foo.bar`是否作为`foo.hasOwnProperty(‘bar’)`的非继承属性存在。然后如果`foo.bar`存在，我们通过写`foo.bar.hasOwnProperty(‘baz’)`来检查`foo.bar.baz`是否存在。

然而，如果属性被显式设置为`undefined`，那么`hasOwnProperty`仍然返回`true`，所以即使我们使用`hasOwnProperty`，我们可能仍然需要检查属性是否被显式设置为`undefined`。

因此，`typeof`对于检查`undefined`可能更可靠，因为无论`undefined`是隐式的，也就是当属性不存在时，或者当它确实存在并被显式设置为`undefined`时，它都有效。

# ===运算符

我们也可以只使用`===`操作符来检查`undefined`。例如，我们可以直接与`===`操作符进行比较来检查`undefined`，如下所示:

```
if (foo.bar !== undefined) {
  //...
}
```

上面的代码也可以工作，因为`===`在进行任何比较之前不进行任何数据类型强制。

# 对象. is

最后，我们可以使用`Object.is`方法来检查`undefined`。它需要两个参数，这是我们想要比较的项目。

`Object.is`与`===`不同，因为`===` +0 和-0 在`Object.is`中被认为是不同的，而`===`并非如此。同样，`NaN`被认为与`Object.is`本身相同。

然而，对于比较`undefined`，在`Object.is`和`===`之间没有区别。

例如，我们可以使用下面的`Object.is`来检查`undefined`:

```
if (Object.is(foo.bar, undefined)) {
  //...
}
```

上面的代码检查`foo.bar`是否为`undefined`或者`foo.bar`是否存在，就像我们对`===`操作符所做的那样。

![](img/5ad5007a8cd8964c15d044e0f84cdf70.png)

Photo by [David Rotimi](https://unsplash.com/@davidrotimi?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

在 JavaScript 中有很多方法可以检查某个东西是否为`undefined`。一种方法是使用`typeof`操作符来检查某个东西是否具有类型`'undefined'`。

同样，我们可以使用`===`运算符直接比较某个东西是否为`undefined`。同样，我们可以使用`Object.is`来检查`undefined`。

为了检查一个属性是否存在于一个对象中，我们可以使用对象实例的`hasOwnProperty`方法。然而，如果该属性被显式设置为`undefined`，这将不起作用，因为它存在并且被设置为`undefined`。

## 来自简明英语团队的说明

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)，[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**此外，我们总是有兴趣帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****