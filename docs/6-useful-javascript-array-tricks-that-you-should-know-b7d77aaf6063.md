# 您应该知道的 6 个有用的 JavaScript 数组技巧

> 原文：<https://javascript.plainenglish.io/6-useful-javascript-array-tricks-that-you-should-know-b7d77aaf6063?source=collection_archive---------3----------------------->

## JavaScript 数组技巧和实际例子

![](img/157336ece1be91a73713f917514ff8f9.png)

Photo by [Mimi Thian](https://unsplash.com/@mimithian?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

数组是 Javascript 的一个常见概念，它给我们提供了很多处理存储在其中的数据的可能性。总的来说，数组是 Javascript 中最基本的主题之一，您在编程之初就要学习。

在本文中，我们将向您展示一些您可能不知道的有用且强大的数组技巧。让我们开始吧。

![](img/13563063e0199f42cf9cfb1f9a799662.png)

Image created with ❤️️ By author.

# 1.将数组转换为对象

有时，您需要在 JavaScript 中将数组转换为对象。最有效的方法是使用*扩展操作符* `**{…}**` *。*

看看下面的例子:

```
var fruits = ["banana", "apple", "orange"];var fruitsObj = **{ ...fruits }**;console.log(fruitsObj);// Returns : {0: "banana", 1: "apple", 2: "orange"}
```

如你所见，如果我们使用 spread 操作符，这比听起来要简单得多。

# 2.用数据填充数组

有些情况下，您需要多次用相同的数据类型填充数组。你可以通过使用方法`fill`来实现。

这里有一个例子:

```
var newArray = **new Array(5).fill("Hi")**;console.log(newArray);// Returns: ["Hi", "Hi", "Hi", "Hi", "Hi"]
```

这个方法`fill()`可以接受任何数据类型作为参数(字符串、数字……)。

# 3.从数组中获取随机值

在许多情况下，程序的业务逻辑需要从数组中获取一些随机数据。数学方法`Math.random()`和`Math.floor()`对此有所帮助。

方法`Math.random`返回一个介于 0 和 1 之间的随机数。另一方面，方法`Math.floor`返回小于或等于给定数字的最大整数。

> 注意，你必须为数学方法写一个大写字母 M。

下面是我们如何使用这些方法获得随机数组的数据:

```
const colors = ["red", "pink", "yellow", "black", "blue"];const randomColor = colors[**Math.floor(Math.random() * colors.length)**];console.log(randomColor);// Returns a random color from the "colors" array.
```

正如您所看到的，上面的两个方法和属性`length`允许我们获得一个介于 0 和数组`colors` (4)的最后一个索引之间的随机数。我们把它们放在括号中，这样我们就可以从数组中得到随机数据。

# 4.从数组中删除重复项

有时，您需要从数组中删除重复的值。最简单的方法是使用对象 [set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set) 。这是编码面试中最常见的 JavaScript 问题之一。

看看下面的例子:

```
var fruits = ["banana", "apple", "orange", "watermelon", "apple", "orange", "grape", "apple"];/**/ First method:**var uniqueFruits = **Array.from(new Set(fruits))**;console.log(uniqueFruits); // returns [“banana”, “apple”, “orange”, “watermelon”, “grape”]**// Second method:**var uniqueFruits2 = **[ ...new Set(fruits)]**;console.log(uniqueFruits2); // returns [“banana”, “apple”, “orange”, “watermelon”, “grape”]
```

对象`set`使我们更容易从数组中删除重复的元素。它还保持了代码的整洁。

# 5.求两个数组的交集

假设你有两个数组，你想得到*两个数组中包含的值。*

您可以使用对象`set`从第一个数组中删除重复的值，然后使用方法`filter`从第一个数组中过滤掉第二个数组中包含的值。

下面是一个实现这一点的示例:

```
var numOne = [0, 2, 4, 6, 8, 8];var numTwo = [1, 2, 3, 4, 5, 6];var duplicateValues = **[...new Set(numOne)].filter(item => numTwo.includes(item))**;console.log(duplicateValues);// Returns [2, 4, 6]
```

# 6.使用长度来调整数组大小和清空数组

在 JavaScript 中，我们可以覆盖数组的长度。因此，我们将利用这一优势来调整大小和清空数组。

如果我们想调整数组的大小，下面是一个例子:

```
var entries = [1, 2, 3, 4, 5, 6, 7];  
console.log(entries**.length**); // 7  
entries.**length** = 4;  
console.log(entries.**length**); // 4  
console.log(entries); // [1, 2, 3, 4]
```

如果我们想清空数组，这里还有一个:

```
var entries = [1, 2, 3, 4, 5, 6, 7]; 
console.log(entries.**length**); // 7  
entries.**length** = 0;   
console.log(entries.**length**); // 0 
console.log(entries); // []
```

# 结论

如您所见，我们无需编写大量代码就完成了许多数组任务。使用这些数组技巧将使您的代码更加整洁，易于维护。

感谢您阅读本文，希望您觉得有用。如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**

# 更多阅读

[](https://medium.com/javascript-in-plain-english/5-amazing-front-end-development-tools-that-you-should-know-7372dc377d7) [## 你应该知道的 5 个惊人的前端开发工具

### 每个开发人员都应该知道的有用的前端开发工具

medium.com](https://medium.com/javascript-in-plain-english/5-amazing-front-end-development-tools-that-you-should-know-7372dc377d7)