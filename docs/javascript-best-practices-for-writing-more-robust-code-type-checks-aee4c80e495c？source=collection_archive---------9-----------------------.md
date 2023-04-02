# 编写更健壮的代码类型检查的 JavaScript 最佳实践

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-for-writing-more-robust-code-type-checks-aee4c80e495c?source=collection_archive---------9----------------------->

![](img/65bbd7e11aece48a3ebba35bdbbd7d13.png)

Photo by [Frans Daniels](https://unsplash.com/@tales_of_light?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种简单易学的编程语言。编写运行并执行某些操作的程序很容易。然而，很难考虑所有的用例并编写健壮的 JavaScript 代码。

在本文中，我们将看看如何在 JavaScript 中正确地进行类型检查，这样我们在运行应用程序时就不会遇到数据类型错误。

# 运算符的类型

我们可以使用`typeof`操作符来检查原始数据类型和一些对象类型，比如函数。

这些检查是必要的，以便我们知道我们正在处理正确类型的数据。例如，当我们实际上想要将数字加在一起时，我们不想将`+`操作符与字符串一起使用。

为了使用它，我们在想要检查的值前添加`typeof`。`typeof`操作符返回的可能值如下:

*   未定义— `"undefined"`
*   空— `"object"`
*   布尔型— `"boolean"`
*   数量— `"number"`
*   BigInt(ECMAScript 2020 中的新功能)——`"bigint"`
*   字符串— `"string"`
*   符号(ECMAScript 2015 中的新功能)——`"symbol"`
*   函数对象(在 ECMA-262 术语中实现[[调用]])—`"function"`
*   任何其他对象— `"object"`

这些是`typeof`可以返回的所有可能的类型名字符串。除了`'object'`和`'function'`之外，大多数都是原始值类型。

注意`typeof null`返回`'object'`。这是一个 bug，但是由于它破坏了太多的现有代码而无法纠正，所以它保持原样。

例如，我们可以使用`typeof`操作符进行以下检查:

```
typeof 3 === 'number';
typeof 3.1 === 'number';
typeof(40) === 'number';
typeof Math.LN2 === 'number';
typeof Infinity === 'number';
typeof NaN === 'number';
```

以上代码全部返回`true`。从上面的代码中我们可以看到，当我们使用数字、`Infinity`和`NaN`作为操作数时，它们都返回类型`'number'`。

BigInt 检查的一个示例如下:

```
typeof 41n === 'bigint';
```

BigInts 在数字文字后面有一个`n`，以区别于普通数字。而且，它们只能是整数，大多数情况下只能对其他 BigInts 进行操作。

要检查字符串，我们可以编写如下代码:

```
typeof '' === 'string';
typeof 'bla' === 'string';
typeof `template` === 'string';
```

这对普通字符串和模板字符串都适用。

为了检查布尔值，我们可以写:

```
typeof true === 'boolean';
typeof false === 'boolean';
typeof Boolean(0) === 'boolean';
typeof !!(1) === 'boolean';
```

`Boolean()`和`!!`做同样的事情。它们都将参数或操作数转换为布尔值。以上表情全部返回`true`。

为了检查符号，我们可以写:

```
typeof Symbol() === 'symbol'
typeof Symbol('foo') === 'symbol'
typeof Symbol.iterator === 'symbol'
```

以上表达式均返回`true`。

为了检查函数，我们可以写:

```
typeof () => {} === 'function';
typeof class C {} === 'function';
typeof Math.sin === 'function';
```

注意，由于类只是 JavaScript 中构造函数的语法糖，所以类在与`typeof`一起使用时会返回`'function'`。以上表达式均返回`true`。

为了检查某物是否是一个对象，我们可以这样写:

```
typeof {a: 1} === 'object';
typeof [1, 2, 3] === 'object';
typeof new Date() === 'object';
```

以上表达式均返回`true`。

我们应该对原始值使用包装器对象，所以下面这些是无用的和混乱的:

```
typeof new Boolean(true) === 'object'; 
typeof new Number(1) === 'object'; 
typeof new String('abc') === 'object';
```

所有 3 个表达式都是`true`，因为它们都有类型`'object'`。这不是对象文字的问题，因为它们返回我们期望的类型。

然而，它们并没有比文字做得更好，所以我们应该避免使用构造函数来定义原始值。

从 ES2015 开始，`typeof`运算符不再处理未声明的变量。

例如，如果没有声明`foo`，那么在 ES2015 或更高版本中，那么`typeof foo`将在我们的代码中抛出一个错误。

而在 ES2015 之前，如果变量未声明，`typeof foo`返回`undefined`。

对于块范围的变量或常量，我们只能在声明变量时对它们进行操作。例如，以下内容:

```
typeof foo === 'undefined';
let foo;
```

会给我们一个`ReferenceError`。

因此，我们在使用`typeof`操作符时应该小心，这样我们就不会在检查过程中导致更多的崩溃。

![](img/36dcba132ca1627f00324bb561e29a83.png)

Photo by [David Clode](https://unsplash.com/@davidclode?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

操作符让我们主要在原始值上进行类型检查。然而，我们也可以用它来检查一个函数或类是否是一个函数。

其他类型的对象将返回带有`typeof`操作符的`'object'`，所以它不是很有用。

## 来自简明英语团队的一封信

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！我们还推出了一个 YouTube，希望你能通过 [**订阅我们的简明英语频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) 来支持我们**

**一如既往,“简明英语”希望帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****