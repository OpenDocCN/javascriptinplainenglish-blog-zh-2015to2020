# 顶级 JavaScript 代码调优技术

> 原文：<https://javascript.plainenglish.io/top-javascript-code-tuning-techniques-806f8d780a3b?source=collection_archive---------7----------------------->

![](img/a377e1ca7ff8af16302e7725ad063f4a.png)

Photo by [Adrian Dascal](https://unsplash.com/@dascal?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。编写可以运行但有错误的代码很容易。

在本文中，我们将研究 JavaScript 代码的一些常见代码调优技术。

# 当我们知道答案时停止测试

JavaScript 做短路评估。因此，我们应该利用这一点，让它只测试需要的条件。

例如，以下内容是多余的:

```
if ((5 < x) && (x < 10)) {
  //..
}
```

我们检查`x`是否大于 5，以及`x`是否小于 10。

如果我们知道`x`不大于 5，那么我们就不需要检查`x`是否小于 10。

因此，我们可以将其改写为:

```
if (5 < x) {
  //..
  else if (x < 10) {
    //..
  }
}
```

然后我们检查只有当`x`大于 5 时`x`才小于 10

# 按频率排列测试

我们应该将更频繁遇到的案例放在第一位，这样就不必经常检查早期的案例。

例如，如果我们有:

```
switch (val) {
  case '+': {
    //...
    break;
  }
  case '-': {
    //..
    break;
  }
  //...
}
```

假设更经常遇到加法，那么我们把它放在第一位，这样其他情况就不必频繁检查，减少了必须运行的代码量。

# 用表格查找代替复杂的表达式

如果我们有很多案例要检查，而我们只检查一些常量，那么我们可以用查找表代替`if`或`switch`语句。

例如，我们可以写:

```
const table = {
  0: 'Sunday',
  1: 'Monday',
  2: 'Tuesday',
  3: 'Wednesday',
  4: 'Thursday',
  5: 'Friday',
  6: 'Saturday',
}
```

而不是:

```
const dayOfWeek = (day) => {
  if (day === 0) {
    return 'Sunday'
  } else if (day === 1) {
    return 'Monday'
  }
  //...
}
```

# 不切换

如果我们只是在循环的每次迭代中运行相同的`if`或`else`，那么我们不应该将`if`和`else`块放入循环中。

例如，如果我们有:

```
for (i = 0; i < count; i++) {
  if (type === SUM) {
    //...
  } else {
    //...
  }
}
```

并且每次迭代的`type`等于`SUM`或者相反，那么我们应该将`if`和`else`移动到循环体之外，如下所示:

```
if (type == SUM) {
  for (i = 0; i < count; i++) {
    //...
  }
} else {
  for (i = 0; i < count; i++) {
    //...
  }
}
```

然后，在每个循环中没有布尔检查，使代码运行得更快。

# 干扰

干扰是将两个运行在相同元件上的环路组合在一起。

由于它们对相同的元素起作用，我们可以很容易地将它们结合起来。

例如，如果我们有:

```
for (let i = 0; i < employees.length; i++) {
  employeeName[i] = "";
}for (let i = 0; i < employees.length; i++) {
  employeesalary[i] = 0;
}
```

然后我们可以将它们合并为一个，如下所示:

```
for (let i = 0; i < employees.length; i++) {
  employeeName[i] = "";
  employeesalary[i] = 0;
}
```

![](img/4845b23ebc72304e0156333d1c8d4fcf.png)

Photo by [Tim Doerfler](https://unsplash.com/@tadoerfler?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 铺开

展开是将循环的情况分离到它们自己的代码中。

例如，我们可以这样写:

```
let i = 0;
while (i < count - 1) {
  foo[i] = i;
  foo[i + 1] = i + 1;
  i += 2;
}
if (i === count - 1) {
  foo[count - 1] = count - 1;
}
```

在上面的代码中，我们有一个`while`循环，一直运行到`i`变为`count - 1`。

然后在它下面，我们有一个`if`语句，它只在`i`为`count — 1`时运行，这可能并不总是这样，因为`i`增加了 2。

减少迭代次数使我们的代码更快。

# 最小化循环内部的工作

如果循环体中的代码更少，那么它的执行速度会更快。

因此，我们应该尽量简化我们的循环，使它们执行得更快。更简单的循环也更容易阅读。

# 结论

我们应该致力于简化我们的循环，使它们运行得更快。

如果有不总是在循环中运行的情况，那么它应该被分离出循环。

我们还应该利用短路求值来减少必须检查条件语句的情况。

## **简明英语笔记**

你知道我们推出了一个 YouTube 频道吗？我们制作的每个视频都旨在教给你一些新的东西。点击此处 查看我们，并确保订阅该频道😎