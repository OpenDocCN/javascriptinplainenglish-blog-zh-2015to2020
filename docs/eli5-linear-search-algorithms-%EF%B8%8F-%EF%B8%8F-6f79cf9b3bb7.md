# ELI5:线性搜索算法🕵️‍♀️

> 原文：<https://javascript.plainenglish.io/eli5-linear-search-algorithms-%EF%B8%8F-%EF%B8%8F-6f79cf9b3bb7?source=collection_archive---------10----------------------->

先说算法。今天，我将分解(就像你 5 岁)什么是线性搜索算法，如何用 JavaScript 实现它，以及我们何时使用它。准备好了吗？我们走吧！

![](img/22d0ba6cd159cad0d05fbe9a4e1f5c7a.png)

Photo by [Macau Photo Agency](https://unsplash.com/@macauphotoagency?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 什么是线性搜索算法？➡️➡️➡️➡️

简单地说，线性搜索算法是一种在特定列表/数组中查找特定项目的方法。对于这个算法，我们要处理两件事:1)包含许多项的列表/数组；以及 2)我们正在寻找的特定项目。好的，线性搜索算法提供了一个框架，可以在给定的数据数组中找到我们要找的特定项目。

lemurs lining up

# 线性搜索算法的特征📝

*   最简单的算法
*   使用 for 循环
*   最适合用于搜索小数据集
*   寻找一种元素时最佳

# 代码:实现线性搜索⌨️

用 JavaScript 写的线性搜索算法是什么样子的？下面我给你写出来:

```
function linearSearchAlgo(array, itemToSearchFor){
  for(let i = 0; i < array.length; i++){
    if(array[i] === itemToSearchFor) return i;
  }
  return -1;
}
```

哇，这个算法又短又中肯。到底发生了什么？我们的函数接受两个参数:array 和 itemToSearchFor。我们函数的主体是一个简单的 for 循环。这意味着我们将从数组的起始位置(索引 0)开始，遍历每个索引位置，直到找到我们要找的项，或者遍历每个索引位置，但没有找到我们要找的项。

在 for 循环中，我们将当前项与 itemToSearchFor 进行比较。如果它们匹配，我们返回当前循环的索引位置。否则，如果它们不匹配，我们返回-1(或者我们可以返回 *null* ，或者*“对不起，我们找不到你要找的东西”*，或者 *false* ，或者真的*你可以返回任何你想要的东西来表示搜索失败*)。

![](img/393c9b715aaf64e44159fcfe43ee2461.png)

Photo by [Jon Tyson](https://unsplash.com/@jontyson?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 我们什么时候使用线性搜索？🧐

但是，一旦我们找到了它，我们想怎么处理它呢？有时，我们只想知道项目的索引位置。其他时候，我们希望确认数组中的项目存在(或不存在)。最后，我们可能需要在代码中的其他地方使用索引位置或数组中存在的项的知识。

当您需要在一小组项目中搜索时，以及当您只搜索一个项目时，此算法非常有用。如果你的数组比较大，或者你需要查找不止一个条目，那么我不推荐线性搜索算法。在这些情况下(更大的数组或搜索多个条目)，线性搜索算法变得非常低效。当事情变得更复杂时，会有更适合的算法。

# 结论💭

线性搜索算法易于实现和理解。然而，由于它是一个简单的遍历一系列条目的循环，这使得线性搜索算法效率低下。如果我们有一个包含 50，000 个元素的数组，而我们要找的元素在最后一个位置…在*最终*到达第 50，000 个元素之前，我们需要搜索每一个元素。这是很多额外的工作和时间，这就是为什么这个算法不是最好的。但是，当我们学习算法时，我们都需要从某个地方开始。我觉得哪怕是最基本的数据结构，懂了也是好的。

你可以在这里了解更多关于线性搜索数据结构[的知识，以及它的时间复杂度。](https://en.wikipedia.org/wiki/Linear_search)

# ELI5 算法系列📚

*   [ELI5:冒泡排序算法🛁](https://haleepagel.medium.com/eli5-bubble-sort-algorithms-%EF%B8%8F-78bbce018846)
*   [ELI5:线性搜索算法🕵️‍♀️](https://haleepagel.medium.com/eli5-linear-search-algorithms-%EF%B8%8F-%EF%B8%8F-6f79cf9b3bb7)
*   [ELI5:二分搜索法树🌲](https://haleepagel.medium.com/eli5-binary-search-trees-fd560f81b553)

感谢阅读！我叫 Halee Pagel(与 Cali Bagel 押韵)，是日本东京的一名软件工程师。你可以在推特上找到我，我主要用它来喜欢科技迷因和 MLB 的更新。✌️