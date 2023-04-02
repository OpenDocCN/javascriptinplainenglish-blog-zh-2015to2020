# JavaScript 错误——包装器和对象

> 原文：<https://javascript.plainenglish.io/javascript-mistakes-wrappers-and-objects-98d70dd6bb7f?source=collection_archive---------11----------------------->

![](img/59f54b7954cda6d6dc240b043d7d9310.png)

Photo by [James Orr](https://unsplash.com/@orrbarone?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。编写可以运行但有错误的代码很容易。

在本文中，我们将看到我们不应该使用的坏结构，比如手动为原始值、八进制文本创建包装对象，以及将参数赋给不同的值。

# 没有原始包装实例

对于字符串、数字和布尔值，我们不应该用`new`操作符手动创建包装对象。

我们需要它们来调用原始值的方法，JavaScript 解释器会自动创建包装器，然后调用这些方法。一旦调用完我们想要调用的方法，它们就会被自动销毁。

然而，没有理由手动创建它们。它们令人困惑，因为它们有类型`'object'`。

例如，如果我们有以下表达式:

```
typeof new String("foo");
typeof new Number(3);
typeof new Boolean(false);
```

然后它们都将返回类型`'object'`，即使我们像处理它们的原始对应物一样处理它们。

要将它们转换回原始值，我们必须对它们调用`valueOf`方法，如下所示:

```
new String("foo").valueOf();
new Number(3).valueOf();
new Boolean(false).valueOf();
```

来取回这些包装对象的原始版本。

我们应该将它们写成原始值，如下所示:

```
"foo"
3
false
```

打字更少，也不会混淆类型。

# 没有八进制文字

八进制数字是任何以 0 开头的数字。JavaScript 将任何以 0 开头的数字都视为八进制数。

因此，`051`实际上是十进制的 41。

这是一个混乱的来源，所以 ES5 中不赞成使用八进制数字。

因此，如果我们想在代码中写 51，那么它前面就不应该有 0。

# 没有重新分配函数参数

我们不应该在 JavaScript 中将函数参数重新赋值给不同的值，因为这会改变`arguments`对象。这意味着实际参数被修改。

将函数参数重新赋值给一个新值很容易出错。这使得错误难以追踪。

例如，我们不应该编写这样的代码:

```
const foo = (x) => {
  x = 13;
}const foo = (x) => {
  for (x of baz) {}
}
```

相反，我们应该将参数赋给函数体内的一个变量，然后像下面的代码一样操作它:

```
const foo = (x) => {
  let y = x;
}const foo = (x) => {
  for (const b of baz) {}
}
```

# 不要使用`__proto__ Property to Access the Prototype of the Given Object`

我们不应该使用`__proto__`属性来访问我们试图访问该属性的对象的原型。

相反，我们可以使用`Object.getPrototypeOf`方法来访问当前对象的原型。

例如，不写:

```
const obj = {};
console.log(obj.__proto__)
```

我们应该写:

```
const obj = {};
console.log(Object.getPrototypeOf(obj))
```

# 无变量重新声明

重新声明变量是不好的。然而，如果我们使用`var`关键字来声明我们的变量，这是可能的。我们不希望这种情况发生，因为我们不知道变量实际上有哪些值。

例如，如果我们有:

```
var a = 3;
var a = 2;
```

当我们在最后一行下面记录`a`时，我们得到`a`是 2。

相反，我们应该用`let`声明一个变量，如果我们试图用两个不同的名字声明两个变量，我们会得到一个错误。

![](img/8664caaf20d81d507e69e36aea592d0b.png)

Photo by [Kae Ng](https://unsplash.com/@ffkae?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# return 语句中没有赋值

在 JavaScript 中，我们可以在与`return`语句相同的行中进行赋值操作。

例如，我们可以这样写:

```
const baz = (bar) => {
  return foo = bar + 2;
}
```

在上面的代码中，我们有了`bar`参数，我们给它加了 2。然后我们把总和赋给`foo`，然后返回。

因此`foo`是 5，我们返回了它。然而，这也可能是一个打字错误，因为开发人员可能想写`==`或`===`而不是`=`。

为了使事情更清楚，我们应该将赋值表达式放在与`return`语句不同的行上。

我们也可以在赋值表达式两边加上圆括号，让每个人都清楚我们先把值赋给左操作数，然后把左操作数作为值返回。

所以我们可以写:

```
const baz = (bar) => {
  return foo == bar + 2;
}const baz = (bar) => {
  return foo === bar + 2;
}const baz = (bar) => {
  return (foo = bar + 2);
}
```

而不是我们上面的。

# 结论

我们不应该对原始值使用包装对象，因为手动创建它们没有价值。只会给其他开发者带来困惑，需要更多的打字。

此外，我们不应该在 JavaScript 代码中使用八进制文字，因为它们已经过时了。

我们不应该多次声明同名的变量。

最后，赋值表达式不应该出现在`return`语句中，以减少不确定性。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物，表达对它们的喜爱:[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****