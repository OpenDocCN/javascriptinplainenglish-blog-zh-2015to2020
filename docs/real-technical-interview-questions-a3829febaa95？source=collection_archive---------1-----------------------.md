# 真正的技术面试问题

> 原文：<https://javascript.plainenglish.io/real-technical-interview-questions-a3829febaa95?source=collection_archive---------1----------------------->

由于许多刚起步的开发人员会花大量时间为他们梦想的工作做准备，我认为这篇文章会有所帮助。这些是我遇到的一些真正的问题——在线评估、电话采访和对新开发人员的面对面采访。

BRING IT ON!

下面我将列出来自**公司的几个问题，但**我不会公布解决方案。如果你想讨论一个问题/答案，请留下评论或给我发消息:)

## 亚马逊的在线评估

***合并两个排序列表***

合并两个排序的链表，并将其作为一个新的列表返回。新列表应该由前两个列表的节点拼接而成。

示例:

```
**Input:** 1->2->4, 1->3->4
**Output:** 1->1->2->3->4->4
```

***搜索 2D 矩阵二***

编写一个有效的算法，在一个 *m* x *n* 矩阵中搜索一个值。该矩阵具有以下特性:

*   每行中的整数从左到右升序排列。
*   每列中的整数从上到下按升序排序。

示例:

考虑以下矩阵:

```
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```

给定目标= `5`，返回`true`。

给定目标= `20`，返回`false`。

***另一棵树的子树***

给定两个非空二叉树 **s** 和 **t** ，检查树 **t** 是否与 **s** 的子树具有完全相同的结构和节点值。 **s** 的子树是由 **s** 中的一个节点及其所有子节点组成的树。树 **s** 也可以被认为是它自身的子树。

示例 1:

给定的树 s:

```
 3
    / \
   4   5
  / \
 1   2
```

给定树 t:

```
 4 
  / \
 1   2
```

返回 **true** ，因为 t 与 s 的子树有相同的结构和节点值。

示例 2:

给定的树 s:

```
 3
    / \
   4   5
  / \
 1   2
    /
   0
```

给定树 t:

```
 4
  / \
 1   2
```

返回**假**。

## 脸书的电话采访

***有效回文 II***

给定一个非空字符串`s`，你最多可以删除**一个字符**。判断一下能不能做成回文。

示例 1:

```
**Input:** "aba"
**Output:** True
```

示例 2:

```
**Input:** "abca"
**Output:** True
**Explanation:** You could delete the character 'c'.
```

注意:

1.  该字符串将只包含小写字符 a-z。该字符串的最大长度为 50000。

***有效回文 III***

给定一个字符串`s`和一个整数`k`，找出给定的字符串是否是一个 *K 回文*。

如果一个字符串可以通过移除最多`k`个字符转换成一个回文，那么它就是 K-回文。

示例 1:

```
**Input:** s = "abcdeca", k = 2
**Output:** true
**Explanation:** Remove 'b' and 'e' characters.
```

约束条件:

*   `1 <= s.length <= 1000`
*   `s`只有小写的英文字母。
*   `1 <= k <= s.length`

***先坏版本***

你是一名产品经理，目前正带领一个团队开发新产品。不幸的是，你们产品的最新版本没有通过质量检查。由于每个版本都是基于前一个版本开发的，所以一个坏版本之后的所有版本也都是坏的。

假设你有`n`版本`[1, 2, ..., n]`，你想找出第一个坏的，这导致了所有后续的都是坏的。

给你一个 API `bool isBadVersion(version)`，它将返回`version`是否坏。实现一个函数来查找第一个坏版本。您应该尽量减少调用 API 的次数。

示例:

```
Given n = 5, and version = 4 is the first bad version.call isBadVersion(3) -> false
call isBadVersion(5) -> true
call isBadVersion(4) -> trueThen 4 is the first bad version.
```

***添加和搜索 Word —数据结构设计***

设计支持以下两种操作的数据结构:

```
void addWord(word)
bool search(word)
```

search(word)可以搜索文字或者只包含字母`a-z`或者`.`的正则表达式字符串。一个`.`意味着它可以代表任何一个字母。

**例如:**

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

## 谷歌现场面试

***巧克力甜度***

给定一个由`n`个非负整数组成的数组`chocolate`，其中的值是巧克力的甜度。你还会得到一个值`k`,表示你将与多少朋友分享这块巧克力。你的朋友很贪婪，所以他们总是拿走最甜的一块。找出你能得到的最大甜度。

示例:

```
Input: chocolate = [6, 3, 2, 8, 7, 5], k = 3
Output: 9
Explanation:
The values in array are sweetness level in each chunk of chocolate. Since k = 3, so you have to divide this array in 3 pieces,
such that you would get maximum out of the minimum sweetness level. So, you should divide this array in
[6, 3] -> 6 + 3 = 9
[2, 8] -> 2 + 8 = 10
[7, 5] -> 7 + 5 = 12
Your other two friends will take the sweetest chunk, so they will take 12 and 10\. The maximum sweetness level you could get is 9.
```

***断字三***

给定一个没有空格的句子`s`和一个词典`dict`，你必须找到在**最小**单个词典单词数中破句的方法。如果有多个解决方案，返回其中任何一个。

示例 1:

```
Input: s = "bedbathandbeyand", dict = ["bed", "bath", "bat", "and", "hand", "bey", "beyand"]
Output: ["bed", "bath", "and", "beyand"] or ["bed", "bat", "hand", "beyand"]
```

示例 2:

```
Input: s = "catsandog", dict = ["cats", "dog", "sand", "and", "cat"]
Output: []
```

## 优步现场面试

***跳跃数字***

给定一个正 int `n`，打印所有小于等于`n`的跳转数。如果一个数中所有相邻的数字相差 1，则该数称为跳跃数。比如`8987`和`4343456`是跳号，而`796`和`89098`不是。所有的一位数都被认为是跳跃数。

**举例:**

```
Input: 105
Output: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 12, 21, 23, 32, 34, 43, 45, 54, 56, 65, 67, 76, 78, 87, 89, 98, 101]
```

准备面试时，这些技术问题应该是一个很好的起点。记得问你的面试官一些问题，并通过你的回答进行交流。面试官想看看你是怎么想的，而不是直接跳进问题里。另外，记得在白板上尝试和练习这些 Algo/DS 问题。许多公司会希望你亲自用白板写下你的答案。