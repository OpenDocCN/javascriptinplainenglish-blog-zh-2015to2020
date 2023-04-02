# JavaScript 代码样式化最佳实践— If 和混合字符

> 原文：<https://javascript.plainenglish.io/javascript-code-styling-best-practices-ifs-and-mixing-characters-64ee8141164a?source=collection_archive---------8----------------------->

![](img/82274de991aa07d763e864cb586da985.png)

Photo by [Jerome Heuze](https://unsplash.com/@jeromeheuze?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。编写可以运行但有错误的代码很容易。

在本文中，我们将看看`else`块中的`if`块、混合运算符、混合空格和制表符以及多重赋值。

# `Lone if`语句出现在`else`块中

`else`块中单独的`if`语句可以被重写为`else if`块。

因此，我们可能不需要在一个`else`块中有一个单独的`if`块。

如果我们有以下代码:

```
if (a) {
  // ...
} else {
  if (b) {
    // ...
  }
}
```

那么它应该重写为:

```
if (a) {
  // ...
} else if (b) {
    // ...
}
```

它比第一个例子中的更短，更简洁。在阅读代码时，嵌套更少，混乱也更少。嵌套代码总是比非嵌套代码更令人困惑。

然而，如果我们有比`else`块中的`if`块更多的代码，那么将唯一的`if`块放在`else`块中也没问题。

# 没有括号的不同布尔运算符的混合

如果我们有复杂的布尔表达式，那么我们应该用括号把它们括起来，以消除我们要做的事情的歧义。

例如，如果我们有以下代码:

```
const foo = a && b || c && d;
```

这很令人困惑，因为我们不知道如何将表达式中的运算分隔开，并组合成一个表达式。

相反，我们应该编写带括号的布尔表达式，如下所示:

```
const foo = (a && b) || (c && d);
```

在上面的代码中，我们知道先计算`a && b`和`c && d`。然后使用`||`操作符将每个返回的结果组合起来。

这也适用于与其他布尔运算符结合使用的三元运算符。

例如，如果我们有以下代码:

```
const foo = a && b ? 'foo' : 'bar';
```

然后很难找到布尔表达式的结束和三元运算的开始。

为了使上面的代码更清楚，我们应该将布尔表达式放在括号内，如下所示:

```
const foo = (a && b) ? 'foo' : 'bar';
```

# 没有混合空格和制表符缩进

混合空格和制表符是不好的。当我们在不同的操作系统中更改文件时，它会使文本编辑器混乱，并可能产生问题。

空格字符更好，因为它们在大多数操作系统上都是一样的。

另一方面，选项卡可能在不同的操作系统中呈现不同的效果。

有时制表符是 4 个空格，有时是 2 个空格，这取决于文本编辑器或操作系统。

因此，我们应该坚持在每个地方缩进两个空格，这样我们在不同的操作系统中使用文件就不会有问题。

如果我们想节省打字，我们可以在许多文本编辑器中自动将制表符改为空格。然后，当我们按 tab 键时，如果我们将其配置为将制表符更改为 2 个空格，我们会得到 2 个空格。

# 不要链接赋值表达式

对于许多人来说，赋值表达式链接是 JavaScript 的一个令人困惑的特性。

例如，如果我们有下面的赋值链:

```
const a = b = 0;
b = 1;
```

然后我们得到 0 和 1，因为`b`不是常数。正如我们所看到的，代码是欺骗性的。

因此，在一行中编写多个赋值表达式的便利性并没有超过它所带来的混乱。

我们要么把赋值表达式写在不同的行里，要么用逗号写多个赋值。

例如，如果我们有以下代码:

```
const a = 0,
  b = 0;
```

然后`a`和`b`都被声明为常量。

即使我们将它们放在多行中，它仍然是好的，因为它使代码更加清晰:

```
const a = 0;
const b = 0;
```

在上面的代码中，我们知道`a`和`b`都是常量，因为有了`const`关键字。

所以要分开申报。

![](img/bd15bfadcdd649f61b966d6baf3f7698.png)

Photo by [JJ Ying](https://unsplash.com/@jjying?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

如果我们在一个`else`块中只有一个`if`块，那么我们应该把它们重写为一个`else if`块。

如果我们有一个布尔表达式，那么我们应该用括号把它们分开，这样我们就知道先计算哪些运算。

混合空格和制表符进行缩进是不好的，因为制表符在不同的操作系统或文本编辑器中可能看起来不同。为了保持缩进的一致性，我们应该使用两个空格。

多重赋值表达式令人困惑，因为它们看起来具有欺骗性。例如，常量没有被声明为常量。

# **简明英语团队的说明**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意吧:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)，[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**我们还推出了一个 YouTube，希望你们支持我们，通过 [**订阅我们的简明英语频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)**

**一如既往,“简明英语”希望帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****