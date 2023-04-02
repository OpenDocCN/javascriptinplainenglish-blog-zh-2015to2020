# Snapchat —面向初级开发人员的视频访谈

> 原文：<https://javascript.plainenglish.io/snap-snapchat-phone-interview-for-junior-developer-e7e630438b52?source=collection_archive---------7----------------------->

## 在这次新冠肺炎疫情期间，雇主们只转向电话和视频采访。

![](img/edd7bd6084ec8581f5a09ee2fbc179b2.png)

Source: forbes.com

本周，一位初级开发人员联系我，讨论他们采访 Snap Inc(以前的 Snapchat)的经历。

他们的视频采访以两个行为问题开始，“说说你自己”和“你为什么想成为一名网络开发者？”

然后是技术问题。这些最初的视频/电话采访相当短，只有 30 分钟，很少 45 分钟到 1 小时。在更长、更艰难的几轮谈判之前，它更像是一个过滤器。所以，你需要敏锐而清晰地梳理你的思维过程，并写下功能代码。记得问面试官任何能说明问题的问题。永远不要直接进入编码领域。先谈谈你打算如何解决这个问题。

**以下是被问到的问题:**

给定一个单词列表(**没有重复的**，请写一个程序，返回给定单词列表中所有连接的单词。

串联单词被定义为完全由给定数组中至少两个较短单词组成的字符串。

**示例:**

```
**Input:** ["cat","cats","catsdogcats","dog","dogcatsdog","hippopotamuses","rat","ratcatdogcat"]**Output:** ["catsdogcats","dogcatsdog","ratcatdogcat"]**Explanation:** "catsdogcats" can be concatenated by "cats", "dog" and "cats"; 
 "dogcatsdog" can be concatenated by "dog", "cats" and "dog"; 
"ratcatdogcat" can be concatenated by "rat", "cat", "dog" and "cat".
```

**注:**

1.  给定数组的元素数不会超过`10,000`
2.  给定数组中元素的长度和不会超过`600,000`。
3.  所有输入字符串将仅包括小写字母。
4.  返回的元素顺序并不重要。

**给定代码:**

```
var findAllConcatenatedWordsInADict = function(words) {
    **// code your solution here**
};
```

要进行更多练习，也可以尝试以下方法:

给定一个**非空的**字符串 *s* 和一个包含一个**非空**单词列表的词典*单词词典*，在 *s* 中加上空格，构成一个句子，其中每个单词都是一个有效的词典单词。返回所有这些可能的句子。

**注:**

*   字典中的相同单词可以在分段中多次重复使用。
*   你可以假设字典里没有重复的单词。

**例 1:**

```
**Input:** s = "catsanddog"
wordDict = ["cat", "cats", "and", "sand", "dog"]
**Output:** [
  "cats and dog",
  "cat sand dog"
]
```

**例 2:**

```
**Input:** s = "pineapplepenapple"
wordDict = ["apple", "pen", "applepen", "pine", "pineapple"]
**Output:** [
  "pine apple pen apple",
  "pineapple pen apple",
  "pine applepen apple"
]
**Explanation:** Note that you are allowed to reuse a dictionary word.
```

**例 3:**

```
**Input:** s = "catsandog"
wordDict = ["cats", "dog", "sand", "and", "cat"]
**Output:** []
```

请在注释中写入您的 JavaScript 解决方案。如果您不熟悉 JS，那么可以用您熟悉的语言提交它。和往常一样，我会发布我想出的解决方案。祝你好运，保持安全！希望我们能很快通过这种病毒🦠🦠🦠