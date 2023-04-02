# 在 Adobe 面试—软件工程在线评估

> 原文：<https://javascript.plainenglish.io/adobe-online-assessment-software-engineering-ee1dede9911d?source=collection_archive---------5----------------------->

![](img/db794c4e12da117f6fdbcee427602739.png)

Source: inkhive.com

本周，我们来看看 Adobe 在线评估中的两个问题。这是 2019 年的，所以可能在 2020 年发生了变化。

这两个都被认为是“简单”的，但显然这将取决于你对算法的知识和实践。

Good luck!

## 问题 1

你正在和你的朋友玩下面的 Nim 游戏:桌子上有一堆石头，每次你们中的一个人轮流移走 1 到 3 块石头。谁移走最后一块石头，谁就是赢家。你将在第一个回合移走石头。

你们两个都很聪明，都有博弈的最优策略。写一个函数来决定你是否能赢得游戏给定的石头堆的数量。

**举例:**

```
**Input:** 4
**Output:** false 
**Explanation:** If there are 4 stones in the heap, then you will never win the game;
             No matter 1, 2, or 3 stones you remove, the last stone will always be 
             removed by your friend.
```

*想一想:如果堆里有 5 颗石头，你能想出一种方法来移走这些石头，这样你就永远是赢家吗？*

**注:这个可以用 (1)的时间复杂度*和* (1)的空间复杂度*来解决。***

## 问题 2

两个整数之间的汉明距离是对应的比特不同的位置的数量。

如果你不熟悉汉明距离，请查看维基百科的描述。

给定两个整数`x`和`y`，计算汉明距离。

**注:**
0 ≤ `x`，`y` < 231。

**例如:**

```
**Input:** x = 1, y = 4**Output:** 2**Explanation:**
1   (0 0 0 1)
4   (0 1 0 0)
       ↑   ↑The above arrows point to positions where the corresponding bits are different.
```

*两个整数之间的汉明距离是对应的比特不相同的位置的个数。*

**注意:和上面的例子一样，这也可以用时间复杂度 *O* (1)和空间复杂度 *O* (1)来解决。**

一如既往，请在评论和任何问题中发表你的 JS 解决方案。在你们大部分人有机会尝试之后，我会公布我的解决方案。记住，如果这些很难解决，不要担心，继续练习，它会变得更容易！