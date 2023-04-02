# 如何在 JavaScript 中克隆数组

> 原文：<https://javascript.plainenglish.io/3-ways-to-clone-arrays-in-javascript-3e80e050f11b?source=collection_archive---------15----------------------->

## 3 种用 JavaScript 克隆数组的有用方法，并附有实例

![](img/24d961a33311ee83e7bcbdf8a924c0e7.png)

Photo by [Per Lööv](https://unsplash.com/@perloov?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

JavaScript 中的数组用于在单个变量中存储多个值。它是一个特殊的变量，一次可以保存多个值。

作为一名程序员，您经常需要一种将现有数组元素复制到新数组的方法。但是如果你做得不好，就会出现一些问题。

在本文中，我们将发现一些用 JavaScript 克隆数组的有用方法。让我们开始吧。

![](img/ffcd3aaf0c42e3c80c5be92a4d531e5f.png)

Image Created with ❤️️ By [Mehdi Aoussiad](https://mehdiouss315.medium.com/).

在开始之前，让我们将一个阵列分配给另一个阵列，以便克隆或复制它。没关系，它可以工作，但是在 JavaScript 中，数组被认为是一个引用类型。这意味着如果我们把一个数组赋给另一个数组，它不会把值赋给一个新的数组。它只将引用分配给原始数组。因此，如果我们改变第二个数组中的元素，第一个数组中的元素也会受到影响。

看看下面的例子:

```
var fruits = ['Apple','Orange'];

var newFruits = fruits;

newFruits.push('Mango')

console.log(fruits);
//[ 'Apple', 'Orange', 'Mango' ]

console.log(newFruits);
//[ 'Apple', 'Orange', 'Mango' ]
```

如您所见，尽管我们只在`newFruits`中添加了‘Mango ’,但两个数组都发生了变化。所以，这对我们来说不是一个好的选择。这就是为什么我决定给你 3 种有效且有用的方法来克隆 JavaScript 中的数组。

# 1.切片法

方法`slice()`用于复制数组的片段。您也可以克隆整个阵列。

slice 方法选择从给定的 ***start*** 参数和 ***end*** at 参数开始的元素，但不包括给定的 *end* 参数。它有两个参数，开始和结束索引。

看看下面的例子:

```
// Cloning a part of an array.
var fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
var citrus = fruits.**slice(1, 3)**;
Console.log(citrus); // Output: ["Orange", "Lemon"]// Cloning a whole array.
var fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
var citrus = fruits.**slice()**;
console.log(citrus); // Output: ["Banana", "Orange", "Lemon", "Apple", "Mango"]
```

使用不带参数的方法`slice()`将克隆整个数组。

# 2.扩展运算符

扩展操作符`{…}`是 JavaScript 中添加的重要且有用的特性之一。我们可以用它来克隆数组，但请记住，它不适用于多维数组。

这里有一个例子:

```
var fruits = ['Apple','Orange'];

var newFruits = [**...fruits**];// The spread operator

console.log(newFruits); //[ 'Apple', 'Orange' ]newFruits.push('Mango');

console.log(fruits);
//[ 'Apple', 'Orange' ]

console.log(newFruits);
//[ 'Apple', 'Orange', 'Mango' ]
```

如您所见，使用 spread 操作符克隆数组要干净得多。

您可以查看我关于这个操作符的文章以获得更多信息。

[](https://medium.com/javascript-in-plain-english/5-useful-things-the-spread-operator-can-do-in-javascript-f0306358bc9c) [## JavaScript 中 Spread 操作符可以做的 5 件有用的事情

### 用 Spread 操作符编写一个更好更干净的 JavaScript 代码

medium.com](https://medium.com/javascript-in-plain-english/5-useful-things-the-spread-operator-can-do-in-javascript-f0306358bc9c) 

# 3.来自的方法数组

静态方法`**Array.from()**`从类似数组或可迭代的对象创建一个新的浅拷贝实例`Array`。浅复制数组基本上意味着只有第一级元素会被复制，它不适用于多维数组。

看看下面的例子:

```
var fruits = ['Apple','Orange'];

var newFruites = **Array.from**(fruits);

newFruites.push('Mango')

console.log(fruits);
//[ 'Apple', 'Orange']

console.log(newFruites);
//[ 'Apple', 'Orange', 'Mango' ]
```

如您所见，使用这种有用的方法，复制数组变得更加容易。

# 结论

现在用 JavaScript 克隆数组变得更加容易和简洁。希望您对 Javascript 中复制数组的不同方式有了更好的了解。现在你可以决定哪种方法适合你。

感谢您阅读本文，希望您觉得有用。

# 更多阅读

[](https://medium.com/swlh/7-javascript-features-you-need-to-know-before-learning-react-e77c3b3481d8) [## 学习 React 之前你需要知道的 7 个 JavaScript 特性

### 在开始使用 React 之前，学习您需要的基础知识

medium.com](https://medium.com/swlh/7-javascript-features-you-need-to-know-before-learning-react-e77c3b3481d8)