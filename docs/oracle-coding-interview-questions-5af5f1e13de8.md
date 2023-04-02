# Oracle 编码面试问题

> 原文：<https://javascript.plainenglish.io/oracle-coding-interview-questions-5af5f1e13de8?source=collection_archive---------1----------------------->

## 通过每天解决一个问题，变得非常擅长编写面试代码

![](img/247d57485d4fd23c68a7ff0fb297e597.png)

[a photo by Oracle](https://www.glassdoor.com/Photos/Oracle-Office-Photos-IMG3981015.htm)

# 日常编码问题

它们是受真实编程面试启发的各种各样的问题，带有深入的解决方案，清晰地带您了解每个核心概念。

> 通过每天解决一个问题，变得格外擅长编写面试代码。

我们将一起使用 JavaScript 解决这些问题。

# 问题#1

## 问题

如果一个数的二进制表示中没有相邻的数，我们说这个数是稀疏的。

```
For example, 21 (10101) is sparse, but 22 (10110) is not. For a given input N, find the smallest sparse number greater than or equal to N.
```

以比`O(N log N)`更快的速度完成这项工作。

## 解决办法

首先，把一个整数转换成二进制，如下。

然后我们实现下面的`get_next_sparse(num)`函数，返回大于或等于`num`的最小稀疏数。

我们遍历一个字符串数组，当当前数字和前一个数字等于字符串 1 时中断，然后创建一个 0 数组，如下所示。

最后，这个解决方案的输出。

```
console.log(get_next_sparse(21))
<< 21console.log(get_next_sparse(25))
<< 32
```

很简单，对吧？

我将在本文中更新 Oracle 提出的新问题🔖它重新阅读并获得最新的问题和解决方案。

感谢阅读！