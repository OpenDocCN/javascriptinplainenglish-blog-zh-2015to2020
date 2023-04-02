# JavaScript 最佳实践—字符串和函数

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-strings-and-functions-1dc905de6a55?source=collection_archive---------6----------------------->

![](img/38ffeca7f389a9f48401b318bb700247.png)

Photo by [Gábor Szűts](https://unsplash.com/@szutsi?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。编写可以运行但有错误的代码很容易。

在本文中，我们将研究使用模板字符串和定义函数的最佳方式。

# 使用模板字符串

我们应该尽可能使用模板字符串。使用它们有很多好处。

我们可以将 JavaScript 表达式放在字符串中，我们可以保存单引号和双引号来引用字符串中的文本。

此外，它还可以用来创建多行字符串，因为我们可以通过键入来添加换行符，而不是显式地添加额外的换行符。

例如，我们可以如下使用模板字符串:

```
const name = 'jane';
const greeting = `Hi, ${name}`;
```

在上面的代码中，我们有一个模板字符串，其中插入了表达式`name`。我们通过使用`${}`作为插值表达式的分隔符来实现这一点。

插值分隔符和表达式本身之间没有任何空格。

这个间距很好，因为我们已经有了分隔符将表达式与字符串的其余部分分开，所以我们不需要在表达式和分隔符之间有更多的间距。

我们可以如下创建一个多行字符串:

```
const name = 'jane';
const greeting = `Hi, 
${name}`;
```

然后我们得到:

```
Hi, 
jane
```

作为`greeting`的价值。

正如我们所看到的，我们所要做的就是输入一个额外的换行符。我们不必键入转义的换行符来创建换行符。

模板字符串由反斜杠分隔，因此我们可以使用单引号和双引号来引用字符串中的文本。

# 使用函数表达式代替函数声明

在 JavaScript 中，有两种方法来定义函数。一个是函数表达式，另一个是函数声明。

函数声明定义如下:

```
function foo() {
  // ...
}
```

我们有名为`foo`的`function`关键字，但我们没有将它赋给变量。

函数声明被放在顶部，这样它们就可以在我们代码的任何地方被引用。

函数表达式是通过创建一个函数，然后将它赋给一个变量来定义的。

例如，我们可以创建如下函数表达式:

```
const bar = function() {
  // ...
}const baz = () => {
  //...
}
```

在上面的代码中，我们定义了传统函数和箭头函数，并将它们分别赋给了一个变量。

这些没有被提升，所以它们只能在被定义后被引用。

函数表达式更好，因为我们不必担心当我们必须考虑提升时出现的混乱。

提升不利于可读性，因为提升后的函数可以在代码中的任何地方被引用。

函数表达式也适用于所有类型的函数，而不仅仅是传统函数。

我们也可以在函数中放一个名字，但是这不是很有用，因为在它被赋值给变量后，我们不能用名字来引用它。

例如，如果我们有以下代码:

```
const bar = function foo() {
  // ...
}
```

然后我们要调用函数为`bar`而不是`foo`。因此，这个额外的名字并不那么有用。

![](img/804d137cae3ed1bf4a475b469d1abfad.png)

Photo by [David Clode](https://unsplash.com/@davidclode?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 将立即调用的函数表达式用括号括起来

立即调用的函数表达式(IIFEs)是定义后立即运行的函数。

在过去，它们对于封装数据很有用，但现在它对于创建异步函数并立即调用它们仍然很有用。

我们应该把生活用括号括起来，以确保每个人都知道这是一种生活。

例如，我们可以如下创建一个异步生命:

```
((async () => {
  const foo = await Promise.resolve(1);
  console.log(foo);
})())
```

在上面的代码中，我们将异步函数放在括号中，这样我们就可以用左括号和右括号立即调用它。

然后我们把整个表达式用括号括起来，这样每个人都知道它会立即运行。

# 结论

如果我们创建字符串，我们应该使用模板字符串。它们允许我们在字符串中插入表达式，并释放单引号和双引号来引用文本。

我们应该将函数定义为函数表达式，而不是函数声明，这样我们只能在它们被定义后调用它们。这样，阅读起来就容易多了，因为流程实际上是按顺序进行的。

生活应该用括号括起来，这样我们都知道它是一种生活。

## 简明英语笔记

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页 [**plainenglish.io**](https://plainenglish.io/) 上找到所有这些——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**