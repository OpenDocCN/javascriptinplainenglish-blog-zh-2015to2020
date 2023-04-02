# JavaScript 最佳实践— ES6 特性和正则表达式

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-es6-features-and-regex-40a1bcbe6026?source=collection_archive---------7----------------------->

![](img/edd2e5dbaa8a99a449f8d2ee69d83d15.png)

Photo by [Shane Young](https://unsplash.com/@shane_young?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。编写可以运行但有错误的代码很容易。

在本文中，我们将研究模板标记间距、包装正则表达式和箭头函数体。

# 模板标记和它们的文字之间的间距

从 ES6 开始，随着模板字符串的引入，引入了一种称为模板标签的新功能。

它们只适用于模板字符串。这是一个接受一些参数的函数，包括模板字符串本身及其组成部分。

我们通过定义一个模板文字标签来使用模板标签，用法如下:

```
const foo = (strings, ...args) => {
  console.log(strings, args);
}const a = 1;
const b = 2;
foo`foo ${a} bar ${b}`
```

在上面的代码中，我们定义了`foo`文字标签，它有一个`strings`参数，这个参数有一个静态字符串所有部分的数组。

`args`参数是一个数组，包含字符串中所有插值的值。

因此，根据控制台日志输出的`string`的值是`[“foo “, “ bar “, “”, raw: Array(3)]`，而`args`的值是 `[1, 2]`，这是我们内插到字符串中的 2 个值。

模板文字标签对于获取模板字符串的一部分，然后从中返回某些内容非常有用。

通常，我们在模板文字标记名和模板字符串本身之间没有任何空格。

正如我们在上面的代码中一样，我们有:

```
foo`foo ${a} bar ${b}`
```

在`foo`和开始的反勾号之间没有空格，所以很明显我们是在紧随其后的模板字符串上调用`foo`。

# 换行正则表达式文本

正则表达式可以被包装，这样我们就清楚我们在正则表达式上调用了一个方法。

例如，如果我们想如下调用`exec`函数:

```
const result = /foo/.exec("foo");
```

那么人们可能不知道我们实际上在它上面调用了`exec`方法。

如果我们用括号将正则表达式括起来，那么我们可以编写以下代码:

```
const result = (/foo/).exec("foo");
```

那么对于一些人来说，我们在`/foo/`正则表达式上调用`exec`可能会更清楚。

这个语法更像是一个可选的建议，而不是每个人都应该遵循的。

# 箭头函数体中的大括号

箭头函数是较短的函数，不绑定到像`this`或`arguments`这样的变量。

此外，我们不能将它们用作构造函数，也不能在其上使用`bind`、`call`或`apply`。

它还让我们以更短的方式编写函数。它的一个好处是，如果我们返回的东西和 arrow 函数的签名在同一行，那么我们就不需要关键字`return`来返回函数末尾的项目。

相反，函数末尾的内容将被返回。

对于多行箭头函数，return 语法的工作方式与任何其他函数相同。我们需要关键字`return`来返回一些东西。

例如，如果我们有以下代码:

```
const foo = () => 2;
```

然后由`foo`函数返回 2。

如果我们想返回一个对象，我们可以编写下面的代码:

```
const foo = () => ({
  a: 1,
  b: 2
});
```

在上面的代码中，我们返回用括号括起来的对象，所以当我们调用`foo`时，我们得到:

```
{
  a: 1,
  b: 2
}
```

已退回。

如果我们有一个多行函数，那么 return 语法的工作方式和其他函数一样。

例如，我们编写以下代码来返回多行函数中的内容:

```
const foo = () => {
  return {
    a: 1,
    b: 2
  }
};
```

在上面的代码中，`foo`函数的第二行有`return`语句。

如果我们调用`foo`，我们会得到与前面的`foo`函数相同的结果。

因此，对于在函数的第一行返回的函数，我们不需要大括号。不然就要加大括号。

![](img/a3694c101b5c7875e84f702d2fa903a2.png)

Photo by [Hassan Pasha](https://unsplash.com/@hpzworkz?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

正则表达式可以用括号括起来，这样我们就可以清楚地知道我们是在对它调用方法。

通常，我们不在模板标签名和模板字符串文字之间加空格，这样我们就清楚我们是在操作它。

如果箭头函数在第一行返回一些东西，它们通常没有大括号。

否则，我们需要大括号和`return`关键字来返回一些东西。

# 简明英语笔记

你知道我们推出了一个 YouTube 频道吗？我们制作的每个视频都旨在教给你一些新的东西。点击此处 查看我们，并确保订阅该频道😎