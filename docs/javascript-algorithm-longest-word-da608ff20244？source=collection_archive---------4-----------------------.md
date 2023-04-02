# JavaScript 算法:最长的单词

> 原文：<https://javascript.plainenglish.io/javascript-algorithm-longest-word-da608ff20244?source=collection_archive---------4----------------------->

![](img/fb1194975a8f600f1999922c172621d7.png)

对于今天的算法，我们将编写一个名为`LongestWord`的函数，它将接受一个字符串`sen`作为输入。

给你一个包含句子的字符串。该函数的目标是输出句子中最长的单词。如果有多个单词具有相同的长度，则输出第一次出现的相同长度的单词。

另外，请注意，有些句子会包含数字或非字母数字字符。每个单词的字母数中不包括这些字符。让我们举个例子:

```
let sen = "I am eating 3!@Pancakes";
```

那句话中最长的单词是`Pancakes`。在`Pancakes`之前的非字母字符`3!@`不计算在内，但是即使没有这些字符，这个字符串仍然是最长的。

这是另一个例子:

```
let sen = "I love dogs";
```

即使`love`和`dogs`具有相同的长度，该函数也将返回`love`，因为它是句子中出现的第一个长度为 4 的字符串。

让我们把这变成代码。

在我们对字符串做任何事情之前，我们必须删除字符串中所有可能的数字和非字母数字字符。我们创建一个正则表达式模式，匹配所有不是 a-z、A-Z 和空白的字符。

```
let regex = /([^A-Z a-z])+/g;
```

我们获取正则表达式并在我们的`replace()`方法中使用它。为了删除它们，我们用一个空字符串替换所有匹配正则表达式模式的字符。我们用一个`split()`方法跟随那个方法，这样我们可以把我们的字符串变成一个数组。我们添加了一个分隔符，将句子分成单词，而不是单个字符。所有这些都被分配给一个名为`text`的变量。

```
let text = sen.replace(regex, "").split(" ");
```

接下来，我们创建两个变量。`longestText`变量保存句子中最长的单词。`longestCount`变量保存最长单词的长度。我们将其设置为 0。

```
let longestText = "";
let longestCount = 0;
```

现在我们遍历我们的文本数组。

```
for(let i = 0; i < text.length; i++){
    if(text[i].length > longestCount){
      longestCount = text[i].length;
      longestText = text[i];
    }
}
```

在我们的循环中，我们将每个单词的长度与`longestCount`进行比较。如果文本的长度大于当前最大字符串的长度，我们将该单词的长度赋给`longestCount`变量，并将单词本身赋给`longestText`变量。

在我们的 if 语句中，我们确保只使用大于`>`而不大于或等于`≥`。这是为了处理具有相同长度的多个字符串。该函数将只考虑该长度的第一个字符串。任何其他具有相同长度的字符串都会被忽略，直到另一个更长的字符串出现。

在循环结束时，我们返回我们的`longestText`变量。

```
return longestText;
```

以下是剩余的代码:

```
function LongestWord(sen) { 
  let regex = /([^A-Z a-z])+/g;
  let text = sen.replace(regex, "").split(" ");
  let longestText = "";
  let longestCount = 0;

  for(let i = 0; i < text.length; i++){
    if(text[i].length > longestCount){
      longestCount = text[i].length;
      longestText = text[i];
    }
  }
  return longestText;
}
```

如果你觉得这个算法有帮助，可以看看我最近的其他 JavaScript 算法解决方案:

[](https://medium.com/javascript-in-plain-english/javascript-algorithm-find-intersection-22d6eaa4c523) [## JavaScript 算法:寻找交集

### 对于今天的算法，我们要写一个叫 FindIntersection 的函数。

medium.com](https://medium.com/javascript-in-plain-english/javascript-algorithm-find-intersection-22d6eaa4c523) [](https://levelup.gitconnected.com/javascript-algorithm-marcs-cakewalk-98ad0c699348) [## JavaScript 算法:Marc 的 Cakewalk

### 对于今天的算法，我们将编写一个名为 marcsCakewalk 的函数，它将接受一个数组，卡路里，作为…

levelup.gitconnected.com](https://levelup.gitconnected.com/javascript-algorithm-marcs-cakewalk-98ad0c699348) [](https://medium.com/javascript-in-plain-english/javascript-algorithm-funny-string-adabc8eac4d7) [## JavaScript 算法:有趣的字符串

### 对于今天的算法，我们要写一个叫做 funnyString 的函数，它接受一个输入:一个字符串，s。

medium.com](https://medium.com/javascript-in-plain-english/javascript-algorithm-funny-string-adabc8eac4d7)