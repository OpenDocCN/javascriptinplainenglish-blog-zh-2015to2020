# JavaScript 中优先级队列的介绍

> 原文：<https://javascript.plainenglish.io/introduction-to-priority-queues-in-javascript-30cfc49b01ee?source=collection_archive---------11----------------------->

## JavaScript 数据结构系列的第 8 部分

![](img/e8f5c0e3f27b0e43d45b4c564e7485e5.png)

Photo by [Adrien Delforge](https://unsplash.com/@adriendlf?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在本文中，我们将了解优先级队列，并使用 JavaScript 构建一个优先级队列。如果您不熟悉二进制堆，我建议您先看看下面关于如何创建二进制堆的文章。本文和我们的优先级队列实现就是建立在这一知识之上的。

[](https://medium.com/javascript-in-plain-english/how-to-create-a-binary-heap-in-javascript-e1e6f6446ff9) [## 如何用 JavaScript 创建二进制堆

### JavaScript 数据结构系列的第 7 部分

medium.com](https://medium.com/javascript-in-plain-english/how-to-create-a-binary-heap-in-javascript-e1e6f6446ff9) 

# 什么是优先级队列？

根据维基百科的说法，优先级队列是一种抽象数据类型，类似于常规队列或堆栈数据结构，其中每个元素都有一个与之相关联的“优先级”。这是什么意思？基本上，数据结构中的每个元素都有一个优先级，优先级最高的元素最先被服务。

例如，当您扫描大量数据并且需要报告最高优先级的项目时，优先级队列会很有用。优先级队列的另一个常见用途是在图形搜索算法中，比如[吉克斯特拉的算法](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm)。一个真实的例子是在医院里。假设一个病人带着一个需要立即治疗的重伤来了。这个病人将比另一个有小剪纸的病人或另一个有轻微头痛的病人有更高的优先权。

# 实现优先级队列

有几种方法可以实现优先级队列。最常见的是使用二进制堆。我们将更详细地讨论这个实现，但在此之前，让我们先来看看一个使用 list 的简单实现。

```
[{priority: 1}, {priority: 3}, {priority: 4}, {priority: 2}]
```

我们可以使用一个列表或数组来存储我们的优先级队列。上面的例子向我们展示了一个包含 4 个项目的列表，每个项目都有不同的优先级。这个实现的问题是，每当我们想要返回具有最高优先级的元素时，我们每次都需要遍历整个列表。随着数据集的增长，这可能会变得非常昂贵。这个问题的解决方案是使用二进制堆来实现我们的优先级队列。通过使用二进制堆，我们将把用于插入和移除的大 O 保持为 O(log n)。

# 使用二进制堆的优先级队列

为了用 JavaScript 构建我们的优先级队列，我们将首先创建一个优先级队列类和一个节点类。我们的优先级队列类将类似于我们在上一篇文章中创建的二进制堆类，只有一个属性被初始化为空数组。我们的节点类将接受两个参数，一个表示节点的值，一个表示优先级。

```
class PriorityQueue {
  constructor() {
    this.values = [];
  }
}class Node {
  constructor(value, priority) {
    this.value = value;
    this.priority = priority;
  }
}
```

接下来，我们将向优先级队列类添加入队(插入节点)和出列(移除节点)方法。这类似于我们在二进制堆中的实现，除了一些细微的变化。

1.  我们将方法的名称从 insert 和 extract max 改为 enqueue 和 dequeue。
2.  在我们的 enqueue 方法中，我们创建了一个具有值和优先级属性的新节点。
3.  我们不是像在二进制堆中那样比较值，而是比较优先级。
4.  我们将使用最小二进制堆而不是最大二进制堆，所以我们将改变一些操作符。因此，优先级值越低，优先级越高。

## 实现排队

首先，我们将创建一个带有值和优先级参数的新节点，并将该节点放入值数组中。然后我们将开始我们的冒泡效应，比较节点的优先级，直到我们的节点在二进制堆中的正确位置。

Implementing the enqueue method

## 实现出列

dequeue 方法将返回具有最高优先级的节点，但是我们也需要保持我们的堆处于正确的顺序。因此，我们将把值数组中的最后一个节点设置为新的头部，并使用向下冒泡效果。这将比较每个节点的优先级，直到它处于正确的位置。

Implementing the dequeue method

我们现在可以使用我们的优先级队列类以及入队和出队方法。记住，当我们调用 enqueue 方法时，我们需要传递两个参数，一个值和一个优先级。当我们调用 dequeue 方法时，它将返回具有最高优先级的元素(在我们的例子中，它将是具有最低优先级值的元素，因为我们使用的是 min 二进制堆)。

感谢阅读！请继续关注下一篇文章，我们将研究散列表。

想要更多吗？查看下面的文章，了解使用 JavaScript 的深度优先搜索和广度优先搜索。

[](https://medium.com/javascript-in-plain-english/tree-traversal-with-javascript-29b57d61d486) [## 用 JavaScript 遍历树

### JavaScript 数据结构系列的第 6 部分

medium.com](https://medium.com/javascript-in-plain-english/tree-traversal-with-javascript-29b57d61d486)