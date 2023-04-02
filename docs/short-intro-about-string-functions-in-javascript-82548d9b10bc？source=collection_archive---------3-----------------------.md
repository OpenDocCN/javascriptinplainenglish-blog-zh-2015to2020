# JavaScript 中字符串函数的简短介绍

> 原文：<https://javascript.plainenglish.io/short-intro-about-string-functions-in-javascript-82548d9b10bc?source=collection_archive---------3----------------------->

了解可用的 JavaScript 字符串函数。

![](img/151861f86700a2624bb37b094cfdb96a.png)

Image taken from unsplash

**字符串属性**

1 .长度

```
var str = "Jagathish";
str.length; //9  --> returns the length of the string
```

**功能**

1.  `toUpperCase()` - >返回源字符串大写格式的新字符串

```
var str = "Jagathish";var str2 = str.toUpperCase();console.log(str);//Jagathishconsole.log(str2);//JAGATHISH 
```

2.`toLowerCase()` - >返回源字符串小写格式的新字符串

```
var str = "Jagathish";var str2 = str.toLowerCase();console.log(str);//Jagathishconsole.log(str2);//jagathish
```

3.`trim()` - >返回一个新字符串，该字符串的开头和结尾都删除了空格。

```
var str = "   Jagathish  ";var trimmedStr = str.trim(); console.log(trimmedStr); //"Jagathish"var str1 = "J  aga";
var trimmedStr1 = str1.trim();
console.log(trimmedStr1); //"J  aga"var str2 = "J   .";
var trimmedStr2 = str2.trim();
console.log(trimmedStr2); //"J   ."
```

4.`trimStart()` - >返回一个新字符串，从该字符串的开始处移除空格。`trimLeft()`是这种方法的别名。

```
var str = "   Jagathish  ";var trimmedStr = str.trimStart(); console.log(trimmedStr); //"Jagathish  "
```

5.`trimEnd()` - >返回一个新字符串，该字符串末尾的空格被删除。`trimRight()`是这个方法的别名。

```
var str = "   Jagathish  ";var trimmedStr = str.trimEnd(); console.log(trimmedStr); //"   Jagathish"
```

5.`charAt()` - >返回给定索引处的字符。

```
var str = "Jagathish";console.log( str.charAt() ); //the default index is 0 so "J"str.charAt(1); //astr.charAt(8); //hstr.charAt(100); //"" if max than str length returns empty stringstr.charAt(-1); // for all negative values returns empty string//no type conversion takes pace so the result is empty string
str.charAt("1");  //""
```

6.`charCodeAt()` - >返回给定索引处字符的`UTF-16`字符代码。`chatAt`方法中`index` 的所有规则与`charCodeAt`相似。

```
// char code for **a - 97, b =98 ,... z -122**// char code for **A - 65 ... Z - 90**var str = "Jagathish";str.charCodeAt(0); // 74str.charCodeAt(1); // 97
```

7.`concat(arguments)`将传递的参数与源字符串连接起来。它不会在源字符串上改变。

```
var str = "JavaScript";str.concat(' ', "Jeep", "."); // JavaScript Jeep.
```

*注意:*不推荐使用这种方法，你可以使用赋值操作符来代替普通的连接。
例子

```
var str = "JavaScript".var name = str + " " + "Jeep" +".";
```

8.`includes(searchingString)`返回是否可以在源字符串中找到搜索字符串。

```
var str = "JavaScript Jeep";str.includes("Jeep"); // true
str.includes("jeep"); // false --> case sensitive// we can also specify the index to start searchvar str = "Jeep";
str.includes("Jeep", 0);  // true
str.includes("Jeep", 1);  // false
```

9.`endsWith(searchString)`返回源字符串是否以搜索字符串结尾。

```
var str = "JavaScript Jeep";str.endsWith("Jeep"); // truestr.endsWith("jeep"); // falsestr.endsWith("Kepp"); // false// we can also specify the endPosition(index+1) to which the test should runvar str = "Jeep";str.endsWith("J");  //falsestr.endsWith("J", 1) // true
```

10.`startsWith(searchString)`返回源字符串是否以搜索字符串开始。

```
var str = "JavaScript Jeep";str.startsWith("Java"); // truestr.startsWith("java"); // falsestr.startsWith("Ava"); // false// we can also specify the startPosition(index) to which the test should runvar str = "JAVASCRIPT"str.startsWith("VA");  //falsestr.startsWith("VA", 2) // truestr.startsWith("J", 1); // false --> because we are saying to start search from index 1 .
```

11.`includes(searchString)`确定是否在源字符串中找到搜索字符串

