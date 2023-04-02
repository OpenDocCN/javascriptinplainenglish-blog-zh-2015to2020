# 编写更健壮代码的 JavaScript 最佳实践——好的和不好的特性

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-for-writing-more-robust-code-good-and-bad-features-b5bd0e7a5126?source=collection_archive---------14----------------------->

![](img/f6506774b81764d882a79c1bb80fc1be.png)

Photo by [Zdeněk Macháček](https://unsplash.com/@zmachacek?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种简单易学的编程语言。编写运行并执行某些操作的程序很容易。然而，很难考虑所有的用例并编写健壮的 JavaScript 代码。

在本文中，我们将看看 JavaScript 的一些好特性，我们应该用它们来编写健壮的 JavaScript 代码。

# 用===代替==

在 JavaScript 中，有用于确定两个实体相等的`===`和`==`操作符。

`==`在对类型的两个操作数进行比较之前，通过遵循一长串规则来强制类型。

另一方面，`===`在比较操作数之前不做任何数据类型强制。

这使得`===`变得更好，因为 JavaScript 的数据类型强制会产生我们意想不到的奇怪结果。

使用`==`操作符会导致类似于`0 == false`返回`true`的表达式。

我们不想因为在代码中保存一个字符而使事情变得更加困难，所以我们应该总是使用`===`来进行相等比较。

同样，为了检查不相等性，我们应该使用`!==`操作符，在比较其操作数的不相等性之前，它也不进行任何数据类型强制。

# 从不使用 Eval

`eval`让我们运行嵌入在字符串中的代码。这意味着理论上，任何用户都可以运行任何东西，只要他们可以设置我们传递给`eval`的字符串的值。

`eval`中的代码在全局范围内运行，所以如果我们让任何东西用`eval`运行，它会产生很大的影响。

因此，我们应该不惜一切代价避免这种情况。此外，它降低了我们的应用程序的性能，因为从字符串运行代码意味着 JavaScript 解释器不能事先进行任何性能优化。

# 永远不要使用函数构造函数

`Function`构造器类似于`eval`。它让我们通过传递想要在函数内部运行的代码来定义函数。

例如，我们可以通过编写以下代码将参数名和函数体作为字符串传递:

```
const add = new Function('x', 'y', 'return x + y');
```

在上面的代码中，我们用参数名`'x'`和`'y'`字符串调用了`Function`构造函数。然后我们用`'return x + y'`作为第三个参数来返回`x`和`y`的和。

然后，我们可以如下运行我们的函数:

```
const sum = add(1, 2);
```

像`eval`一样，我们动态地传入字符串来编写代码。它还在全局范围内运行一切，并像`eval`一样阻止优化。

它也有和`eval`一样的安全问题，因为我们仍然通过字符串来定义我们的函数。

因此，我们也不应该用这个来定义我们的函数。

# 不要在 setTimeout 和 setInterval 中将字符串作为代码传入

`setTimeout`和`setInterval`功能分别允许我们延迟一段时间后运行代码和在指定的时间间隔内运行代码。

我们不应该在它们中使用的一个特性是向它们传递字符串来运行代码。

例如，我们不应该像下面这样写:

```
setTimeout('console.log("foo")', 100)
```

这将在 100 毫秒后运行`console.log("foo")`,但是我们不应该这样做，因为我们再次从一个字符串中运行代码，这与我们从一个字符串中运行代码的其他情况存在相同的安全和性能问题。

因此，我们应该始终运行用代码而不是字符串定义的代码，如下所示:

```
setTimeout(() => console.log("foo"), 100)
```

上面的代码像处理字符串一样运行`console.log`,但是 JavaScript 解释器可以优化代码，并且没有安全问题，因为没有攻击者将代码作为字符串注入我们的应用程序的风险。

![](img/271823d516bc858ec05c30fa4cc5c8b9.png)

Photo by [Paul M](https://unsplash.com/@mobography?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 总是添加分号和括号

应该总是添加分号，以使我们的代码尽可能清晰。当我们自己不插入分号时，JavaScript 可以自动插入分号。对于单行`if`块，括号也是可选的。

当它认为它应该是一个新行时，它会插入它，就像在它发现的任何逻辑中断中。

但是，这往往会导致混乱。如果我们写下如下内容:

```
if (foo)
  x = false
  bar();
```

那么我们可能会认为，如果`foo`是`true`，那么两条线都在运行。但事实并非如此。

JavaScript 解释器会认为只有`x = false`是`if`块的一部分，而`bar()`是独立的。

因此，如果我们想消除这样令人困惑的代码，我们应该写:

```
if (foo) {
  x = false;
  bar();
}
```

用分号和括号分隔我们的行和块。

这样，我们知道如果`foo`是`true`，它们都会运行。

# 结论

对于相等和不相等的比较，`===`和`!==`总是比`==`和`!=`好。

此外，我们不应该从带有`eval`、`Function`构造函数、`setTimeout`、`setInterval`的字符串或者任何其他允许我们这样做的地方运行代码。

最后，我们应该添加分号和括号来分隔代码块。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物: [**AI in Plain English**](https://medium.com/ai-in-plain-english) ，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****