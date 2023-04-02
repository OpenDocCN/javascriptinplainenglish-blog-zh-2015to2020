# JavaScript 大 O 符号分解

> 原文：<https://javascript.plainenglish.io/javascript-big-o-notation-breakdown-5f3aeb809749?source=collection_archive---------16----------------------->

![](img/250764c4b2bf4ff1975c5f7b1f6f19b9.png)

Photo by [Kaleidico](https://unsplash.com/@kaleidico?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

大 O 是一个度量标准，用于衡量算法执行所需的步骤数与输入之间的关系。它衡量算法的模式或趋势，以及当输入接近无穷大时，算法在最坏情况下的表现。换句话说，Big-O 解决了一个问题“当我的输入增加时，我的算法/代码将如何表现？”。

Big-O 符号很重要，因为它显示了你的算法/代码有多高效。在这篇文章中，我们将讨论时间复杂性，以及如何测量算法的时间复杂度。

## O(1) —常数时间

恒定时间意味着无论输入什么，运行算法的时间总是一样的，所以它是恒定时间。

我们可以看到，无论什么数被馈入函数，都只有一个操作需要计算机来执行。计算机不在乎你把什么数字加在一起，10 + 20 还是 10 亿+10 亿，因为它可以在大致相同的时间内运行一个基本运算(如加、减、除、乘)，而不管输入如何。

**常数时间复杂度示例**:总是返回数组第一个(或*第 n 个*)元素的算法；**常量空间复杂性示例**:变量声明

## O(n) —线性时间

线性时间假设时间复杂度将随着输入线性增长。一个很好的例子是考虑一个在输入数组上循环的算法。循环输入所需的步数取决于数组的长度。如果数组有十个元素，代码块将需要执行十次，随着输入线性增长。

**线性时间复杂度示例**:针对循环；while 循环通过 *n 个*输入；**线性空间复杂度示例**:数组；混杂

## O(n ) —二次时间

二次时间实际上是属于多项式时间的特例。

当运算数量相对于输入以二次方式增长时，算法具有二次时间。二次增长意味着当输入为 2 时，执行的操作数为 4，4 个输入意味着 16 次执行，5 个输入是 25 次执行， *n* 个输入意味着 *n* 次执行。这将意味着对于一个输入为 *n* 的操作，你将有 *n* 个操作。

**二次时间复杂度示例**:嵌套循环——代码块，集合中的每个元素都要与其他元素进行测试/比较。

这三个例子只是时间/空间复杂性的一部分，可用于分析代码和计算最坏情况的运行时。查看下面的参考资料，了解更多关于编码和构建可伸缩性代码的精彩内容。

## 其他资源

[](https://www.bigocheatsheet.com/) [## 了解你的复杂性！

### 你好。这个网页涵盖了计算机科学中常用算法的空间和时间复杂性。当…

www.bigocheatsheet.com](https://www.bigocheatsheet.com/) [](https://www.interviewcake.com/article/javascript/big-o-notation-time-and-space-complexity) [## 大 O 批注|面试蛋糕

### 最后简单解释一下大 O 记数法。我会向你展示你在技术面试中所需要的一切…

www.interviewcake.com](https://www.interviewcake.com/article/javascript/big-o-notation-time-and-space-complexity)