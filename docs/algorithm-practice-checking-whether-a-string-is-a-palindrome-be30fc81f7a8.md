# 算法练习:检查一个字符串是否是回文

> 原文：<https://javascript.plainenglish.io/algorithm-practice-checking-whether-a-string-is-a-palindrome-be30fc81f7a8?source=collection_archive---------8----------------------->

![](img/9fd66b41f4ef2eff9d45b6935cb7160e.png)

Photo by [Joshua Sortino](https://unsplash.com/@sortino?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

这里有一个在技术面试中经常出现的经典算法问题:*给定一个字符串，我们如何检查它是否是回文*？

在我们深入问题本身之前，让我们先退后一步，回顾一下什么是回文。一个**回文**是一个前后读起来一样的字符串。经典的例子是“*racecar”*—反过来读，字符串还是“racecar”。

现在我们已经清楚了回文的概念，让我们进入算法和如何解决它:给定一个*非空的*字符串，我们的任务是编写一个函数，根据该字符串是否是回文返回`true`或`false`。请注意，单字母字符串是一个回文。

![](img/f4c98f0522da1684176f699379616252.png)

Src: [Dictionary.com](https://www.dictionary.com/e/palindromic-word/)

在我们开始编写解决方案之前，让我们从概念的角度来看这个问题。回文是一个向前和向后读都一样的字符串——所以这意味着如果我们向后遍历字符串，它应该与向前读的字符串完全相等。很简单。

下一个问题是:我们如何迭代一个字符串？一种方法是循环遍历字符串，将每次循环得到的每个字母存储在一个数组中，这样我们就可以轻松地访问结果。然后，我们可以将数组中的字母连接在一起创建一个字符串，将结果字符串与原始传入的字符串进行比较，并返回一个布尔值。

这在代码中可能是这样的:

上述解决方案是可行的，并且简单有效。从上述解决方案的空间和时间复杂性来看，我们使用 O(n)时间和空间。我们能想出一个稍微高效一点的解决方案吗？

如果我们能找到一种方法来解决这个问题，而不需要在一个数组中存储颠倒的字母，会怎么样呢？我们可以使用双指针方法，其中我们有一个右指针和一个左指针，在每次迭代中，我们可以比较右指针处的字母是否等于右指针处的字母。如果在每次迭代中，两个字母是相同的，那么这个字符串就是一个回文。

下面是该解决方案的代码:

这在 O(n)时间和 O(1)空间中运行，使它更有空间效率。完美。