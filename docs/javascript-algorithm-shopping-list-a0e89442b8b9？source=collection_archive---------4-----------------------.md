# JavaScript 算法:购物清单

> 原文：<https://javascript.plainenglish.io/javascript-algorithm-shopping-list-a0e89442b8b9?source=collection_archive---------4----------------------->

![](img/edf3ed953688af80d4bea3e7ee24980f.png)

对于今天的算法，我们不打算写一个函数，而是学习基本的 JavaScript 基础知识，主要是数学运算符的使用。

对于这个算法，你负责点餐，你计划点很多食物。你要买的食物有:

```
4 sandwiches
6 salads
5 wraps
10 French fries
```

每个类别的每个项目的成本为:

```
sandwich - $8.00
salad - $7.00
wrap - $6.50
french fries - $1.20
```

该函数的目标是输出订单的总成本。

我们首先为每种食物类别创建四个变量。每个食品项目的成本乘以数量被分配给变量。

```
const sandwiches = 4 * 8.00;
const salads = 6 * 7.00;
const wraps = 5 * 6.50;
const frenchFries = 10 * 1.20;
```

接下来，我们将所有的食物加起来，并将其分配给一个名为`totalPrice`的变量。

```
let totalPrice = sandwiches + salads + wraps + frenchFries;
```

该函数将输出总数`118.5`。就是这样。希望你明白发生了什么，以及我们是如何得出这个总数的。

如果你觉得这个算法有帮助，可以看看我最近的其他 JavaScript 算法解决方案:

[](https://levelup.gitconnected.com/javascript-algorithm-marcs-cakewalk-98ad0c699348) [## JavaScript 算法:Marc 的 Cakewalk

### 对于今天的算法，我们将编写一个名为 marcsCakewalk 的函数，它将接受一个数组，卡路里，作为…

levelup.gitconnected.com](https://levelup.gitconnected.com/javascript-algorithm-marcs-cakewalk-98ad0c699348) [](https://codeburst.io/javascript-algorithm-camelcase-4df119b6216e) [## JavaScript 算法:CamelCase

### 对于今天的简短算法，我们将创建一个名为 camelcase 的函数，它将接受一个字符串输入 s。

codeburst.io](https://codeburst.io/javascript-algorithm-camelcase-4df119b6216e) [](https://medium.com/javascript-in-plain-english/javascript-algorithm-cats-and-a-mouse-fd60fb1811ba) [## JavaScript 算法:猫和老鼠

### 对于今天的算法，我们将创建一个名为 catAndMouse 的函数，它将接受三个整数:x、y 和…

medium.com](https://medium.com/javascript-in-plain-english/javascript-algorithm-cats-and-a-mouse-fd60fb1811ba)