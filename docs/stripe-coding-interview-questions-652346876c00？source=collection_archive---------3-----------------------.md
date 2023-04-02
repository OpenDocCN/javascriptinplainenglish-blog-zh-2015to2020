# 条纹编码面试问题

> 原文：<https://javascript.plainenglish.io/stripe-coding-interview-questions-652346876c00?source=collection_archive---------3----------------------->

## 通过每天解决一个问题，变得非常擅长编写面试代码

![](img/6cd713d26fc7a9aba8cf2edffd0d9da5.png)

# 日常编码问题

它们是受真实编程面试启发的各种各样的问题，带有深入的解决方案，清晰地带您了解每个核心概念。

> 通过每天解决一个问题，变得格外擅长编写面试代码。

我们将一起使用 JavaScript 解决这些问题。

# 问题#1

## 问题

```
Given an array of integers, find the first missing positive integer in linear time and constant space. In other words, find the lowest positive integer that does not exist in the array. The array can contain duplicates and negative numbers as well.
```

例如:

```
The input [3, 4, -1, 1] should give 2\. 
The input [1, 2, 0] should give 3.
```

您可以就地修改输入数组。

## 解决办法

首先，我们需要创建一个随机数据集来测试我们的解决方案。

那么，下面是我的解决方案。

让我们测试一下我的解决方案。

```
var arr = [3, 4, -1, 1]
findFirstMissingPos(arr)
**<< 2**var arr = [1, 2, 0]
**<< 3**
```

很简单，对吧？

我将在本文中更新 Stripe 提出的新问题，请🔖它重新阅读并获得最新的问题和解决方案。

感谢阅读😘，再见👋，别忘了👏最多 50 次并跟随！