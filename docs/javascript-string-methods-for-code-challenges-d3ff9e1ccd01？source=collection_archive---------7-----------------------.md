# 用于代码挑战的 JavaScript 字符串方法

> 原文：<https://javascript.plainenglish.io/javascript-string-methods-for-code-challenges-d3ff9e1ccd01?source=collection_archive---------7----------------------->

![](img/1dcf79b7defca075095a2db98377298e.png)

Get it? STRING lights?

除了数组，操纵字符串是代码挑战中的一个热门话题。这里有一个字符串方法的列表，可以在你下一次技术面试/作业中帮助你😊

*   String.length →查找字符串中的字符数。

```
let str = "Coding is the LOVE of my LIFE!"
str.length = 30
```

*   String.toLowerCase() →将所有字符转换为小写

```
let str = "Coding is the LOVE of my LIFE."
str.toLowerCase() = coding is the love of my life!
```

*   String.toUpperCase() →将所有字符转为大写

```
let str = "Coding is the LOVE of my LIFE!"
str.toUpperCase() = CODING IS THE LOVE OF MY LIFE!
```

*   String.split() →将一个字符串拆分为一个子字符串数组

```
let str = "Coding is the LOVE of my LIFE!"str.split("") = [ 'C','o','d','i','n','g',' ','i','s',' ','t','h','e',' ', 'L','O','V','E',' ','o','f',' ','m','y',' ','L','I','F','E','!' ]// separates string into individual charactersstr.split(" ") = [ 'Coding', 'is', 'the', 'LOVE', 'of', 'my', 'LIFE!' ]// separates string into individual words
```

*   String.substr() →从指定位置的字符开始提取字符串的一部分，并返回指定数量的字符..接受两个参数:index 和可选的要提取的字符数。

```
let str = "Coding is the LOVE of my LIFE!"
str.substr(1) = oding is the LOVE of my LIFE!// if no second parameter is given, whole rest of string is returnedstr.substr(1,4) = odin
```

*   String.substring() →类似于上面的 substr()方法，只是返回的字符串中不包含结束索引。

```
let str = "Coding is the LOVE of my LIFE!"
str.substring(1,4) = odi
```

*   String.charAt() →返回指定索引处的字符

```
let str = "Coding is the LOVE of my LIFE!"
str.charAt(2) = d
```

*   String.concat() →连接两个或多个字符串

```
let str = "Coding is the LOVE of my LIFE! "
let str1 = "All I need is JavaScript."str.concat(str1) = Coding is the LOVE of my LIFE! All I need is JavaScript.
```

*   String.indexOf() →返回指定值在字符串中第一次出现的索引。接受要搜索的字符串的参数。

```
let str = "Coding is the LOVE of my LIFE!"
str.indexOf("LOVE") = 14
```

*   String.search() →在字符串中搜索特定值或正则表达式，并返回其索引位置。接受字符串值或正则表达式作为参数

```
let str = "Coding is the LOVE of my LIFE!"
str.search("is") = 7
```

*   String.slice() →提取字符串的一部分，并在新字符串中返回提取的部分。接受两个参数:开始和结束索引，以指定要提取的字符串部分

```
let str = "Coding is the LOVE of my LIFE!"
str.slice(1, 4) = odi
```

*   String.includes() →检查字符串是否包含指定字符串的字符

```
let str = "Coding is the LOVE of my LIFE!"
str.includes("OVE") = true
```

*   String.startsWith() →检查字符串是否以指定字符串的字符开头

```
let str = "Coding is the LOVE of my LIFE!"
str.startsWith("Cod") = true 
```

*   String.endsWith() →检查字符串是否以指定字符串的字符结尾

```
let str = "Coding is the LOVE of my LIFE!"
str.endsWith("!") = true
```

*   String.trim() →删除字符串两边的空白

```
let str = "   Coding is the LOVE of my LIFE!   "
str.trim() = Coding is the LOVE of my LIFE!
```

*   String.repeat() →返回一个新字符串，该字符串包含它所调用的字符串的指定副本数。接受 count number 参数。

```
let str = "Coding is the LOVE of my LIFE!"
str.repeat(3) =  Coding is the LOVE of my LIFE! Coding is the LOVE of my LIFE! Coding is the LOVE of my LIFE!
```

*   String.match() →使用正则表达式在字符串中搜索匹配项，并以数组形式返回匹配项。

```
let str = "Coding is the LOVE of my LIFE!"
str.match(/of/g) = [ 'of' ]
```

*   String.replace() —>在字符串中搜索指定值或正则表达式，并返回指定值被替换的新字符串。接受两个参数:1st 是要替换的搜索值，2nd 是要替换搜索值的新值。

```
let str = "Coding is the LOVE of my LIFE!  Coding is the best thing to happen on Earth. I love coding!"str.replace("Coding", "JavaScript")= JavaScript is the LOVE of my LIFE! Coding is the best thing to happen on Earth. I love coding!// Notice how only the first instance of "coding" was replaced. To do a global replacement for all occurrences, we need to place a global modifier, "g", in the parameter. See belowstr.replace(/Coding/g, "JavaScript")= JavaScript is the LOVE of my LIFE! JavaScript is the best thing to happen on Earth. I love coding!// Now the last "coding" was not changed because parameters are case-sensitive. To make the search value case-insensitive, we need to add "i". str.replace(/Coding/gi, "JavaScript") = JavaScript is the LOVE of my LIFE! JavaScript is the best thing to happen on Earth. I love JavaScript!
```

Go get ’em tiger!