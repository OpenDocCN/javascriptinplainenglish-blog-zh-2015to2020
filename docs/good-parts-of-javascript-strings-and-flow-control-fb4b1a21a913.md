# JavaScript 的好部分——字符串和流控制

> 原文：<https://javascript.plainenglish.io/good-parts-of-javascript-strings-and-flow-control-fb4b1a21a913?source=collection_archive---------11----------------------->

![](img/13eb79e5c3cc9a3ce3967d5c45fb8267.png)

DFPhoto by [Kira auf der Heide](https://unsplash.com/@kadh?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是世界上最流行的编程语言之一。它可以做很多事情，并且有一些领先于许多其他语言的特性。

在本文中，我们将研究 JavaScript 的一些好的部分，包括字符串和流控制。

# 用线串

字符串可以用单引号、双引号或勾号括起来。

它可以包含 0 个或多个字符。`\`(反斜杠)是转义字符。

JavaScript 没有字符类型。我们只是把一个字符放在一个字符串中来代表一个字符。

单引号或双引号很适合用来分隔没有任何动态部分的短字符串。

Backticks 用于分隔模板字符串，模板字符串可以包含表达式，也可以是多行。

因此，只要可能，我们就应该使用模板字符串。

它比使用常规字符串方便得多。

我们也可以在模板字符串中使用单引号或双引号作为普通字符，而不是用它来分隔字符串。

例如，我们可以用以下方式定义字符串。我们可以用单引号定义一个字符串:

```
const foo = 'foo';
```

要用双引号定义字符串，我们可以写:

```
const foo = "foo";
```

或者我们可以通过编写以下内容来定义模板字符串:

```
const world = 'world';
const greeting = `hello ${world}`;
```

模板字符串可以包含表达式，表达式也可以包含。

表达式被`${}`字符包围。

这很好，因为动态字符串代替了串联，如果我们必须创建动态字符串，这是很难使用的。

我们可以轻松创建多行字符串，如下所示:

```
const hello = `
hello
world
`
```

现在`'hello'`和`'world'`将会分开。

我们也可以使用模板标签通过传递模板字符串来返回值。

模板标签只是具有特殊签名的函数。

模板标签的第一个参数是字符串数组，其余参数是表达式。

如果我们写道:

```
const tag = (strings, ...exps)=>{
 console.log(strings, exps);
}const name = 'world';
tag`hello ${name}`;
```

我们得到`strings`是`[“hello “, “”, raw: Array(2)]``exps`是一系列的表达式，也就是`[“world”]`。

所有字符串都有`length`属性。它返回字符串的长度。

JavaScript 字符串是不可变的。一旦它被制造出来，就永远不会被改变。

但是我们可以通过串联或者将它们放入模板字符串来创建新的字符串。

2 长度相同、字符顺序完全相同的字符串被认为是相同的。

所以:

```
'foo' + 'bar' === 'foobar';
```

返回`true`。

JavaScript 字符串也有许多我们可以用来操作它们的方法。

![](img/4dc1ad71318113f86ce647788ee36b8e.png)

Photo by [Samuel Ferrara](https://unsplash.com/@samferrara?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 声明

JavaScript 程序由一条或多条语句组成。

如果我们只使用脚本标签，那么所有的东西都会被放入全局命名空间。

但是，随着 JavaScript 模块的引入，模块可以捆绑在一起，编译成一个大的包。

`let`或`var`可以用来声明变量。如果在函数中声明，它们就是私有的。

`let`亦为私人街区内。

`const`定义一个常量，也是块范围的。

常数是可变的，但是我们不能给它们赋值。

`switch`、`while`、`for`和`do`语句允许有一个可选的`label`语句，可以和`break`语句一起使用。

语句通常从上到下运行。

可以用条件语句`if`和`switch`改变顺序。

循环语句，`while`，`for`，`do`也可以改变语句的执行顺序。

像`break`、`return`和`throw`这样的破坏性语句也可以改变执行流程。

函数调用也可以改变程序的流程。

一个块用花括号括起来。块为用`let`或`const`声明的任何数据创建一个新的范围。

因此，我们最好使用`let`或`const`来声明我们的变量和常量。

一条`if`语句根据括号内表达式的值改变程序的流程。

如果返回`true`，则运行`if`块中的代码。

否则，将检查`else if`的条件，如果可能，将运行这些程序块中的代码。

如果一个`else`程序块存在，那么如果在它之前的其他程序块没有运行，它就会运行。

# 结论

模板字符串很棒。我们可以有表达式，它们可以不止一行。

语句是程序的组成部分。我们可以运行基于各种流控制语句的流变化。

## **用简单英语写的 JavaScript**

通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达爱意吧！**