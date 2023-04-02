# 如何在 JavaScript 中使用 forEach

> 原文：<https://javascript.plainenglish.io/how-to-use-foreach-in-javascript-ac26b0c533b0?source=collection_archive---------8----------------------->

## 了解如何使用 forEach，它与 for 循环和 map 有何不同，以及何时应该/不应该使用它

![](img/bea7b7543c1c9aa939a02dae246f6f3c.png)

如果你花了一点时间在任何编程语言上，你可能会遇到“ ***代表循环*** ”。循环非常有用，但是有时候写起来有点复杂。我记得这是我遇到的第一个编程概念，我花了很长时间才记住。我花了更长的时间才意识到用 Javascript 写这些有更简单的方法——更简单的方法是 **forEach** 。

## 好吧，但有什么区别呢？

**for** 循环和 **forEach** 循环的区别在于，在 **for** 循环中，您决定您希望函数运行多少次，而 f **orEach** 循环将运行它所使用的数组内的项目数。

例如，如果数组中有三个项目，那么 **forEach** 将运行三次——对数组中的每个项目运行一次。而对于回路**、**的**，你需要告诉它运行三次。这听起来有点复杂，所以让我们看看如何使用 **for** 循环和 **forEach** 编写相同的函数:**

**为:**

```
const animals = [‘cat’, ‘dog’, ‘sunny’];
const copy = [];for (let i=0; i<animals.length; i++) {
    copy.push(animals[I])
}
```

**ForEach:**

```
const animals = [‘cat’, ‘dog’, ‘sunny’];
const copy = [];animals.forEach(function(animal){
    copy.push(animal)
});
```

所以在这里，`*animals.forEach(function(animal)*`和`f*or(let i=0; i<animals.length; i++)*`是一样的。

您甚至可以选择使用 ES2015 语法使 forEach 更加简洁，如下所示:

```
const animals = [‘cat’, ‘dog’, ‘sunny’];
const copy = [];animals.forEach(animal => { copy.push(animal) });
```

这里要记住的关键是，当你计划对数组中的每一项运行相同的函数时，forEach 是有用的。但是如果你需要写一些更加定制化的东西，一个循环的**可能是一个更好的解决方案。**

希望你觉得这个超级短的教程有用。如果是这样，一定要留下充足的掌声！