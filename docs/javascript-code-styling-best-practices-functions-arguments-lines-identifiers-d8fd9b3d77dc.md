# JavaScript 代码样式最佳实践—函数参数、行、标识符

> 原文：<https://javascript.plainenglish.io/javascript-code-styling-best-practices-functions-arguments-lines-identifiers-d8fd9b3d77dc?source=collection_archive---------9----------------------->

![](img/1439d442c7f4a45314a89db8ff673826.png)

Photo by [Zdeněk Macháček](https://unsplash.com/@zmachacek?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。很容易写出看起来乱七八糟、难以阅读但却能运行的代码。

在本文中，我们将研究函数调用的参数之间的换行符和括号之间的一致换行符等问题。

# 在函数调用的参数之间添加换行符

只有当我们传递的表达式很长时，函数调用的参数之间才需要换行符。

例如，如果我们只是调用一个有几个数字的函数，那么我们不需要在参数之间换行。

我们可以写:

```
foo(1, 2, 3);
```

因为它不会占用太多空间，而且我们已经可以很容易地读取传递给`foo`函数调用的参数。

另一方面，如果我们像下面这样为参数传递长表达式，那么我们应该在参数之间添加换行符:

```
const foo = () => {};
foo(
  [1, 2, 3, 4, 5, 6],
  [1, 2, 3, 4, 5],
  [1, 2, 3, 4, 5, 6, 7, 8]
);
```

这样，我们可以清楚地阅读作为参数传入的每个表达式。每一个长表达式都在自己的行中，这意味着它们不会溢出页面，所以我们不必水平滚动。

它也清楚地区分了争论的开始和结束。

# 函数括号内的一致换行符

通常，我们不需要在函数签名的括号内换行，因为我们的代码中不应该有太多的函数参数。

我们的函数中最多应该有 5 个参数，这样签名就不会淹没我们代码的读者。

这使得阅读代码更容易，并减少了错误地传入错误类型的数据或以错误的顺序传入参数的机会。

函数的参数越多，当我们在函数调用中传递参数时就越容易出错。

因此，我们通常不需要在代码的参数之间换行。

我们可以这样写:

```
const foo = (a, b) => {};
```

上面的`foo`函数中的签名表明，我们不需要在函数签名中将参数分成它们自己的行。

唯一可能的例外是接受我们正在析构的有很多条目的对象参数。

例如，我们可能有一个对象参数，它有许多属性，并且可能有默认值分配给一些参数或条目。

我们可以写:

```
const foo = ({
  foo = 1,
  bar,
  baz,
  qux,
  a
}) => {};
```

在上面的代码中，我们的`foo`函数接受一个具有 5 个或更多属性的参数。

我们作为参数传入的对象包括 5 个属性，包括默认值为 1 的`foo`、`bar`、`baz`、`qux`和`a`，可能还有更多我们没有选择析构的属性。

我们将每个属性放在一行中，以便与对象保持一致，对象在自己的行中有属性和值，并为默认值留出空间。

# 最小和最大标识符长度

我们可能希望强制最小或最大标识符长度，以便我们可以创建描述性的但不太长的标识符。

例如，我们可能希望强制最小标识符长度，这样人们就不能定义太短的变量名，如下所示:

```
let x = 1;
```

在上面的代码中，我们定义了一个名为`x`的变量，没有人知道 ie 是什么意思，因为没有上下文，这个名字也不是描述性的。

例如，如果我们让每个人创建至少 5 个字符长的标识符名称，那么我们可能看不到这些类型的标识符被创建。

例如，如果规则发现上面的例子有问题，那么我们可以将它改为:

```
let numFruits = 1;
```

现在我们知道，变量实际上意味着水果的数量。

我们可能还想创建最大标识符长度，这样人们就不会创建太长的名字，也不会创建包含大量无用字符的名字，这些字符不会向我们传达任何有用的信息。

![](img/2d2a4c8e3b0dca71555afa120d2c9127.png)

Photo by [Zdeněk Macháček](https://unsplash.com/@zmachacek?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

只有当我们传递给函数调用的表达式很长时，函数调用的参数之间才需要换行符。

函数签名通常不需要换行符，因为函数通常不应该有太多的参数。唯一可能的例外是当我们析构对象的时候。

我们可能还想强制实施最小和最大标识符长度，以便标识符创建我们的描述性但不太冗长。

# **简明英语团队的笔记**

你知道我们有四种出版物吗？给他们一个关注来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**我们还推出了一个 YouTube，希望你能通过 [**订阅我们的简明英语频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) 来支持我们**

**一如既往,“简明英语”希望帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，附上您的媒体用户名和您感兴趣的内容，我们将会与您联系！****