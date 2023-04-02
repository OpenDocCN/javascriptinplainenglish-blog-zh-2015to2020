# 理解 JavaScript 中的正则表达式

> 原文：<https://javascript.plainenglish.io/understanding-regular-expressions-in-javascript-36f86bfc756?source=collection_archive---------7----------------------->

## 通过实例学习 JavaScript 正则表达式

![](img/9ec35d58cf369b1cace6f3a117763450.png)

Photo by [Tudor Baciu](https://unsplash.com/@baciutudor?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 什么是 JavaScript 中的正则表达式？

正则表达式是表示搜索模式的特殊字符串。也称为“regex”或“regexp”，它们帮助程序员匹配、搜索和替换文本。正则表达式是用于匹配字符串中字符组合的模式。在 JavaScript 中，正则表达式也是对象。目标是将符号和文本组合成一个模式，只匹配你想要的。

在本文中，我们将探索一些字符、一些快捷方式以及编写正则表达式的常见用法。

![](img/e23657485a949b4e5f76a54c4e087872.png)

Image Created with ❤️ ️By Mehdi Aoussiad.

# 使用测试方法

正则表达式在编程语言中用来匹配字符串的各个部分。

假设您想在字符串`**"hello world”**` 中找到单词`**hello**` ，您可以使用以下正则表达式:`**/hello/**`。JavaScript 有多种使用 regexes 的方式。测试正则表达式的一种方法是使用方法`**.test()**`。如果您的模式发现或没有发现什么，该方法返回`**true**`或`**false**`。请看下面的例子:

```
let testStr = "hello world";
let testRegex = /hello/;
testRegex.test(testStr);
// Returns true
```

如您所见，它返回 true，因为正则表达式在我们的字符串中可用。

# 匹配文字字符串

在前面的例子中，我们使用测试方法来搜索字符串中的单词。下面是另一个搜索字符串文字匹配的例子`"Kevin"`:

```
let testStr = "Hello, my name is Kevin.";
let testRegex = /Kevin/;
testRegex.test(testStr);
// Returns true
```

请注意，任何其他形式的字符串`"**K**evin"`都不匹配。例如，regex `/**K**evin/`与`"kevin"`或`"**KEVIN**"`不匹配。那将是错误的。

```
let wrongRegex = /KEVIN/;
wrongRegex.test(testStr);
// Returns false
```

# 匹配时忽略大小写

正如您在最后一个例子中看到的，为了匹配文字字符串，我们应该有相同的字母大小写(大写和小写)。但有时，您可能还需要匹配大小写差异。您可以使用所谓的标志来匹配这两种情况。还有其他的标志，但是在这里你会把注意力集中在忽略这个情况的标志上-`**i**`标志。您可以通过将它附加到正则表达式的末尾来使用它。

```
let testStr = "Hello, my name is KEVIN.";
let testRegex = /Kevin/**i**; // the i flag.
testRegex.test(testStr);
// Returns true
```

如您所见，即使我们有案例差异( *KEVIN & Kevin* )，它也会返回真，因为我们使用了`**i**`标志。

# 提取匹配项

您还可以使用方法`**.match()**`提取您找到的实际匹配。这里有一个例子:

```
"Hello, World!".match(/Hello/);
// Returns ["Hello"]
let ourStr = "Regular expressions";
let ourRegex = /expressions/;
ourStr.match(ourRegex);
// Returns ["expressions"]
```

注意语法`.match`与你一直使用的方法`.test`相反:

```
'string'.match(/regex/);
/regex/.test('string');
```

# 查找比第一个匹配更多的内容

我们可以在一个字符串中找到多个匹配。为了实现这一点，我们需要使用`**g**` 标志，将它附加在正则表达式的末尾。看看下面的例子:

```
let testStr = "Repeat, Repeat, Repeat";
let ourRegex = /Repeat/;
testStr.match(ourRegex);
// Returns ["Repeat"]
```

使用`**g**`旗:

```
let repeatRegex = /Repeat/g;
testStr.match(repeatRegex);
// Returns ["Repeat", "Repeat", "Repeat"]
```

如您所见，我们可以使用`**g**`标志不止一次地搜索或提取一个模式。

# 匹配字母表中的字母

您还可以指定在正则表达式中匹配字母表中的字母。例如，要匹配小写字母`a`到`e`，您可以使用`**[a-e]**`。

```
let catStr = "cat";
let batStr = "bat";
let matStr = "mat";
let bgRegex = /[a-e]at/;
catStr.match(bgRegex); // Returns ["cat"]
batStr.match(bgRegex); // Returns ["bat"]
matStr.match(bgRegex); // Returns null
```

# 匹配所有数字

寻找数字字符的快捷键是`**\d**`，带小写`**d**`。这相当于字符类[0–9]，它查找 0 到 9 之间任意数字的单个字符。看看下面的例子:

```
let movieName = "2001: A Space Odyssey";let numRegex = /\d/g;movieName.match(numRegex).length; // Returns 4.
// because the movieName string contains 4 digit characters.
```

# 匹配空白

您可以使用小写字母`s`的\s 来搜索空白。该模式不仅匹配空白，还匹配回车、制表符、换页符和换行符。

```
let whiteSpace = "Whitespace. Whitespace everywhere!"
let spaceRegex = /\s/g;
whiteSpace.match(spaceRegex);
// Returns [" ", " "]
// Returns the whitespace in the string.
```

如您所见，它返回了`whiteSpace`字符串的空白。

# 结论

如您所见，正则表达式在 JavaScript 中非常有用和重要。您可以将它们用于许多任务，例如表单和密码验证。确保我们没有涵盖正则表达式的所有内容，您将需要从其他资源中了解更多关于它们的内容。

感谢您阅读本文，希望您觉得有用。