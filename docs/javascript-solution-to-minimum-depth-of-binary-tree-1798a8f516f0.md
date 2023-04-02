# 二叉树最小深度的 JavaScript 解决方案

> 原文：<https://javascript.plainenglish.io/javascript-solution-to-minimum-depth-of-binary-tree-1798a8f516f0?source=collection_archive---------5----------------------->

[***先决条件:JavaScript 中的树遍历***](https://medium.com/javascript-in-plain-english/tree-traversal-in-javascript-9b1e92e15abb)

![](img/6ccff12f961cd4116a648f345df83b22.png)

Photo by [Greg Rosenke](https://unsplash.com/@greg_rosenke?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

对于我们最后的 She 编码数据结构和算法事件，我选择了 [**二叉树的最小深度**](https://leetcode.com/problems/minimum-depth-of-binary-tree/) 问题来与我们的参与者练习树遍历。一开始可能很有挑战性，在深度优先搜索和广度优先搜索的帮助下，我们可以很容易地解决它。

**问题**

```
Given a binary tree, find its minimum depth.The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.**Note:** A leaf is a node with no children.**Example 1:** 3
    / \
   9  20 
      / \ 
     15  7**Input:** root = [3,9,20,null,null,15,7]
**Output:** 2 **Example 2:** 2
      \
       3
        \
         4
          \
           5
            \
             6**Input:** root = [2,null,3,null,4,null,5,null,6]
**Output:** 5**Constraints:** -- The number of nodes in the tree is in the range [0, 105].
-- -1000 <= Node.val <= 1000
```

输入是一个二叉树，节点有 val，left 和 right 属性，如下所示。

[https://gist.github.com/GAierken/f1ab81562b141052ce18ca003da0e6b1](https://gist.github.com/GAierken/f1ab81562b141052ce18ca003da0e6b1)

输出是一个整数，表示给定树的最小深度。上面的例子 1，树有三条路径:3 → 9(两级)，3 → 20 →15(三级)，3 → 20 →15(三级)。所以最小深度是 2。例 2，树只有一条路径:2 → 3 → 4 → 5 → 6(五层)，最小深度输出为 5。

我们如何计算每条路径的级别，并最终返回最小深度？首先遍历树，计算所有路径的级别，并在比较后不断更新最小深度。说到树的遍历，常用的方法是深度优先搜索和广度优先搜索。

**解决方案**

*广度优先搜索*

在开始遍历之前，我们需要确保 root 不是空值。当 root 为 null 时，没有级别，所以我们返回 0。BFS 逐层遍历树，我们循环遍历一层的节点，当我们到达叶节点时，我们将深度增加 1 并立即返回深度，最短路径将首先到达叶，因此输出深度是树的最小深度。

[https://gist.github.com/GAierken/d6d17a119e195f0660128ff971951756](https://gist.github.com/GAierken/d6d17a119e195f0660128ff971951756)

*深度优先搜索*

像往常一样，在我们遍历树之前，确保 root 不为空，如果是，则没有深度，我们返回 0。当根没有左孩子时，我们遍历右孩子，深度增加一级。同样，如果根没有右孩子，我们向左遍历，深度增加 1。当根既有左又有右时，我们遍历两条路径，选择较小的深度并增加一个，返回最后的最小深度。

[https://gist.github.com/GAierken/8746b29ef2ccb577100d49ef9070dc26.js](https://gist.github.com/GAierken/8746b29ef2ccb577100d49ef9070dc26.js)

我希望这能帮助你更好地理解如何解决类似的问题。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**

**资源:资源:**

[](https://medium.com/javascript-in-plain-english/tree-traversal-in-javascript-9b1e92e15abb) [## JavaScript 中的树遍历

### 呼吸优先搜索 vs 深度优先搜索

medium.com](https://medium.com/javascript-in-plain-english/tree-traversal-in-javascript-9b1e92e15abb) [](https://leetcode.com/problems/minimum-depth-of-binary-tree/) [## 二叉树的最小深度- LeetCode

### 给定一棵二叉树，求其最小深度。最小深度是从…沿最短路径的节点数

leetcode.com](https://leetcode.com/problems/minimum-depth-of-binary-tree/)