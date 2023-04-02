# JavaScript 最佳实践—解析数字、承诺、Unicode 和 Regex

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-parsing-numbers-promises-unicode-and-regex-a608dc8df8?source=collection_archive---------9----------------------->

![](img/a3ebf86da3bab5624d000705ecdbdca0.png)

Photo by [Alex Makarov](https://unsplash.com/@alex_makarov?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。编写运行但有错误的代码很容易。

在本文中，我们将研究更好地解析数字的方法，在异步函数中要求`await`，并在正则表达式中添加一个 Unicode 标志。

# 调用解析时添加基数参数

JavaScript `parseInt`函数接受第二个参数，该参数具有被解析数字的基数。基数是数字的`0x`前缀。

在 ES5 之前，`parsseInt`自动检测八进制文字，这使得任何带有 0 的内容在被解析为八进制数而不是十进制数之前都是 0。

然而，ES5 或更高版本改变了这种行为，现在前导为 0 的数字被解析为十进制数。

但是显式地指定`parseInt`函数仍然是一个好主意，这样每个人都知道我们要把数字串解析成什么样的数字。

例如，不是编写以下代码:

```
const num = parseInt("021");
```

我们应该编写以下代码:

```
const num = parseInt("021", 10);
```

第二个参数中的`10`确保`parseInt`将`'021'`解析为十进制数，这样它或任何开发人员都不会与它被解析的内容相混淆。

因此`num`为 21。

我们还可以指定它被解析为八进制文字，如下所示:

```
const num = parseInt("021", 8);
```

那么`num`是 17，因为八进制`021`是 17 作为十进制数。

# 没有没有`await`表达式的异步函数

在 JavaScript 中，`async`函数是一个内部有一系列承诺并且只返回一个承诺的函数。

没有任何`await`表达式的`async`函数只是用我们放在`return`语句之后的任何东西来返回一个承诺，或者如果我们没有返回任何里面的东西，则返回一个解析为`undefined`的承诺。

没有必要让`async`函数里面没有`await`表达式，因为我们要么没有在里面运行任何承诺代码，要么我们在承诺之前忘记了`await`，所以它们不会被正确地链接。

无论哪种方式，我们都应该纠正这些情况。如果只是同步代码，那么我们不需要把它们放在`async`函数中。

如果我们忘记了与`await`的承诺，那么我们应该把`await`放在承诺的前面。

因此，我们不应该有这样的代码:

```
const foo = async () => {
  return 'foo';
}
```

相反，我们写下这样的内容:

```
const getMichael = async () => {
  const response = await fetch('https://api.agify.io/?name=michael');
  const data = await response.json();
  return data;
}
```

上面的代码是调用 API 的异步函数，这是 promises 的常见用法。

最后，它返回从 API 获得的数据。

如果我们在异步函数中抛出一个错误，如下所示:

```
const error = async () => {
  throw new Error("error");
}
```

我们可以将上面的代码替换为:

```
const error = () => Promise.reject(new Error("error"))
```

他们都做出了被拒绝的承诺。

![](img/44281f70e77414c75381b54e2e4f8715.png)

Photo by [Stan W.](https://unsplash.com/@stanyw?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 在正则表达式上使用`u`标志

有了添加到正则表达式末尾的`u`标志，我们可以正确解析 UTF-16 字符，包括表情符号和扩展字符集中的其他字符。

它还会抛出语法错误，以便我们了解它们。如果没有`u`标志，如果正则表达式中有语法错误，就不会抛出语法错误。

因此，它让我们在早期发现错误，而不是在定义正则表达式之后运行代码。

因此，不要像下面这样编写代码:

```
const a = /foo/;
const b = new RegExp('foo');
```

我们应该这样写:

```
const a = /foo/u;
const b = new RegExp('foo', 'u');
```

# 结论

当我们调用`parseInt`时，基数参数应该被添加，这样它将总是被解析为我们想要的那种数字。

它还消除了从`parseInt`返回哪种数字时的任何模糊性。

没有`await`表达式的异步函数可能不需要在异步函数中。

应该在正则表达式文字或对象之后添加`u`标志，这样我们就可以更早地发现正则表达式的语法错误，并正确检查 UTF-16 字符集中的字符。

# **简明英语团队的笔记**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**我们还推出了一个 YouTube，希望你能通过 [**订阅我们的简明英语频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) 来支持我们**

**一如既往,“简明英语”希望帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****