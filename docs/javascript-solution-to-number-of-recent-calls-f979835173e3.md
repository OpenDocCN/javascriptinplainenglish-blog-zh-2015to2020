# 最近通话次数的 JavaScript 解决方案

> 原文：<https://javascript.plainenglish.io/javascript-solution-to-number-of-recent-calls-f979835173e3?source=collection_archive---------6----------------------->

![](img/6174aeedf9e7706c88c69b31ebffa41c.png)

Photo by [Levi Jones](https://unsplash.com/@levidjones?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

[**最近通话次数**](https://leetcode.com/problems/number-of-recent-calls/) 是 leetcode 的一个问题，把我们绊倒了。(你可以看到它有多少反对票。🧐)在下面的博客中，我将试着一行一行地解释这个问题和例子，让它更容易理解。

***问题解释***

写一个**类** `RecentCounter`来统计最近的请求。

*上面的重点是类，我们需要实现一个类，而不是一个函数。*

它只有一个方法:`ping(int t)`，其中 ***t*** 以毫秒为单位表示某个时间。

*类实例需要有****ping****方法，这是一个以****t****`*1 <= t <= 10^9*`*范围内的整数作为参数的函数。**

*返回从 3000 毫秒前到现在已经完成的`ping`的数量。*

*****ping****方法返回值是整数。***

**任何时间在`[t - 3000, t]`的 ping 都将被计算在内，包括当前的 ping。**

***任何被调用过的* ***以前的 t*** *，在[* ***当前 t*** *-3000，* ***当前 t*** *]范围内的，都会被统计。***

**保证对`ping`的每次调用都使用比以前严格更大的`t`值。**

***每调用一次****ping****，自变量***增加，就形成一个升序的整数列表。****

****例 1:****

```
****Input:** inputs = ["RecentCounter","ping","ping","ping","ping"], 
/*
first, we create a new RecentCounter object
obj = new RecentCounter()
then, we call **ping** four times with four different **t** */inputs = [[],[1],[100],[3001],[3002]]
**/***
obj.ping(1) 
obj.ping(100)
obj.ping(3001)
obj.ping(3002)
*/**Output:** [null,1,2,3,3]/*
object initialized, ping is not called, no return value, **null** obj.ping(1) should return 1
obj.ping(100) should return 2
obj.ping(3001) should return 3
obj.ping(3002) should return 3
*/**
```

****注:****

1.  **每个测试用例最多会有对`ping`的`10000`调用。**
2.  **每个测试用例将调用`ping`，并严格增加`t`的值。**
3.  **对 ping 的每个调用都有`1 <= t <= 10^9`。**

*****解答说明*****

**让我们从最简单的开始，用一个 ***ping*** 的方法创建一个类。**

**[https://gist.github.com/GAierken/5c96cfe365b63a24f560714b0d5f8f66](https://gist.github.com/GAierken/5c96cfe365b63a24f560714b0d5f8f66)**

**现在我们有了框架。根据问题和例子，我们需要一个变量来存储 **t，**因为它会出现多次，当我们初始化对象时，最好有一个数据类型，比如数组。**

**[https://gist.github.com/GAierken/2813b3f4dcb81797e625bc8160310bd0](https://gist.github.com/GAierken/2813b3f4dcb81797e625bc8160310bd0)**

**我们如何使用这个数组？我们只需要存储 3000 毫秒前到现在发生的 **t** ，这意味着先前存储的 **t** 应该大于或等于(当前 **t** -3000)。由于每个测试用例将调用`ping`并严格增加 **t** 的值，数组中存储的元素按升序排列。我们将新的 **t** 添加到末尾，并在 t-3000 的时间帧内保持数组的前端，当不满足时，我们移除前端元素。听起来熟悉吗？先进先出？是的，是排队。在我们过滤掉不在范围内的元素之后，我们返回队列的大小。**

```
**ping(1)
queue = [ 1 ] // 1ping(100)
queue = [ 1, 100 ]  // 2ping(3001)
queue = [ 1, 100, 3001 ] // 3ping(3002)
queue = [ 100, 3001, 3002 ] // 3**
```

**让我们实现代码。**

**[https://gist.github.com/GAierken/98b1ea82235da9f00dfea13345b95aa5](https://gist.github.com/GAierken/98b1ea82235da9f00dfea13345b95aa5)**

**ping 方法的时间复杂度为 **O(n)** 。**

**希望能对类似问题的解决有所帮助。**

*****资源:*****

**[](https://leetcode.com/problems/number-of-recent-calls/) [## 最近通话次数- LeetCode

### 写一个类 RecentCounter 来计算最近的请求。它只有一个方法:ping(int t)，其中 t 代表某个时间…

leetcode.com](https://leetcode.com/problems/number-of-recent-calls/) [](https://www.geeksforgeeks.org/queue-set-1introduction-and-array-implementation/) [## Queue | Set 1(简介和数组实现)- GeeksforGeeks

### 像堆栈一样，队列也是一个线性结构，它遵循特定的操作执行顺序。的…

www.geeksforgeeks.org](https://www.geeksforgeeks.org/queue-set-1introduction-and-array-implementation/) [](https://medium.com/datadriveninvestor/queue-in-javascript-e77ab51f6de0) [## JavaScript 中的队列

### JavaScript 中队列的实现

medium.com](https://medium.com/datadriveninvestor/queue-in-javascript-e77ab51f6de0)**