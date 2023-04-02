# 在 JavaScript 中搜索数组的不同方法

> 原文：<https://javascript.plainenglish.io/different-methods-to-search-an-array-in-javascript-3376ee7cc636?source=collection_archive---------12----------------------->

## 了解如何使用 JavaScript 在数组中搜索项目

![](img/1be18be80dea2f7c36c0653dbb5efb97.png)

Photo by Oskar Yildiz on Unsplash

大家好！

在这个故事中，我们将看到 JavaScript 中不同的方法来搜索数组中的项目。

在 JavaScript 中有四种不同的方法来搜索数组。它们如下:

*   过滤器
*   发现
*   包含
*   索引 Of

从以上不同的方法中，我们要根据我们的用例来选择方法。

现在让我们深入正题。

# Array.filter()

**Array.filter()** 方法用于查找数组中特定条件下的元素。让我们看一个例子，它打印一个大于 10 的数组中的所有元素。

```
const array = [10, 11, 3, 20, 5];

const greaterThanTen = array.filter(element => element > 10);

console.log(greaterThanTen) //[11, 20]
```

语法:

```
let newArray = array.filter(callback);
```

在哪里

*   **new array**——是函数返回的数组。
*   **数组**——这是一个调用过滤方法的数组。
*   这是一个回调函数，应用于数组中的所有元素。

# Array.find()

**Array.find()** 方法用于在特定条件下查找数组中的第一个元素。类似于 **filter()** 方法，不同的是以 callback 为函数，返回特定条件下的第一个元素。

让我们看一个在数组中查找方法的例子。

```
const array = [10, 11, 3, 20, 5];

const greaterThanTen = array.find(element => element > 10);

console.log(greaterThanTen)//11
```

语法:

```
let element = array.find(callback);
```

在哪里

*   **元素** -它是当前迭代的元素。
*   **索引** -元素的位置。
*   **数组**——用于查找被调用的。

注意，如果数组中没有元素满足这个条件，那么它返回 **undefined** 。

# Array.includes()

这个 **includes()** 方法用于确定一个数组是否包含某个值，并在适当的条件下返回 true 或 false。

让我们看一个例子，它检查数字 20，它是否是数组中的一个元素？

```
const array = [10, 11, 3, 20, 5];

const includesTwenty = array.includes(20);

console.log(includesTwenty)//true
```

语法:

```
const includesValue = array.includes(valueToFind, fromIndex)
```

在哪里

*   **valueToFind** -用于检查的数值。
*   **fromIndex** -它决定了数组中你想要开始的起始位置。

# Array.indexOf()

indexOf()方法用于返回在数组中找到的第一个元素。如果没有找到元素，它返回-1。

让我们看一个例子

```
const array = [10, 11, 3, 20, 5];

const includesTenTwice = array.includes(10, 1);

console.log(includesTenTwice)//false
```

语法:

```
const indexOfElement = array.indexOf(element, fromIndex)
```

这个语法也类似于 **induces()** 方法。

在哪里

*   **元素** -它是当前迭代的元素。
*   **from index**——它决定了一个数组中你想要开始的起始位置。

# 结论

我希望您会喜欢，并发现使用 JavaScript 在数组中搜索条目的不同方法的新内容。根据您的用例，您将选择最佳方法。

感谢阅读！