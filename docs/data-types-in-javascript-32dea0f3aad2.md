# JavaScript 中的数据类型

> 原文：<https://javascript.plainenglish.io/data-types-in-javascript-32dea0f3aad2?source=collection_archive---------15----------------------->

## 数据类型介绍，JavaScript 中不同的数据类型及其操作(附代码示例)。

![](img/fa02704fb9193cb611e58e81e3647965.png)

Photo by [Clément H](https://unsplash.com/@clemhlrdt?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 什么是数据类型？

> **数据类型**是数据的属性，它告诉编译器或解释器程序员打算如何使用数据。

所有的计算机程序都涉及存储和处理数据。计算机只理解简单的指令和小块信息(或数据)。如果需要对该信息执行任何复杂的操作，则需要指定一个**“数据类型”**。因此，数据类型是数据的一种属性，有助于计算机对数据进行计算(如算术运算)。

# JavaScript 中不同数据类型的介绍

JavaScript 是一种松散类型的语言。也就是说，在 JavaScript 变量中，没有绑定到特定类型的数据。这种类型的编程语言通常被称为— **“动态类型”**编程语言。

为了对数据进行操作和计算，JavaScript 必须了解数据的类型。JavaScript 中有八种基本的数据类型，让我们用例子概括一下它们:

## 1.数字

JavaScript 中的 *number* 类型可以表示整数和十进制/浮点值。JavaScript 数字可以执行所有的算术运算，如:加法`+`、减法`—`、乘法`*`、除法`/`、取幂`**`、取模/余数`%`、递增`++` 和递减`--` 。

除了*数字之外，*还有**“特殊数字类型”——`NaN`(非数字)代表不正确或未定义的数学运算，`-Infinity`和`Infinity`代表数学上的[无穷大](https://en.wikipedia.org/wiki/Infinity)**

**numbers and their operations in javascript**

## **2.BigInt**

**JavaScript *数字*类型不能表示大于`(2^53-1)`或小于`-(2^53-1)`的数字。因此，使用 *bigint* 类型可以存储和安全地操作大整数。一个 *bigint* 可以简单地通过在一个数字的末尾添加一个`n`来创建。**

**bigints and their operations in javascript**

## **3.线**

**一个*字符串*类型是一个由**“引号”**包围的字符序列。一个报价可以是双`""`，单`''`。JavaScript 中没有特殊的*字符*类型`char` 。即使引号中的单个字符值也被视为一个*字符串*类型。**

**strings and their operations in javascript**

## **4.布尔代数学体系的**

**一个*布尔*类型可以表示两个值中的任意一个:**真**或**假**。布尔值通常表示“是”或“否”。它们在编程中用于执行表达式、逻辑运算(AND/OR/NOT)、条件语句(if…else)和控制流(循环)的比较。**

**boolean and their operations in javascript**

## **5.不明确的**

**如果 JavaScript 中的一个变量从未被**【初始化】**或一个**【值未被赋值】**，那么这个变量就被称为*未定义。***

**undefined and their operations in javascript**

## **6.空**

**JavaScript 中的一个*空值*是它自己的一种类型，代表— **“空”**、**“空”**或**“值未知”**。**

**null and its operation in javascript**

## **7.物体和符号**

**JavaScript 中的对象用于存储各种数据的集合，其类型由*对象给出。*JavaScript 中的对象基本上是能够存储更复杂数据的**“键-值”**对的集合。**

***符号*类型用于创建对象的唯一标识符。符号是*唯一的*和*不可变的*数据类型。它们主要用作唯一属性键，因为符号永远不会与任何其他属性(符号或字符串)冲突。**

**objects and symbols its operation in javascript**

# **结论**

**这总结了不同的数据类型以及它们在 JavaScript 中的操作。别忘了自己尝试一下 gists 中提供的例子。另外， [MDN web 文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures)给出了更多关于这个主题的详细信息(解释了基本类型和非基本类型),请查看。**

> ****下期见，祝好运，编码愉快！****

# ****简明英语笔记****

**你知道我们推出了一个 YouTube 频道吗？我们制作的每个视频都旨在教给你一些新的东西。点击这里 查看我们，并确保订阅该频道😎**