# 使用扩展运算符编写干净的 JavaScript 的 5 种方法

> 原文：<https://javascript.plainenglish.io/5-ways-to-write-a-clean-javascript-using-the-spread-operator-f03a3d5f6340?source=collection_archive---------1----------------------->

## 带实例的扩散算子

![](img/83a6bed2cc0e906f9ddeafeb0a39d822.png)

Photo by [Mimi Thian](https://unsplash.com/@mimithian?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

ES6 引入了许多有用的特性，使得开发人员更容易编写 JavaScript。其中之一是 spread 运算符(`{…}`)，它基本上是在 JavaScript 中扩展或扩展可迭代对象的值。它还允许我们在需要多个参数或元素的地方扩展数组和其他表达式。因此，在许多情况下，spread 运算符对于以一种简单的方式编写更好更干净的 JavaScript 代码非常有用。

在本文中，我们将向您展示一些使用 spread 运算符编写更好的 JavaScript 的方法。让我们开始吧。

# 1.将字符串转换为数组

在 ES5 中，我们使用方法`split()`将字符串转换成数组。所以，我们得到了一个字符数组，如下图所示。

```
const string = 'word';
const arr = string.split('');console.log(arr); // Prints: ["w", "o", "r", "d"]
```

使用 spread 运算符，我们可以以更干净的方式实现同样的目的。请看下面的例子:

```
const string = 'word';
const arr = [...string]; // The spread operator.console.log(arr); // Prints: ["w", "o", "r", "d"]
```

# 2.获取数组的最大值

下面的 ES5 代码使用`**apply()**`计算数组中的最大值:

```
var arr = [6, 89, 3, 45];
var maximus = **Math.max.apply**(null, arr); // returns 89
```

spread 运算符使该语法更易于阅读和编写。

请看下面的例子:

```
const arr = [6, 89, 3, 45];
const maximus = **Math.max(...arr)**; // returns 89
```

`**...arr**`返回一个解包的数组。换句话说，它*扩展了*数组。

# 3.组合两个对象

我们可以使用 JavaScript 中的 spread 运算符轻松合并两个对象，而不必分配一个完整的对象。

请看下面的例子:

```
let employee = { name:'Jhon',lastname:'Doe'};
const salary = { grade: 'A', basic: '$3000' };
const summary = {**...employee**, **...salary**};console.log(summary);
// Prints: {name: "Jhon", lastname: "Doe", grade: "A", basic: "$3000"}
```

如您所见，这里`**employee**`和`**salary**`对象被合并在一起，这产生了一个具有合并的键值对的新对象。

# 4.组合数组

spread 运算符的另一个优点是能够在任何索引处组合数组，或将一个数组的所有元素插入到另一个数组中。

Spread 语法使下面的操作变得非常简单和清晰:

```
let thisArray = ['sage', 'rosemary', 'parsley', 'thyme'];
let thatArray = ['basil', 'cilantro', **...thisArray**, 'coriander'];// thatArray now equals: ['basil', 'cilantro', 'sage', 'rosemary', 'parsley', 'thyme', 'coriander']
```

通过使用 spread 运算符`**{…}**`，我们以一种简单的方式实现了我们想要的。如果我们使用传统的方法，那将会更加复杂和冗长。

# 5.复制数组

我们也可以使用 spread 运算符将 JavaScript 中数组的内容复制到另一个数组中。

您可以查看以下示例:

```
const arr1 = ['JAN', 'FEB', 'MAR', 'APR', 'MAY'];
let arr2;arr2 = [**...arr1**];console.log(arr2);
// Prints:[ 'JAN', 'FEB', 'MAR', 'APR', 'MAY' ]
```

如您所见，我们已经使用扩散运算符将`**arr1**`的所有内容复制到了另一个数组`**arr2**`中。

# 结论

spread 运算符(…)对于在 Javascript 中处理数组和对象非常有用。在使用 React 等框架以及开发 reducers 时，您会经常看到这种情况。通过使用这个操作符，您将使您的代码更加清晰，易于维护。

感谢您阅读本文，希望您觉得有用。如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**

# 更多阅读

[](https://medium.com/javascript-in-plain-english/understanding-iife-in-javascript-4aa5ebea4c3f) [## 理解 JavaScript 中的生活

### 通过实例了解 JavaScript 中的生活

medium.com](https://medium.com/javascript-in-plain-english/understanding-iife-in-javascript-4aa5ebea4c3f)