# 如何在 JavaScript 中检查回文

> 原文：<https://javascript.plainenglish.io/how-to-check-for-palindromes-in-javascript-e95f3d2dd281?source=collection_archive---------12----------------------->

![](img/b2ccb4866aadde8a24fae858b0ca7532.png)

解决回文算法是技术面试中最常见的数据结构任务之一。在这篇博客中，我将分解其中的一个算法，并与你分享我解决它的方法。这可能不是最聪明和最有效的方法，所以请随意想出其他方法！

首先:什么是回文？它是一个字符序列，前后读起来一样。我们的任务是找出一个给定的字符串是否是一个回文。如果给定的字符串是回文，则返回 true。否则，返回 false。

因此，我们的**第一步**是删除所有非字母数字字符，并将所有内容转换成小写，以便检查回文。幸运的是，为了完成这个任务，我们有两个内置方法。

```
function findPalindrome(string) {
    let cleanChar = string.replace(/[^A-Z0-9]/ig, "").toLowerCase();
}
```

我们的**第二步**是反转我们的“干净”弦。在这种情况下，split()、reverse()和 join()方法非常方便。方法将一个字符串分割成一个字符串数组。方法的作用是:反转一个数组。 **join()** 方法将一个数组的所有元素连接回一个字符串。

```
function findPalindrome(string) {
    let cleanChar = string.replace(/[^A-Z0-9]/ig, "").toLowerCase();
    let reversedChar = cleanChar.split('').reverse().join('');
}
```

我们的**第三步也是最后一步**是比较两个字符串，并检查我们的字符串是否是回文。

```
function findPalindrome(string) {
    let cleanChar = string.replace(/[^A-Z0-9]/ig, "").toLowerCase();
    let reversedChar = cleanChar.split('').reverse().join('');       return (cleanChar === reversedChar)
}
```

这是我解决回文算法的快速指南。希望对你有帮助！

**来源:**

1.  [JavaScript 中检查回文的两种方法](https://www.freecodecamp.org/news/two-ways-to-check-for-palindromes-in-javascript-64fea8191fd7/)
2.  [堆栈溢出](https://stackoverflow.com/questions/14813369/palindrome-check-in-javascript)
3.  JavaScript 中检查回文的 11 种方法