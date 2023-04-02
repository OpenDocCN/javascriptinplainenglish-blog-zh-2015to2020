# 在谷歌面试——电话面试问题

> 原文：<https://javascript.plainenglish.io/google-phone-interview-question-f13e9e4d67a8?source=collection_archive---------2----------------------->

一位经验丰富的初级开发人员最近向我讲述了他们在谷歌的面试经历。不幸的是，他们在电话采访后没有前进👎。

![](img/aa35a72849b4eae0f2fdbd93ce208e6f.png)

Source: roberthalf.com

上周，我在一次[亚马逊采访](https://medium.com/javascript-in-plain-english/amazon-software-development-engineer-interview-questions-a40a6b092c3a)中发布的难题得到了很多高质量的评论/讨论。所以，对我来说，看看这个星期谷歌提问的进展是有意义的。

## 困难的问题

Median 是有序整数列表中的中间值。如果列表的大小是偶数，则没有中间值。所以中位数是两个中间值的平均值。

举个例子，

`[2,3,4]`，中位数为`3`

`[2,3]`，中位数是`(2 + 3) / 2 = 2.5`

设计支持以下两种操作的数据结构:

*   void addNum(int num) —将数据流中的整数添加到数据结构中。
*   double find median()-返回到目前为止所有元素的中值。

**举例:**

```
addNum(1)
addNum(2)
findMedian() -> 1.5
addNum(3) 
findMedian() -> 2
```

如果你能解决这个问题，就要为接下来的问题做好准备

*   如果流中的所有整数都在 0 到 100 之间，你会如何优化它？
*   如果流中 99%的整数都在 0 到 100 之间，你会如何优化它？

如您所见，这些问题可以深入探究。

我采访过的有经验的初级开发人员告诉我，他们花了整整 45 分钟打电话。通常谷歌会在电话面试中问 2-3 个问题。这可能是他们没有向☹️.前进的原因

如果你觉得上面的问题很难，试试这个问题，它应该更容易，但概念相似。

## 练习用的简单问题

给定一个整数流和一个窗口大小，计算滑动窗口中所有整数的移动平均值。

**示例:**

```
MovingAverage m = new MovingAverage(3);
m.next(1) = 1
m.next(10) = (1 + 10) / 2
m.next(3) = (1 + 10 + 3) / 3
m.next(5) = (10 + 3 + 5) / 3
```

试着回答这些问题，并请就你将如何进行这次电话面试发表评论。请记住，面试官喜欢听听你的思维过程，然后才开始解决一个算法问题。

【JavaScript 用简单英语写的一句话:我们总是对帮助推广优质内容感兴趣。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的 Medium 用户名给我们发邮件到[submissions@javascriptinplainenglish.com](mailto:submissions@javascriptinplainenglish.com)，我们会把你添加为作者。