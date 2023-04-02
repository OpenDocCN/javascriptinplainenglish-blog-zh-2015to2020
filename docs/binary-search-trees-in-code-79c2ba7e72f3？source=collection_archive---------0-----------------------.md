# 代码中的二分搜索法树

> 原文：<https://javascript.plainenglish.io/binary-search-trees-in-code-79c2ba7e72f3?source=collection_archive---------0----------------------->

![](img/a0f41ce96fbf8548f8a094db8ad8c271.png)

上周，我们看了看二分搜索法树背后的理论，以及它们是如何工作的(如果你错过了，请在这里阅读[。本周，我们将使用 JavaScript 以编程方式创建和操作二叉查找树。](https://medium.com/javascript-in-plain-english/binary-search-trees-how-they-work-9c64029eedb7)

提醒一下，**二叉树**是计算机科学中的一种树形数据结构，其中每个节点最多只能有两个孩子。子节点通常被称为左节点和右节点。一个**二叉查找树**是一种特定类型的二叉树，其中节点按照它们在树中的排列进行排序。每个节点仍然最多只能有两个子节点，但是在二叉查找树中，**左边的子节点** **将*始终*小于根节点的值**，右边的子节点**将*始终*大于它的值**。

# JavaScript 中的二叉查找树

我们如何在 JavaScript 中以编程方式表示二叉查找树？实际上，这很简单。事实上比表示一个[图](https://medium.com/swlh/bfs-and-dfs-in-code-ba3f01c16156)简单多了。JavaScript 对象是表示树中每个节点的完美数据结构，因为它允许我们存储每个节点左右子节点的关系数据。

例如，假设我们有以下一组数字，并希望将其排列成二叉查找树:

```
let numbers = [9, 4, 12, 15, 72, 21, 36, 2, 10, 14, 8, 19]
```

从 9 开始，我们可以将 9 表示为根节点，如下所示:

```
let rootNode = {element: 9, rightChild: null, leftChild: null}
```

转到列表中的下一个数字 4，我们看到 4 小于 9，因此是它的左孩子。我们用 JS 对象表示 4，并更新 rootNode:

```
**let nodeA = {element: 4, rightChild: null, leftChild: null}** let rootNode = {element: 9, rightChild: null, **leftChild: nodeA**}
```

接下来，我们移动到数组中的第三个元素，即 12。12 大于 9，所以将被添加为根节点的右子节点:

```
**let nodeB = {element: 12, rightChild: null, leftChild: null}**let nodeA = {element: 4, rightChild: null, leftChild: null} let rootNode = {element: 9, **rightChild: nodeB,** leftChild: nodeA}
```

这一过程贯穿初始数字集合中的每一个元素，直到我们到达最后一个。12 之后，我们代表 15。我们知道它大于 9，所以向右移动。我们知道它大于 12，所以向右移动。然后我们看到还没有合适的孩子，所以 15 成为合适的孩子，以此类推。我们现在有了完整的二叉查找树，并且可以有效地遍历它，因为我们知道每个被访问的节点都不需要访问它的兄弟节点或它的任何后代节点。

```
let nodeK = {element: 19, rightChild: null, leftChild: null}let nodeJ = {element: 8, rightChild: null, leftChild: null}let nodeI = {element: 14, rightChild: null, leftChild: null}let nodeH = {element: 10, rightChild: null, leftChild: null}let nodeG = {element: 2, rightChild: null, leftChild: null}let nodeF = {element: 36, rightChild: null, leftChild: null}let nodeE = {element: 21, rightChild: nodeF, leftChild: nodeK}let nodeD = {element: 72, rightChild: null, leftChild: nodeE}let nodeC = {element: 15, rightChild: nodeD, leftChild: nodeI}let nodeB = {element: 12, rightChild: nodeC, leftChild: nodeH}let nodeA = {element: 4, rightChild: nodeJ, leftChild: nodeG}let rootNode = {element: 9, rightChild: nodeB, leftChild: nodeA}
```

上面的代码相当于下面的树:

```
 9 /      \ 4          12 /   \       /   \ 2       8   10     15 /   \ 14     72 / 21 /  \ 19    36
```

你可以看到，对于任何给定的节点，*它右边的所有*节点都比它大，而*它左边的所有*节点都比它小。如果我们想找到节点 72，我们只需从 9，到 12，到 15，然后到 72，因为我们知道 72 将*总是*在 9，12 和 15 的右边。

下周回来学习 JavaScript 中的递归。