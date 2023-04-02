# JavaScript 中的析构赋值及实例

> 原文：<https://javascript.plainenglish.io/better-javascript-the-destructuring-assignment-d128bd2a8d9a?source=collection_archive---------13----------------------->

## 更好的 JavaScript——析构赋值

![](img/07196edacd5df6020e8cef317e17ee56.png)

Photo by [Hitesh Choudhary](https://unsplash.com/@hiteshchoudhary?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 什么是解构作业？

析构赋值是 ES6 中引入的一种特殊语法，用于灵活地赋值直接来自对象的值。它是一个 JavaScript 表达式，可以将数组中的值或对象中的属性解包到不同的变量中。析构赋值使得以一种简单的方式编写更简洁的 JavaScript 代码变得更加容易。

在本文中，我们将探索 JavaScript 中析构赋值的一些用法。

![](img/73a96012886995786cc5d1af1fb0e54f.png)

Image Created with ❤️️ By Mehdi Aoussiad.

# 从对象中提取值

我们可以使用析构赋值以一种更简洁的方式从对象中提取值。看看下面的例子:

考虑下面的 ES5 代码:

```
const user = { name: 'John Doe', age: 34 };

const name = user.name; //Prints: name = 'John Doe'
const age = user.age; //Prints: age = 34
```

下面是一个使用 ES6 析构语法的等效例子:

```
**const { name, age } = user;**
//Prints: name = 'John Doe', age = 34
```

您可以从对象中提取任意多或任意少的值。

# 从对象分配变量

我们也可以使用 ES6 析构语法从对象中分配变量。

下面是一个如何在赋值中给新变量名的例子:

```
const { name: userName, age: userAge } = user;
// userName = 'John Doe', userAge = 34
```

如你所见，你可能会把它理解为“获取`user.name`的值，并把它赋给一个名为`userName`的新变量”等等。

# 从嵌套对象中分配变量

您可以使用前面示例中的相同原则来析构嵌套对象的值。

使用类似于前面示例的对象:

```
const user = {
  johnDoe: { 
    age: 34,
    email: 'johnDoe@gmail.com'
  }
};
```

下面是如何提取对象属性的值并将它们分配给同名变量:

```
const { johnDoe: { age, email }} = user;
```

# 从数组中分配变量

ES6 使得析构数组像析构对象一样容易。看看下面的例子:

析构数组:

```
const [a, b] = [1, 2, 3, 4, 5, 6];
console.log(a, b); // 1, 2
```

另一个例子:

```
const [a, b,,, c] = [1, 2, 3, 4, 5, 6];
console.log(a, b, c); // 1, 2, 5
```

我想你现在明白了，它使我们的代码更干净，更容易维护。

如果您了解 JavaScript 中的 spread 操作符，那么该操作符和数组析构之间的一个关键区别就是 spread 操作符将数组的所有内容解包到一个逗号分隔的列表中。因此，您不能挑选或选择要分配给变量的元素。

# 结论

JavaScript 中的析构赋值是 ES6 中强大的特性之一，它使我们的代码更干净，更容易被其他开发者维护。

感谢您阅读本文，希望您觉得有用。

## 进一步阅读

[](https://medium.com/javascript-in-plain-english/5-useful-things-the-spread-operator-can-do-in-javascript-f0306358bc9c) [## JavaScript 中 Spread 操作符可以做的 5 件有用的事情

### 用 Spread 操作符编写一个更好更干净的 JavaScript 代码

medium.com](https://medium.com/javascript-in-plain-english/5-useful-things-the-spread-operator-can-do-in-javascript-f0306358bc9c)