# 在谷歌面试——JavaScript 评估问题

> 原文：<https://javascript.plainenglish.io/interviewing-at-google-javascript-assessment-questions-f9bf0c0df157?source=collection_archive---------0----------------------->

既然你们大多数人喜欢这个的[微软版本，这里有一个谷歌版本！](https://medium.com/javascript-in-plain-english/microsoft-online-assessment-questions-js-f68ecdb6e927)

![](img/f7a88246b773724ba9883bb9412cd6be.png)

Credit: Shutterstock

以下是谷歌 2019 年针对入门级开发者的在线测评中的一些问题。

像上周一样，我会发布问题，并在发布我的解决方案之前等待评论中的答案。提交答案时考虑时间/空间复杂性。请用 JavaScript 提交答案。

## 问题

在一排树中，第`i`棵树产生类型为`tree[i]`的果实。

你**从你选择的任意一棵树**开始，然后重复执行以下步骤:

1.  在你的篮子里放一片这棵树上的水果。如果不能，就停下来。
2.  移动到当前树右侧的下一棵树。如果右边没有树，就停下来。

请注意，在初始选择启动树之后，您没有任何选择:您必须执行步骤 1，然后步骤 2，然后返回到步骤 1，然后步骤 2，等等，直到您停止。

你有两个篮子，每个篮子可以装任意数量的水果，但是你希望每个篮子只能装一种水果。

用这个程序你能收集的水果总量是多少？

**例 1:**

```
**Input:** [1,2,1]
**Output:** 3
**Explanation:** We can collect [1,2,1].
```

**例 2:**

```
**Input:** [0,1,2,2]
**Output:** 3
**Explanation:** We can collect [1,2,2].
If we started at the first tree, we would only collect [0, 1].
```

**例三:**

```
**Input:** [1,2,3,2,2]
**Output:** 4
**Explanation:** We can collect [2,3,2,2].
If we started at the first tree, we would only collect [1, 2].
```

**例 4:**

```
**Input:** [3,3,3,1,2,1,1,2,3,3,4]
**Output:** 5
**Explanation:** We can collect [1,2,1,1,2].
If we started at the first tree or the eighth tree, we would only collect 4 fruits.
```

**注:**

1.  `1 <= tree.length <= 40000`
2.  `0 <= tree[i] < tree.length`

有一个特殊的键盘，所有键都在一排。

给定一个长度为 26 的字符串`keyboard`，表示键盘的布局(索引从 0 到 25)，最初你的手指在索引 0 处。要输入一个字符，你必须将手指移到所需字符的索引处。将手指从食指`i`移动到食指`j`所需的时间是`|i - j|`。

你想输入一个字符串`word`。写一个函数来计算用一个手指打字需要多少时间。

**例 1:**

```
**Input:** keyboard = "abcdefghijklmnopqrstuvwxyz", word = "cba"
**Output:** 4
**Explanation:** The index moves from 0 to 2 to write 'c' then to 1 to write 'b' then to 0 again to write 'a'.
Total time = 2 + 1 + 1 = 4.
```

**约束:**

*   `keyboard.length == 26`
*   `keyboard`每个英文小写字母按照某种顺序恰好包含一次。
*   `1 <= word.length <= 10^4`
*   `word[i]`是英文小写字母。

您将获得一个许可证密钥，表示为字符串 S，其中仅包含字母数字字符和破折号。字符串被 N 个破折号分成 N+1 组。

给定一个数字 K，我们想要重新格式化字符串，使得每个组包含正好 K 个字符，除了第一个组可能比 K 短，但仍然必须包含至少一个字符。此外，两个组之间必须插入一个破折号，并且所有小写字母都应转换为大写字母。

给定一个非空字符串 S 和一个数字 K，根据上述规则格式化该字符串。

**例一:**

```
**Input:** S = "9B2A-2c-9-y", K = 4**Output:** "9B2A-2C9Y"**Explanation:** The string S has been split into two parts, each part has 4 characters.
Note that the two extra dashes are not needed and can be removed.
```

**例二:**

```
**Input:** S = "2-7g-6-E", K = 2**Output:** "2-7G-6E"**Explanation:** The string S has been split into three parts, each part has 2 characters except the first part as it could be shorter as mentioned above.
```

**注:**

1.  字符串 S 的长度不会超过 12000，K 是正整数。
2.  字符串 S 仅由字母数字字符(a-z 和/或 A-Z 和/或 0–9)和破折号(-)组成。
3.  字符串 S 是非空的。

一如既往，祝你好运，并随时提问，互相帮助！*注:这些问题可能比上周贴出的* [*微软问题*](https://medium.com/javascript-in-plain-english/microsoft-online-assessment-questions-js-f68ecdb6e927) *略简单。*