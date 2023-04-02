# JavaScript 算法:如何对排序后的数组求平方

> 原文：<https://javascript.plainenglish.io/javascript-algorithm-how-to-square-a-sorted-array-f2410580aa09?source=collection_archive---------6----------------------->

![](img/fcacc0dec09ecb44495f9e99d16b151b.png)

Photo by [Hello I'm Nik 🎞](https://unsplash.com/@helloimnik?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# **问题**

给定一个按非递减顺序排序的整数数组`A`，返回每个数字的平方数组，也是按非递减顺序排序。

**例 1:**

```
**Input:** [-4,-1,0,3,10]
**Output:** [0,1,9,16,100]
```

**例 2:**

```
**Input:** [-7,-3,2,3,11]
**Output:** [4,9,9,49,121]
```

**注:**

1.  `1 <= A.length <= 10000`
2.  `-10000 <= A[i] <= 10000`
3.  `A`按非降序排序。

# **解决方案**

*蛮力*

首先，遍历给定的数组，并返回平方元素。如例一所示，[-4，-1，0，3，10]变成了[16，1，0，9，100]。在我们将所有元素平方后，我们可以使用 JavaScript array `**sort()**`方法将转换后的数组按升序就地排序。

[https://gist.github.com/GAierken/851844056ae45cbb1d3da04732f72fac](https://gist.github.com/GAierken/851844056ae45cbb1d3da04732f72fac)

上述方案的时间复杂度为 **O(n log n)** ，并不理想。我们如何优化解决方案？问题陈述了给定的数组是按升序排序的。我们可以用它来改善时间复杂度吗？

*双指针*

按升序排序的给定数组很容易看出，按绝对值计算，最大的数字在给定数组的开头和结尾，值向中间递减。所以我们可以考虑两个指针的方法。一个从开始，一个从结束。

[https://gist.github.com/GAierken/f7dd2583c4419e5faa5de08110fe61ef](https://gist.github.com/GAierken/f7dd2583c4419e5faa5de08110fe61ef)

这两个指针的时间复杂度为 **O(n)** 。

*资源:*

[](https://leetcode.com/problems/squares-of-a-sorted-array/) [## 排序数组的平方- LeetCode

### 提高你的编码技能，迅速找到工作。这是扩展你的知识和做好准备的最好地方…

leetcode.com](https://leetcode.com/problems/squares-of-a-sorted-array/) [](https://www.geeksforgeeks.org/two-pointers-technique/) [## 两点技巧-极客之福

### 两个指针确实是一种简单有效的技术，通常用于在一个有序的

www.geeksforgeeks.org](https://www.geeksforgeeks.org/two-pointers-technique/)