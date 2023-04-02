# JavaScript 中的正则表达式:基础

> 原文：<https://javascript.plainenglish.io/regular-expressions-the-basics-2669c069d5f3?source=collection_archive---------9----------------------->

## 快速参考和复习第一部分

![](img/59681a40db97a97fd57d6b3805a015cf.png)

Photo by [Pisit Heng](https://unsplash.com/@pisitheng?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

正则表达式很有趣！它们就像自己的一种小型编程语言，可以应用于任何地方，不管使用的是什么语言。它们有广泛的用途，从验证用户输入到重命名文件，以及在页面或电子邮件中查找链接或 URL。

```
/(https?:\/\/)(www\.)?(?<domain>[-a-zA-Z0-9@:%._\+~#=]{2,256}\.[a-z]{2,6})(?<path>\/[-a-zA-Z0-9@:%_\/+.~#?&=]*)?/
```

正则表达式也令人困惑。无论你习惯哪种语言，它们看起来都像天书，对初学者来说似乎难以理解。O'Reilly 有一本关于使用正则表达式的 300 多页的书。然而，一旦你能够识别所使用的符号以及它们是如何一起工作的，它们实际上并不那么难理解。一旦你超越了表面，你就可以破译任何正则表达式，并开始构建你自己的表达式。

# 基本语法

正则表达式如此命名是因为它们是正则的(它们遵循一组特定的模式)，并且它们是文本的表达式(它们是数学表示)。为了节省空间，我将把它们称为正则表达式。让我们熟悉一下它们的样子。

识别正则表达式最简单的方法是通过两边的正斜杠`/`。几乎任何时候你看到一个正则表达式，它都会以`/some rules here/`的形式出现。例如，给定字符串“abcdefghijklmnopqrstuvwxyz”，正则表达式`/abc/`将匹配前三个字符“abc”。

然而，要创建真正强大的正则表达式，你需要一些特殊字符的帮助。这里是我之前提到的符号，叫做*元字符*:

```
/ \ | ^ $ ? + . * ( ) [ ] { }
```

这意味着当你在一个正则表达式中发现这些字符时，它们会做一些特殊的事情，也就是说，正则表达式中的普通`$`并不是指金额。我们已经看到了`/`(它代表正则表达式的开始和结束)，我们将在本教程的课程中查看其余部分。下一个最重要的是反斜杠`\`。

# 逃脱

你可能会问，“如果美元符号`$`不是美元符号，如果我想匹配一个实际的美元符号，我该怎么做！?"这就是反斜杠出现的地方；它的第一个功能是*转义*特殊字符。因此正则表达式`/\$/`将匹配$字符，`/\*/`将匹配*字符，依此类推。

```
\/ \\ \| \^ \$ \? \+ \. \* \( \) \[ \] \{ \}
```

反斜线元字符的另一种用法基本上是相反的。除了获取一个特殊字符并使其成为字面字符，您还可以获取一个字面字符(至少是其中的一部分)并将其转换为特殊字符。这些问题的列表相当短，我们将在后面讨论它们。

```
\b \B \c \d \D \f \n \r \s \S \t \u \v \w \W \x \#
```

# 旗帜

另一件容易混淆的事情是*标记了可以在 regex 中使用的*。这些是在结束斜线`/`后面的字母，允许你使用一些高级功能。这些是`g`、`i`、`m`、`s`、`u`和`y`，但是你经常会遇到的只有`g`和`i`。

表示全局搜索，意味着正则表达式将匹配字符串中所有可能的模式，而不仅仅是返回找到的第一个模式。例如，给定字符串“do re mi fa so la ti do”，`/\w+a/`将匹配“fa”，但`/\w+a/g`将返回匹配“fa”和“la”。

`i`表示搜索不区分大小写，或者*不区分大小写*。例如，给定字符串“Adam”，`/a/`将匹配小写字母“A”，但`/a/i`将匹配第一个大写字母“A”。

![](img/8e00be2b24efdda6e776941dc5490b3f.png)

Photo by [Kara Eads](https://unsplash.com/@karaeads?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

正则表达式的真正威力不是在匹配特定内容时发挥作用，而是在匹配非特定内容时发挥作用。更准确地说，当您在寻找一系列可能性时，字符串共享一些一般结构，但实际内容可能完全不同。我们将在[第二部分:括号](https://medium.com/@adam.sultanov/regular-expressions-brackets-f2d6f69ffe13)及以后的文章中对此进行探讨。感谢阅读！

[第三部分:运算符](https://medium.com/javascript-in-plain-english/regular-expressions-operators-dbc98efaf6a9) ~ [第四部分:示例](https://medium.com/@adam.sultanov/regular-expressions-putting-it-all-together-a3fc4ca2923f)