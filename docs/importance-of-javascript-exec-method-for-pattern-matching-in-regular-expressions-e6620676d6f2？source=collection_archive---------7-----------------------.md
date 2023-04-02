# JavaScript Exec 方法在正则表达式模式匹配中的重要性

> 原文：<https://javascript.plainenglish.io/importance-of-javascript-exec-method-for-pattern-matching-in-regular-expressions-e6620676d6f2?source=collection_archive---------7----------------------->

## 一个非常著名的面试问题的不同和伟大的解决方案

![](img/743b77ce1eec36cd39d47f6dfd216431.png)

Photo by [Rachel Danner](https://unsplash.com/@rachel_danner156?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

正则表达式对于在字符串中查找特定匹配的模式非常有用。它们帮助我们将复杂的逻辑转换成简短易懂的代码。
今天我们将看到 javascript 中一个非常棒的模式匹配方法，那就是`exec`方法。

它具有以下语法

```
regexObj.exec(string)
```

在进入实际问题之前，我们先了解一下`exec`是什么，它是做什么的。

**exec 方法类似于正则表达式的 match 方法，如果成功则返回找到的匹配数组，否则返回 null**

*模式还维护一个 lastIndex 属性，它是一个索引，一旦找到一个特定的匹配，它将从这个索引开始搜索下一个匹配。*

我们通过一个例子来了解一下。

**假设，我们想要找到字符串中所有元音的出现以及匹配的位置，我们可以像下面这样做**

```
const text = "This is a simple text";
const pattern = /[aeiou]/g;
let output = "";
while((result = pattern.exec(text)) !== null) {
 output += result[0] + " " + pattern.lastIndex + "\n";
}
console.log(output);/* outputi 3
i 6
a 9
i 12
e 16
e 19*/
```

**正如您从输出中看到的，打印的索引不是匹配的索引，而是它将开始搜索下一个匹配的索引，考虑到索引计数从 0 而不是 1 开始。**

现在开始真正的面试问题。
问题是这样的

写一个程序，它将打印一个单词在一个特定的字符串中出现了多少次，并且打印出它的索引。

让我们从它开始

假设，我们想检查单词“happy”在字符串中出现了多少次，我们可以这样做，如下所示

```
const str = "I felt happy because I saw the others were happy and because I knew I should feel happy, but I wasn’t really happy.";
const pattern = /happy/g;
let count = 0;
while((result = pattern.exec(str)) !== null) {
 count++;
 console.log(result[0], pattern.lastIndex - result[0].length);
}
console.log("Total Occurrences:", count);/* outputhappy 7
happy 43
happy 82
happy 109
Total Occurrences: 4*/
```

但是上面的代码有一个问题，我们直接将模式中的文本 happy 硬编码为
**const pattern =/happy/g；**

但是如果预先不知道要搜索的文本，我们可以使用正则表达式的 RegExp 构造函数来实现与下面相同的事情

```
const str = "I felt happy because I saw the others were happy and because I knew I should feel happy, but I wasn’t really happy.";
**const string = "happy";
const pattern = new RegExp(string,"g");**
let count = 0;
while((result = pattern.exec(str)) !== null) {
 count++;
 console.log(result[0], pattern.lastIndex - result[0].length);
}
console.log("Total Occurrences:", count);/* outputhappy 7
happy 43
happy 82
happy 109
Total Occurrences: 4*/
```

在上面的代码中，我们刚刚更改了:

```
const string = "happy";
const pattern = new RegExp(string,"g");
```

而不是:

```
const pattern = /happy/g;
```

所以现在您可以在字符串变量中存储要搜索的动态文本，并将其用作 RegExp 构造函数的第一个参数。

今天就到这里，希望你今天学到了新的东西。

**别忘了直接在你的收件箱** [**这里**](https://yogeshchavan.dev) **订阅我的每周时事通讯，里面有惊人的技巧、诀窍和文章。**