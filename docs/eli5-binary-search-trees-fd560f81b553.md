# ELI5:二分搜索法树🌲

> 原文：<https://javascript.plainenglish.io/eli5-binary-search-trees-fd560f81b553?source=collection_archive---------11----------------------->

先说算法。特别是二叉查找树算法。在这篇文章中，我将解释(比如你 5 岁)什么是二叉查找树，如何实现它，以及何时使用它。准备好了吗？我们走吧！

![](img/3b717e15d01b1f612cccb312783222da.png)

Photo by [Hector Falcon](https://unsplash.com/@hectorfalcon?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 什么是二叉查找树？🌲

二叉查找树是一种有序的树形数据结构。这是一种根据一组“大于/小于”规则对数据进行排序的便捷方式。

树是由称为节点的东西组成的。每当您向树中添加新数据时，它就开始排序之旅，从根节点开始，沿着树向下，直到找到一个空位置插入。很快会有更多的技术术语。

New nodes finding a home in our binary search tree

换句话说，当你添加一个新的数据到你的树中时，它会寻找一个空的位置来建立它的家。把你的新数据想象成一个人想要在一个完美的地点建造他们的家。你有一些规则可以遵循，以找到你的完美家园地段。一个接一个，新的数据(也就是我们正在寻找一个称之为家的地方的人)基于“我，这个新的数据点，是大于还是小于这个当前的家？”

这种比较产生两种选择:

1.  如果比当前房屋少，它会沿着左边的街道检查下一个地块是否是空的。如果下一个地段是空的，我们的*新的*数据点将在那里安家。如果下一个地块被占用，它继续与下一个住宅进行比较，直到找到一个空的地块。

2.或者，如果它比当前房屋大，它会沿着正确的街道检查下一个地块是否是空的。如果下一个地段是空的，我们的*新的*数据点就在那里安家。如果下一个批次被占用，它将继续进行比较，直到找到一个空批次。

# 二叉查找树的特征📝

*   有一个根节点
*   有父节点、兄弟节点和子节点
*   父节点可以有 0、1 或 2 个子节点。
*   左边的子节点总是小于它们的父节点
*   右边的子节点总是大于它们的父节点
*   左侧的同级节点总是小于当前节点
*   右边的同级节点总是大于当前节点
*   根节点是最大值节点

```
// EXAMPLE OF A BINARY SEARCH TREE//          22         << 22 is the root node//         /  \//        19   100     << 19 & 100 are children of 22 & parent nodes//       / \   / \//      11 20 53 250  << 11 has one child, 20/53/250 have none//      ///     5
```

# 代码:实现⌨️二叉查找树

让我们使用 JavaScript 和 ES6 语法写出我们的代码。我们将在一个类中编写所有内容。

# 1.声明我们的类构造函数

首先，我们声明节点类，并在其中编写构造函数:

```
class Node {
  constructor(data) {
   this.data = data;
   this.left = null;
   this.right = null;
  }
}
```

我们树上的每个节点都有三条重要信息:

1.  this.data =节点的实际数据
2.  this.left =当前节点左侧的同级节点的信息
3.  this.right =当前节点右侧的同级节点的信息

这三条信息创建了节点本身，并为树中的其他节点提供了重要的上下文和连接。当我们遍历树时，这些连接将帮助我们。

我们将 this.left 和 this.right 设置为空值，因为我们没有将新创建的节点插入到我们的树中。那是我们的下一步！

# 2.创建“插入”方法

好了，让我们将新创建的节点插入到二叉查找树中。为此，我们需要在我们的类中创建一个方法。

```
class Node {// oh look, it's the code we just wrote!constructor(data) {
    this.data = data;
    this.left = null;
    this.right = null;
  }// hello, new "insert" method!insert(data) {
    if (data < this.data && this.left) {
      this.left.insert(data);
    } else if (data < this.data) {
      this.left = new Node(data);
    } if (data > this.data && this.right) {
      this.right.insert(data);
    } else if (data > this.data) {
      this.right = new Node(data);
    }
  }
}
```

查看我们新的“insert”方法代码，我们看到它依赖于将未排序的节点与树中的每个节点进行比较，直到找到最佳位置。我们想看看传入的数据是否大于/小于当前节点的数据。如果它小于当前节点，我们检查左边的子节点位置，如果它未赋值，我们在那里插入新数据。如果它已经被分配，我们再次开始比较过程，并使用左边的子节点作为当前节点。未排序的数据会将其自身插入/分配到第一个可用且有效的未结位置。

# 3.创建“包含”方法

我们已经创建了自己的类和一个向二叉查找树中插入新数据的方法。现在我们想检查特定的数据是否包含在我们的树中。我们如何做到这一点？再写一个方法！

```
class Node {// new node constructor!constructor(data) {
    this.data = data;
    this.left = null;
    this.right = null;
  }// "insert" method!insert(data) {
    if (data < this.data && this.left) {
      this.left.insert(data);
    } else if (data < this.data) {
      this.left = new Node(data);
    } if (data > this.data && this.right) {
      this.right.insert(data);
    } else if (data > this.data) {
      this.right = new Node(data);
    }
  }// let's write our "contains" method below!contains(data) {
    if (this.data === data) {
      return this;
    } if (data < this.data && this.left) {
      return this.left.contains(data);
    } else if (data > this.data && this.right) {
      return this.right.contains(data);
    } else {
      return null;
    }
  }
}
```

如果您注意到，我们的“包含”方法与“插入”方法非常相似。我们从一个数据点开始，遍历我们的树，将该数据点与左边和右边的节点进行比较。不同的是我们的基本情况。“包含”方法的基本情况是当我们成功地找到我们正在寻找的数据时:

```
if (this.data === data) {
      return this;
    }
```

或者，当我们搜索了整个树但没有找到数据时:

```
else {
      return null;
    }
```

# 我们什么时候用二分搜索法树？🧐

每当您想要使用大于/小于方法比较数据时，此算法都很有用。如果您有一组数字，可以使用二叉查找树对它们进行排序。另一个例子，如果你有一个名单，你想按字母顺序排序，二叉查找树是一个很好的选择。

# 结论💭

二分搜索法树是一种快速排序和搜索数据集的方法，因为数据是根据二元规则组织的。你可以在这里了解更多关于二叉查找树数据结构[的知识，以及它在大 O 符号中的时间复杂度。](https://en.wikipedia.org/wiki/Binary_search_tree)

# ELI5 算法系列📚

*   [ELI5:冒泡排序算法🛁](https://haleepagel.medium.com/eli5-bubble-sort-algorithms-%EF%B8%8F-78bbce018846)
*   [ELI5:线性搜索算法🕵️‍♀️](https://haleepagel.medium.com/eli5-linear-search-algorithms-%EF%B8%8F-%EF%B8%8F-6f79cf9b3bb7)
*   [ELI5:二分搜索法树🌲](https://haleepagel.medium.com/eli5-binary-search-trees-fd560f81b553)

感谢阅读！我叫 Halee Pagel(与 Cali Bagel 押韵)，是日本东京的一名软件工程师。你可以在推特上找到我，我主要用它来喜欢科技迷因和 MLB 的更新。✌️