# 正则表达式:括号

> 原文：<https://javascript.plainenglish.io/regular-expressions-brackets-f2d6f69ffe13?source=collection_archive---------2----------------------->

## 快速参考和复习第二部分

![](img/846fb8c96fa3ca8a26ca4a196a02ccb6.png)

Photo by [Maksym Ivashchenko](https://unsplash.com/@maksymiv?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在这张照片中找到针。

# JavaScript 中的正则表达式

因为我在代码示例中使用了 JavaScript，所以我认为快速概述一下正则表达式在这种语言中是如何工作的是合适的。在 JavaScript 中，正则表达式是类型为`RegExp`的对象。因此，它们有一个对象原型和相关的属性和方法。这也意味着你可能会遇到他们，要么是字面上的*要么是*构造者*。正则表达式文字正是我们一直在使用的，`/slashes with rules inside/`而构造函数看起来像这样:*

```
new RegExp(/rules go here/, 'optional flags');
```

正则表达式中可能用到的一些常见的字符串方法有`match`、`replace`和`split`。第一个采用的形式是`str.match(regex)`并返回一个匹配数组，如果没有找到匹配，则返回`null`。下一个类似于`str.replace(regex, repl)`，它返回一个新的字符串，其中的`repl`已经替换了正则表达式匹配的内容。最后，`split`用于将一个字符串分解成一个数组，通常只给出一个分隔符，比如空格或逗号，但是也可以使用正则表达式。

# 括号

在这一部分，我们将只深入研究一组符号，即括号。

## [ ]

方括号表示要匹配的一组字符。括号中的任何单个字符都将匹配，您还可以使用连字符来定义一个集合。

```
'elephant'.match(/[abcd]/) // -> matches 'a'
```

您可以使用`^`元字符对括号之间的内容求反。

`'donkey'.match(/[^abcd]/) // -> matches 'o'`

你会经常看到字母表或所有数字的范围。`[A-Za-z]` `[0-9]`记住这些字符集是区分大小写的，除非你设置了`i`标志。

```
'elephant'.match(/[a-d]/) // -> matches 'a'
'elephant'.match(/[A-D]/) // -> no match
'elephant'.match(/[A-D]/i) // -> matches 'a'
```

## { }

花括号用于指定要匹配的确切数量。它们用在一个表达式之后:`\na{2}\`将只匹配' na '两次。

```
'panama'.match(/na{2}/) // -> no match
'banana'.match(/na{2}/) // -> matches 'nana'
```

您可以将它们与逗号结合使用来指定多个数量。`{2,}` =两次以上，`{2,4}` =两到四次之间。

```
'banana'.match(/a{2,4}/) // -> no match
'bananaa'.match(/a{2,4}/) // -> matches 'aa'
'bananaaa'.match(/a{2,4}/) // -> matches 'aaa'
'bananaaaa'.match(/a{2,4}/) // -> matches 'aaaa'
'bananaaaaaaaaaaa'.match(/a{2,4}/) // -> matches 'aaaa'
```

## ( )

括号代表记忆的匹配。这对于查找和替换操作或者任何需要对部分匹配做些什么的时候特别有用。当一场比赛被记住时，你可以用`$n`来指代它，从`$1`开始一直到`$9`，或者用`$&`来指代整场比赛。

```
'Firsty McLastname'.match(/([A-Za-z]+)\s([A-Za-z]+)/) // -> matches 'First McLastname' with 'Firsty' remembered as $1 and 'McLastname' as $2'Firsty McLastname'.replace(/([A-Za-z]+)\s([A-Za-z]+)/, '$1') // -> returns 'Firsty''Firsty McLastname'.replace(/([A-Za-z]+)\s([A-Za-z]+)/, '$2, $1') // -> returns 'McLastname, Firsty''Firstipher Lasterman'.replace(/([A-Za-z]+)\s([A-Za-z]+)/, '$&') // -> returns 'Firstipher Lasterman'
```

除了使用数字，你还可以用语法`(?<myGroupName>...)`给匹配的组命名。

```
'Firsty McLastname'.replace(/(?<first>[A-Za-z]+)\s(?<last>[A-Za-z]+)/, '$<last>, $<first>') // -> returns 'McLastname, Firsty'
```

如果您返回匹配，这些也将显示为`matches.groups.myGroupName`。

正则表达式中也使用括号将表达式的各个部分组合成子组，就像在`/(\$|¥)([0-9]+)\.([0-9]{2})/`中一样。这将匹配“$149.99”，并记住子组“$”、“149”和“99”。但是如果你不想记住美元符号呢？可以用`?:`。

```
'¥2000.50'.match(/(?:\$|¥)([0-9]+)\.([0-9]{2})/) // --> matches '¥2000.50' but only remembers '2000' as $1 and '50' as $2
```

最后一个使用`split`的例子，您可以拆分一个字符串，但也可以返回匹配的结果:

```
'Howard the Duck'.split(/the/) 
// -> returns ['Howard ', ' Duck']'Howard the Duck'.split(/(the)/) 
// -> returns ['Howard ', 'the', ' Duck']
```

![](img/8b6e1862ddd2e017cbce71fc5d41c228.png)

Photo by [Boris Smokrovic](https://unsplash.com/@borisworkshop?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

请继续关注更多的正则表达式的乐趣。

[第 1 部分:基础知识](https://medium.com/javascript-in-plain-english/regular-expressions-the-basics-2669c069d5f3) ~ *你在这里* ~ [第 3 部分:运算符](https://medium.com/javascript-in-plain-english/regular-expressions-operators-dbc98efaf6a9) ~ [第 4 部分:示例](https://medium.com/@adam.sultanov/regular-expressions-putting-it-all-together-a3fc4ca2923f)

## 进一步阅读

[](/best-tool-for-web-scraping-beautifulsoup-vs-regex-vs-advanced-web-scrapers-50b8fb92950d) [## 最佳网络抓取工具:beautiful soup vs . Regex vs . Advanced Web Scrapers

### BeautifulSoup、正则表达式或高级 web scraper——哪一个是 web 抓取的最佳工具？深潜…

javascript.plainenglish.io](/best-tool-for-web-scraping-beautifulsoup-vs-regex-vs-advanced-web-scrapers-50b8fb92950d) 

*更多内容请看*[***plain English . io***](https://plainenglish.io/)*。报名参加我们的* [***免费周报***](http://newsletter.plainenglish.io/) *。关注我们关于*[***Twitter***](https://twitter.com/inPlainEngHQ)[***LinkedIn***](https://www.linkedin.com/company/inplainenglish/)*[***YouTube***](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)*[***不和***](https://discord.gg/GtDtUAvyhW) ***。*****

*****对缩放您的软件启动感兴趣*** *？检查* [***电路***](https://circuit.ooo?utm=publication-post-cta) *。***