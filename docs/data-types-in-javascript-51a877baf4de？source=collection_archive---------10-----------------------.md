# JavaScript 数据类型初学者指南

> 原文：<https://javascript.plainenglish.io/data-types-in-javascript-51a877baf4de?source=collection_archive---------10----------------------->

## 所有的编程语言都有内置的数据结构，但是每种语言的数据结构都不一样。JavaScript 也有自己的数据类型。这些可以用来构建其他数据结构。

![](img/ae7418344003344e172bb3ecab1207be.png)

Photo by [Ferenc Almasi](https://unsplash.com/@flowforfrank?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# **1)原始值**

原始值是数字和字符串等。它只能包含一个东西(可以是字符串、数字或其他)。

## 数字

```
let n = 231;
n = 221;
```

JavaScript 中的数字类型表示整数和浮点数。数字类型有许多运算，例如乘法(*)、除法(/)、加法(+)、减法(-)，等等。

## BigInt

```
// "n" at the end of the value means it's a BigInt typeconst bigInt = 123456234589012345677868923456789234234567890n;
```

BigInt 值是通过将 n 追加到整数的末尾来创建的。在 JavaScript 中，“数字”类型不能表示大于(2^53–1)(即 9007199254740991)或小于-(2^53–1 的负值。这是由它们的内部表示引起的技术限制。

## 线

```
let str1 = "Hi";
let str2 = 'Hello World';
let str3 = `It can contain another variable ${str1}`
```

在 JavaScript 中，有 3 种类型的引号。

1.  双引号:`"Hi"`。
2.  单引号:`'Hello World'`。
3.  反斜线:``Hello``。

双引号和单引号是“简单”的引号。在 JavaScript 中，它们之间几乎没有区别。

反斜线是“扩展功能”引号。它们允许我们通过将变量和表达式包装在`${…}.`中来将它们嵌入到字符串中

## 布尔代数学体系的

```
let isChecked = true;
isChecket = false;
```

布尔类型只有两个值:`true`和`false`。

这种类型通常用于存储“是”或“否”值:`true`表示“是，正确”，而`false`表示“否，不正确”。

## 空

```
let name = null;
let data = null;
```

“null”是 JavaScript 中的一个特殊值。它用于故意缺失值。它只是一个特殊的值，表示“无”、“空”或“值未知”。

## 不明确的

```
let name;
console.log(name);// Result: undefined
```

“undefined”自成一种类型，就像“null”一样。undefined 的意思是“没有赋值”。如果一个变量被声明，但没有被赋值，那么它的值是未定义的。“undefined”用于意外丢失的值。

## 标志

`symbol`类型用于创建对象的唯一标识符。为了完整起见，我不得不在这里提一下。

# 2)目标和功能

“对象”是 JavaScript 中的一种特殊类型。所有其他类型都被称为“原语”，因为它们的值只能包含一个东西(可以是字符串、数字或其他)。相反，对象用于存储数据集合和更复杂的实体。

## 目标

```
const person = {name: 'Mugdho', age: 16};
```

对象和函数是特殊的，因为我们可以从我们的
代码中操纵它们。对象用于对相关数据和代码进行分组。使用*对象*我们可以创建复杂的东西。

## 功能

```
function print(){
    console.log("Hello World")
}
```

*功能*用于引用代码。我们可以随时使用函数来完成复杂的任务。简而言之，函数是一组可重用的代码，可以在程序中的任何地方调用。

现在你可能会问，“我用过的其他类型在哪里，比如数组？”。**在 JavaScript 中，没有其他的基本值类型。**剩下的都是对象！例如，甚至数组、日期和正则表达式从根本上来说都是 JavaScript 中的对象。

这对你有帮助吗？请在下面的评论中告诉我你对这篇文章的想法！