# 可维护的 JavaScript —未定义的、数组和对象

> 原文：<https://javascript.plainenglish.io/maintainable-javascript-undefined-arrays-and-objects-58e46d7a79c9?source=collection_archive---------3----------------------->

![](img/346794bec42806963109087b7af787e7.png)

Photo by [Vivint Solar](https://unsplash.com/@vivintsolar?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果想继续使用代码，创建可维护的 JavaScript 代码很重要。

在本文中，我们将通过一些`undefined`惯例来看看创建可维护的 JavaScript 代码的基础。

# 不明确的

`undefined`是一个经常与`null`混淆的值。

这部分是因为`null == undefined`返回`true`。

然而，它们实际上彼此非常不同。

没有赋值的变量有初始值`undefined`。

这意味着它在等待一个实值被赋给它。

如果我们有:

```
let animal;
console.log(animal === undefined);
```

然后控制台日志会记录`true`。

在我们的代码中不应该使用太多。

但是我们需要检查它们，以便避免各种运行时错误。

我们经常会得到`undefined`的东西。

不存在的属性有值`undefined`。

没有传入参数的参数也是`undefined`。

如果我们试图对`undefined`做些什么，那么我们会得到一个错误。

因此，我们需要对它们进行检查。

为了检查`undefined`，我们可以使用`typeof`操作符。

如果某事为`undefined`，`typeof`将返回`'undefined'`。

例如，我们可以写:

```
let animal;
console.log(typeof animal);
```

然后我们得到`'undefined’`日志。

我们不应该用它来布置作业，但我们应该检查它们。

# 对象文字

对象文字是用一组属性创建对象的一种流行方式。

它比使用`Object`构造函数要短，但却做了同样的事情。

因此，我们应该使用对象文字符号来创建对象。

例如，不要写:

```
let book = new Object();
book.title = "javascript for beginners";
book.author = "jane smith";
```

我们应该写:

```
let book = {
  title: "javascript for beginners",
  author: "jane smith"
}
```

它更短更干净。

我们只是在花括号之间指定所有的属性和值。

我们在第一行包含了开始的花括号。

属性缩进一级。

右大括号自成一行。

大多数风格指南和短绒建议这种格式。

像谷歌风格指南，Airbnb，风格指南，ESLint 默认规则等指南。都找这种风格。

# 数组文字

像对象文字一样，我们不需要`Array`构造函数来创建数组。

相反，我们使用数组文字符号。

例如，不写:

```
let fruits = new Array("apple", "orange", "grape");
let numbers = new Array(1, 2, 3, 4);
```

我们写道:

```
let fruits = ["apple", "orange", "grape"];
let numbers = [1, 2, 3, 4];
```

它要短得多，做同样的事情。

它被广泛使用，在 JavaScript 中很常见。

`Array`构造函数也有两个版本。

如果我们传入多个参数，它将返回一个包含参数的数组。

如果只有一个参数，并且是一个非负整数，那么它创建一个数组，数组中的空槽数由参数指定。

因此，这是避免使用`Array`构造函数的另一个原因。

![](img/7b7f7fe0d37a2a9c455c7293d0f3365b.png)

Photo by [Michael Dziedzic](https://unsplash.com/@lazycreekimages?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用更好的方式处理`undefined`，对象和数组。

对于数组和对象，文字比构造函数更好。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**