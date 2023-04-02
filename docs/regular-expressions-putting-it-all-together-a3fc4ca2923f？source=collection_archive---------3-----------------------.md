# 正则表达式:将所有内容放在一起

> 原文：<https://javascript.plainenglish.io/regular-expressions-putting-it-all-together-a3fc4ca2923f?source=collection_archive---------3----------------------->

## 正则表达式的快速参考和复习(第四部分)

![](img/09ef4f0c8f9908840715ece4fb8c17eb.png)

Photo by [Florian Klauer](https://unsplash.com/@florianklauer?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# JavaScript RegExp 方法

在第 2 部分中，我简要介绍了正则表达式是如何在 JavaScript 中作为对象实现的。我提到了*字符串*方法`match`、`replace`、`split`，但是`RegExp`对象也有自己的一些方法。

## 试验

如果您只想查看一个字符串是否通过了基于正则表达式的测试，而不关心匹配本身，那么`test`是一个很好的方法。它将简单地返回`true`或`false`。

```
re = /goose/
re.test('mongoose')  // -> returns true
```

## 高级管理人员

这类似于字符串方法`match`。它将根据被调用的`RegExp`测试一个字符串，如果不匹配就返回`null`。如果有匹配，它将返回一个数组:完全匹配和所有记住的匹配(括号中正则表达式的部分)通常像数组一样被索引，还有关键字`index`，匹配的第一个字符的索引，`groups`，一个包含任何已命名的捕获组的对象，以及`input`，原始字符串输入。

```
re1 = /(.*) house/
str = 'ginger bread house're.exec(str) // ->
   [0: "ginger bread house",
    1: "ginger bread",
    groups: undefined,
    index: 0,
    input: "ginger bread house"]re2 = /(?<spice>.*) (?<material>.*) (?<shape>house)/re2.exec(str) // ->
   [0: "ginger bread house",
    1: "ginger",
    2: "bread",
    3: "house", 
    groups: {
       material: "bread",
       shape: "house",
       spice: "ginger"
    },
    index: 0,
    input: "ginger bread house"]
```

需要记住的另一点是，`RegExp`对象也有一个`lastIndex`属性。这将始终保持为零，除非您使用`g`全局搜索标志。然后，每次使用一个`RegExp`方法后，`lastIndex`属性被更新为前一个匹配的最后一个字符之后的索引*。换句话说，就是下一次开始搜索的索引。这可能相当棘手，所以这里有一个例子:*

```
var str = "The blue sky over the bluer sea.";
var re = /blue/g;re.test(str);  // -> true
re.lastIndex;  // -> 8// T h e   **b l u e** 
// 0 1 2 3 4 5 6 7 **8**>re.test(str);  // -> true
re.lastIndex;  // -> 26// t  h  e     **b  l  u  e**  r  
// 18 19 20 21 22 23 24 25 **26**>re.test(str);  // -> false
re.lastIndex;  // -> 0
```

因为最后一次搜索是在第二个“blue”之后开始的，所以在该索引和字符串末尾之间没有匹配。

# 向前看，向后看

一些新符号:`?=` =后跟，`?!` = *非*后跟，`?<=` =前导，`?<!` = *非*前导。这些与括号一起使用。

后跟/不后跟的示例:

```
'Donkey'.match(/Donkey(?= Kong)/) // -> no match
'Donkey Kong'.match(/Donkey(?= Kong)/) // -> matches 'Donkey''1.61803'.match(/[0-9](?!\.)/) // -> matches '6' (first number *not* followed by decimal point)
```

以及“之前/之前没有”的示例:

```
'Alexander Hamilton'.match(/(?<=Brandy) Alexander/) // -> no match
'Brandy Alexander'.match(/(?<=Brandy) Alexander/) // -> matches 'Alexander''$1.25'.match(/(?<!\$)\d+\.\d{2}/) // -> no match
'1.25'.match(/(?<!\$)\d+\.\d{2}/) // -> matches '1.25'
'$10.25'.match(/(?<!\$)\d+\.\d{2}/) // -> **matches** '0.25'
```

这最后一个例子说明了为什么您必须小心—有时即使您不打算匹配，您也可以获得匹配。

# 几个更特殊的字符

通过脱字符号、Unicode 码位等引用特定字符的方式还有很多。对于这些有很多好的参考页面(比如 [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions) )，所以这里我只注意到一些非常方便的元字符。

## \s，\S

`\s`是空白元字符。它将匹配空格、制表符、换行符、换页符、不间断空格和许多模糊的空白字符。`\S`将匹配除空白字符以外的任何字符。

```
/\S*\s/.exec("Abe Lincoln") // -> matches "Abe"
```

## \b，\B

`\b`是单词边界元字符，表示一个*单词字符*和一个*非单词字符*之间的边界。在 JavaScript 中，只有 63 个单词字符:

```
a b c d e f g h i j k l m n o p q r s t u v w x y z
A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
0 1 2 3 4 5 6 7 8 9 _
```

其他所有东西，包括空格和字符串的开头和结尾都是非单词字符。

```
"".match(/\b/)  // -> no match
"x".match(/\b/)  // -> matches "" between "x" and the beginning of the string"apples and oranges".match(/.+\b/)  // -> matches "apples and oranges"
"apples and oranges".match(/.+?\b/)  // -> matches "apples"
```

请记住，单词 boundary 本身的长度为 0，因此不包括在匹配中。

# 例子

有趣的部分来了！一些真实世界的正则表达式的例子，用来匹配来自输入的数据，并附有解释。你可能想系好安全带。

**时间验证:**

12 小时时钟—

```
/^(0?[1-9]|1[0-2]):[0-5][0-9]$/
```

第一个子句匹配以下任意一个:`1 2 3 4 5 6 7 8 9 01 02 03 04 05 06 07 08 09 10 11 12`。这后面必须跟一个冒号`:`，然后第二部分匹配从`00`到`59`的任何双字符子串。`^`和`$`字符将匹配定位到字符串的开头和结尾。

24 小时时钟—

```
/^(0?[1-9]|1[0-9]|2[0-3]):[0-5][0-9]$/
```

**字符串中的重复单词:**

```
/(\b\w+\b)(?=.*\b\1\b)/
```

`(?=.*___)`是一个有趣的模式，它基本上确保了字符串中至少存在以下一项。`?=`是一个正的前瞻，所以前面的子串后面必须跟有`.*`，这意味着任何东西，任何次数，包括 0。由于`\b\w+\b`是一个被捕获的组，`\b\1\b`可以引用该匹配，并且周围的单词边界字符`\b`确保该单词再次单独出现，而不是出现在另一个单词中。

**HTML 标签:**

```
/<(\w+)./
```

这是一个伟大的。它以一种超级简单的方式巧妙地抓住了标签名。作为额外的预防措施，您可以添加`s`标志，这样点`.`也将匹配一个换行符。

**浮点文字:**

`/([+-])?\d+\.\d+([Ee]([+-])?\d+)?/`

记住`()?`意味着括号之间的任何内容都是可选的，`[]`是一个字符范围，所以`([+-])?`可以是“+”或“-”或者根本不存在。尽管`+`和`—`在单独使用时是元字符，但在方括号内它们指的是字面字符。

`\d`是一个数字，`+`表示 1 次或更多次，所以`\d+`表示任意个数字，但至少一个。这将匹配 1，或者 81823723476，甚至 0。

如果句点`.`单独放置，它将匹配任何内容(除了换行符)，但是这里我们想要匹配实际的字符，所以必须用斜杠`\.`对其进行转义。

请注意，整个后半部分位于`()?`之间，因此是可选的。这部分允许正则表达式匹配科学记数法中的数字。你有字符集`[Ee]`，所以“E”或“E”必须出现在这里。然后在这个子句中有一个嵌套组`()?`，代表另一个可选的“+”或“-”符号。最后，另一个`\d+`用于任何数量的后续数字。

All together:可选+或-，任意位数，句号，任意位数，可选子句:E 或 E，可选+或-，任意位数。咻！把它写成正则表达式要容易得多。

**验证(美国)电话号码:**

`/\d{3}[-.]\d{3}[-.]\d{4}/`

正好 3 位数，可以是“-”或“.”，正好 3 位数，可以是“-”或“.”，正好 4 位数。您可以将`—`放在方括号之间，以便与字面意思匹配，但是要小心不要意外地创建了一个范围！逃避它更安全。

```
/\d{3}[.-\/]\d{3}[.-\/]\d{4}/   
   // -> matches 555.555.5555 and 555/555/5555 but not 555-555-5555
   // .-\/ has created a range of characters between . and //\d{3}[.\-\/]\d{3}[.\-\/]\d{4}/
   // -> matches all three
```

**电子邮件:**

外面有很多这样的*。这是一个完整的兔子洞，所以我只展示我发现的几个。*

```
/^([a-zA-Z0-9._%-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,6})*$//^([0-9a-zA-Z]+[-._+&amp;])*[0-9a-zA-Z]+@([-0-9a-zA-Z]+[.])+[a-zA-Z]{2,6}$//(\w[-._\w]*\w@\w[-._\w]*\w\.\w{2,3})//^[\w-]+(?:\.[\w-]+)*@(?:[\w-]+\.)+[a-zA-Z]{2,7}$/
```

**(非常基础)密码强度:**

```
^(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,}$
```

这将确保密码长度至少为 8 个字符，并且至少包含一个数字、小写字母和大写字母。

这是一个巧妙的问题，我花了一点时间才弄明白。又有了`(?=.*___)`模式。然后是一个字符范围——`\d`一个数字，`[a-z]`一个小写字母，`[A-Z]`一个大写字母。所有这些的前一个子字符串是`^`，它只是字符串的开始。所有这些加在一起意味着这三个字符范围前面必须是字符串的开头和任何其他内容，也就是说，这个字符可以出现在任何地方。然后最后是`.{8,}$`什么的，八次以上。

**完整网址:**

从这个系列的开始。

```
/(https?:\/\/)(www\.)?(?<domain>[-a-zA-Z0-9@:%._\+~#=]{2,256}\.[a-z]{2,6})(?<path>\/[-a-zA-Z0-9@:%_\/+.~#?&=]*)?/
```

匹配“http”或“https”，可选“www”，括号中的任何字符都是域名中的有效字符，其长度可以在 2 到 256 个字符之间，唯一的警告是它必须以句点结尾，然后是 2 到 6 个字符。这将被保存为组`domain`，之后的任何内容都将被保存为组`path`，只要它只包含有效字符。

# 有用的工具

一些有助于理解您在野外遇到的正则表达式的工具，以及制定自己的正则表达式的工具。

[regex101](https://regex101.com/) 一个 regex 测试仪

regex 可视化工具

一些基本的测试来确保你掌握了基础知识。

非常感谢你的阅读！

[第一部分:基础知识](https://medium.com/javascript-in-plain-english/regular-expressions-the-basics-2669c069d5f3) ~ [第二部分:括号](https://medium.com/javascript-in-plain-english/regular-expressions-brackets-f2d6f69ffe13) ~ [第三部分:运算符](https://medium.com/javascript-in-plain-english/regular-expressions-operators-dbc98efaf6a9) ~ *你在这里*