```
var str = "JavaScript Jeep";str.includes("Java"); // truestr.includes("java"); // falsestr.includes(""); // truestr.includes(); // false

we can also specify the index from which the search should startstr = "JavaScript";str.include("Java", 0); //truestr.include("Java", 1); //false
```

12 .`repeat(times)`返回一个新字符串，该字符串包含给定字符串的指定数量的副本。重复值必须为非负且小于无穷大，否则将引发范围错误。

```
var str = "Jeep ";var repeatedString = str.repeat(1); // "Jeep "repeatedString = str.repeat(2); // "Jeep Jeep"repeatedString = str.repeat(); // ""repeatedString = str.repeat(0); // ""
```

13.`indexOf(searchString)`返回搜索字符串在源字符串中第一次出现的索引，如果搜索字符串不在源字符串中，则返回-1。

```
var str = "Java Jeep JavaScript Jeep";str.indexOf("Jeep"); // 5str.indexOf('jeep'); // -1// we can also specify the index from which the search should happenstr.indexOf("Java", 0); // 0
str.indexOf("Java", 1); // 10
```

14.`lastIndexOf(searchString)`返回搜索字符串在源字符串中最后出现的索引，如果搜索字符串不在源字符串中，则返回-1。

```
var str = "Java Jeep Java Jeep";str.lastIndexOf("Jeep"); // 15str.lastIndexOf('jeep'); // -1
```

15.`match(regex)`提取字符串与正则表达式匹配的结果。

```
// Extracting vowel charactervar str = 'I love cooking.';var regex = /[AEIOUaeiou]/gi;var result = str.match(regex); //["I","o","e","o","o","i"]
```

16.这个方法测试源字符串是否匹配提供的正则表达式。如果源字符串与正则表达式不匹配，则返回-1。

```
var str = "str";var regex1 = **/a-z/gi**;var regex2 = **/[aeiou]/gi**;str.**search**(regex1); // 0 ('s' matches with the regex , where 's' is 
at 0th index)str.**search**(regex2); // -1 (because there is no match) 
```

17.`padStart(newStringLength)`这个方法在源字符串的开头添加填充符(默认为空格)，这样源字符串的长度就被转换为提供的长度。

```
var str = "Jeep";str.**padStart**(5); // " Jeep"str.**padStart**(10); // "      Jeep"str.**padStart**(5, "*"); // "*Jeep"str.**padStart**(10,"@"); // "@@@@@@Jeep"// If the value is lower than the current string's length, the current string will be returned as is.str.**padStart**(1); // "Jeep"str.**padStart**(); // "Jeep"str.**padStart**(-1); // "Jeep"
```

18.`padEnd(newStringLength)`该方法在源字符串的末尾添加填充符(缺省为空格)，以便将源字符串长度转换为所提供的长度。

```
var str = "Jeep";str.**padEnd**(5); // "Jeep "str.**padEnd**(10); // "Jeep      "str.**padEnd**(5, "*"); // "Jeep*"str.**padEnd**(10,"@"); // "Jeep@@@@@@"// If the value is lower than the current string's length, the current string will be returned as is.str.**padEnd**(1); // "Jeep"str.**padEnd**(); // "Jeep"str.**padEnd**(-1); // "Jeep"
```

19.`replace(regex, stringToReplace)`这个方法用匹配模式的`stringToReplace`替换字符串

```
var str = "I love dog. I love dog";var regex =  **/dog/g**;str.**replace**(regex, 'cat'); // I love cat. I love cat
```

20.`splice(fromIndex, toIndex)`提取源字符串的一部分并作为新字符串返回。

```
var str = "JavaScript Jeep";str.slice(**1**); "avascript Jeep"str.slice(**10**); " Jeep"str.slice(**11**); "Jeep"str.slice(**11, 12**); "J"str.slice(**11, 14**); "Jee"str.slice(**0**); "Javascript Jeep"str.slice(); "Javascript Jeep"
```

21 .`split(separator)`这个方法根据分隔符分割字符串。

```
var str = "Java Jeep";str.split(""); // ['J', 'a', 'v', 'a' ," ", 'J', 'e','e', 'p']str.split(" "); ["Java", "Jeep"]str.split() // ["Java Jeep"]------------------------------**// limiting number of split**var str = "this,is,a,test";str.split(","); ["this", "is","a","test"]str.split(",", 1); ["this"]str.split(",", 2); ["this", "is"]str.split(",",100); ["this", "is","a","test"]
```

22 .`substring(startIndex, endIndex)`返回起始和结束索引之间的字符串，或者到字符串的结尾。

```
var str = "JavaScript Jeep";str.substring(11); // "Jeep"str.substring(11,12); // "J"str.substring(); // "JavaScript Jeep"
```