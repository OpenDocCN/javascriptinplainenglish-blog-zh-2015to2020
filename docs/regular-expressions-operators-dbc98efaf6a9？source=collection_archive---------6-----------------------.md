# 正则表达式:运算符

> 原文：<https://javascript.plainenglish.io/regular-expressions-operators-dbc98efaf6a9?source=collection_archive---------6----------------------->

## 快速参考和复习第三部分

![](img/63ada4ab65517961d78ad8449fe33ad9.png)

Photo by [Agence Olloweb](https://unsplash.com/@olloweb?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在本教程中，我们将涵盖几乎所有的*元字符*，不包括括号字符`[]`、`{}`和`()`。以下是清单上的内容:

```
- | ^ $ ? * + .
```

# -

首先，由于我们刚刚脱离括号，让我提一下破折号。在方括号之间，你可以用破折号连接任意两个字符来创建一个*范围*，比如`[a-z]`或`[0-9]`。这些必须在它们的字符类中连接，换句话说，字母与字母，数字与数字，`[a-9]`将抛出一个语法错误。

方便的是，对于您可能想要使用的常见范围，有一些快捷方式。

`\d`匹配任何数字(number)，`\D`匹配除以外的任何*数字。`\w`匹配*字字符*。在 JavaScript 中，这意味着任何字母、任何数字和下划线`_`。同样，`\W`匹配除单词字符以外的任何内容。*

`[0-9]` = `[\d]`，`[^0-9]` = `[\D]`

`[a-zA-Z0-9_]` = `[\w]`，`[^a-zA-Z0-9_]` = `[\W]`

节省打字的好方法。

## |

这是一个烟斗。如果你有一些编程经验，你可能会认出它是 OR 操作符，它在 regex 中有相同的功能。

```
re = /blueberry|raspberry/
'kiwi, strawberry, raspberry, banana, blueberry'.match(re) // -> returns 'raspberry'
```

请记住，这里的顺序很重要，选项是按优先级匹配的。

```
/a*|b/.exec('b') // -> matches ''
/b|a*/.exec('b') // -> matches 'b're1 = /(apple)*|(orange)/    // * = the preceding can occur 0+ times
re2 = /(orange)|(apple*)/
str = 'orange'str.match(re1)   // -> matches the empty string '' 
str.match(re1)   // -> matches 'orange'
```

## ^和美元

`^`和`$`分别用于将我们想要的匹配锁定到输入字符串的开头或结尾。例如:

```
re = /Batman$/'Batman and Robin'.match(re) // -> no match
'Batman'.match(re) // -> matches 'Batman
```

脱字符号`^`与方括号`[`和`]`一起还有另一个特殊用途。记住方括号之间的任何内容都构成了一个潜在的匹配字符集。`^` *对方括号之间的任何内容求反，因此您可以搜索除您列出的字符之外的任何字符。*

```
re = /[^abcdef]/
str1 = 'faded'
str2 = 'bright'str1.match(re) // -> no match
str2.match(re) // -> matches 'r'
```

## *、+和？

`*`、`+`、`?`用于指定一个特定的模式在字符串中可以出现多少次，类似于花括号`{`和`}`。这些符号直接放在它们应该修改的图案之后。`?`表示某件事可以出现 0 次或 1 次，本质上只是使其成为一个可选的标准。

```
re = /a?mb?/
str1 = 'hamburger'
str2 = 'ham sandwich'
str3 = 'ember'str1.match(re) // -> matches 'amb'
str2.match(re) // -> matches 'am'
str2.match(re) // -> matches 'mb'
```

`*`用于表示一个模式可以出现 0 次或多次，类似地，`+`表示 1 次或多次。你可以把它们想象成`{0,}`和`{1,}`。

```
re1 = /a*/
re2 = /o+/
str1 = 'ham'
str2 = 'hum'
str3 = 'haaaaaaaam'
str4 = 'props'
str5 = 'preps'str1.match(re1) // -> matches 'a'
str2.match(re1) // -> matches the empty string ''
str3.match(re1) // -> matches 'aaaaaaaa'
str4.match(re2) // -> matches 'o'
str5.match(re2) // -> no match
```

`?`也有几个其他功能。我在上一篇文章中提到的`?:`，可以用在圆括号中创建非捕获匹配。`?`也可以跟在任何其他重复规则之后来改变它们的运作方式。通常这些规则被称为*贪婪的*，也就是说，它们将匹配尽可能多的有效字符，但是随着后面的`?`变成非贪婪的，将匹配尽可能少的字符。

`x*?, x+?, x??, x{n}?, x{n,}?, x{n,m}?`

```
re = /\d+/
re1 = /\d+?/
str = '12345'str.match(re) // -> matches '12345'
str.match(re1) // -> matches '1'
```

## 。和快捷方式

句点`.`是通配符。它将匹配除换行符`'\n'`之外的任何内容。其中一个可用的标志是`s`，它允许句点匹配一个换行符。

```
re = /.s/
str = 'sick as a dog.'str.match(re) // -> matches 'as' but not 'sick'
```

今天到此为止。感谢阅读！

[第一部分:基础知识](https://medium.com/javascript-in-plain-english/regular-expressions-the-basics-2669c069d5f3) ~ [第二部分:括号](https://medium.com/javascript-in-plain-english/regular-expressions-brackets-f2d6f69ffe13) ~你在这里~ [第四部分:例题](https://medium.com/@adam.sultanov/regular-expressions-putting-it-all-together-a3fc4ca2923f)