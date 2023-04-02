# 脸书面试——现场 JavaScript 技术面试问题

> 原文：<https://javascript.plainenglish.io/facebook-on-site-technical-interview-1264cacad263?source=collection_archive---------0----------------------->

微软[和谷歌](https://medium.com/javascript-in-plain-english/microsoft-online-assessment-questions-js-f68ecdb6e927)[的评估很受欢迎，读者要求更多的例子！](https://medium.com/javascript-in-plain-english/interviewing-at-google-javascript-assessment-questions-f9bf0c0df157)

![](img/92374a5222bce1e6ebb906a4cbf4cb3c.png)

Source: [blog.au.indeed.com](http://blog.au.indeed.com/)

以下是 2019 年脸书现场技术面试的一些问题。

# 容易的

你是一名产品经理，目前正带领一个团队开发新产品。不幸的是，你们产品的最新版本没有通过质量检查。由于每个版本都是基于前一个版本开发的，所以一个坏版本之后的所有版本也都是坏的。

假设你有`n`个版本`[1, 2, ..., n]`，你想找出第一个不好的，导致后面的都不好。

给你一个 API `bool isBadVersion(version)`，它将返回`version`是否坏。实现一个函数来查找第一个坏版本。您应该尽量减少调用 API 的次数。

**例如:**

```
Given n = 5, and version = 4 is the first bad version.call isBadVersion(3) -> false
call isBadVersion(5) -> true
call isBadVersion(4) -> trueThen 4 is the first bad version.
```

# 中等

设计支持以下两种操作的数据结构:

```
void addWord(word)
bool search(word)
```

search(word)可以搜索一个文字单词或只包含字母`a-z`或`.`的正则表达式字符串。一个`.`意味着它可以代表任何一个字母。

**示例:**

```
addWord("bad")
addWord("dad")
addWord("mad")
search("pad") -> false
search("bad") -> true
search(".ad") -> true
search("b..") -> true
```

**注:**
你可以假设所有的单词都是由小写字母`a-z`组成的。

给定一张二维网格图，图中有`'1'` s(陆地)和`'0'` s(水域)，计算岛屿的数量。岛屿被水包围，由水平或垂直连接的相邻陆地形成。你可以假设网格的四个边缘都被水包围。

**例 1:**

```
**Input:**
11110
11010
11000
00000**Output:** 1
```

**例 2:**

```
**Input:**
11000
11000
00100
00011**Output:** 3
```

# 困难的

删除最少数量的无效括号，以使输入字符串有效。返回所有可能的结果。

**注意:**输入字符串可以包含圆括号`(`和`)`以外的字母。

**例 1:**

```
**Input:** "()())()"
**Output:** ["()()()", "(())()"]
```

**例 2:**

```
**Input:** "(a)())()"
**Output:** ["(a)()()", "(a())()"]
```

**例 3:**

```
**Input:** ")("
**Output:** [""]
```

像上周一样，我会发布问题，并在发布我的解决方案之前等待评论中的答案。提交答案时考虑时间/空间复杂性。请用 JavaScript 提交答案。

祝你好运！注意:这些都很难，亲自看起来更难。