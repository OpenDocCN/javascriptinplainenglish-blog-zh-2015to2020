# JavaScript 的优点——流量控制

> 原文：<https://javascript.plainenglish.io/good-parts-of-javascript-flow-control-7ef351299226?source=collection_archive---------6----------------------->

![](img/95d5955f5c20a0935e81d1cd2a5742f2.png)

Photo by [Leila Bandringa](https://unsplash.com/@leila_bandringa?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是世界上最流行的编程语言之一。它可以做很多事情，并且有一些领先于许多其他语言的功能。

在本文中，我们将看看 JavaScript 的一些优秀部分，包括流控制。

# 虚伪的价值观

JavaScript 中的 Falsy 值将阻止`if`或`else if`块运行。

Falsy 值如下:

*   `false`
*   `null`
*   `undefined`
*   空字符串`‘’`
*   0
*   `NaN`

其他一切都是真实的。

# Switch 语句

`switch`语句执行多路分支。

它将表达式与指定的大小写进行比较。

案例可以是一条语句或一个块。一个`case`语句或块可以包含一个或多个表达式。

case 表达式不一定是常量。

# While 语句

一个`while`语句执行一个简单的循环。如果`while`之后的条件为假，循环将停止运行。

否则，循环体将运行。

# For 语句

一个`for`语句比一个`while`循环更复杂。

它可能包含 3 个可选子句，即初始化代码、循环运行时的条件和 increment 语句。

首先，运行初始化代码。

然后在每次迭代中评估该条件。如果条件为真，则循环将运行。

否则，循环将会中断。

然后运行 increment 语句。该语句更新索引变量。

`for`循环看起来像:

```
for (let i = 0; i < MAX; i++) {
  //...
}
```

从`i`开始的循环运行为 0，从`i`开始的循环运行比`MAX`少 1。

# For-In 循环

`for-in`循环遍历对象的键，包括任何继承的属性。

按键循环的顺序取决于浏览器如何实现`for-in`循环。

这意味着我们不能依赖按键循环的顺序。

为了检查属性是否被继承，我们可以使用对象自带的`hasOwnProperty`方法。

我们可以通过编写以下代码来实现这一点:

```
for (const key in obj) {
  if (obj.hasOwnProperty(key)) {
    //..
  }
}
```

在上面的代码中，我们有在`obj`中循环的`key`，包括继承的属性。

因此，如果我们只想处理`obj`本身的键，我们必须调用`obj.hasOwnProperty(key)`。

# For-Of 循环

`for-of`循环遍历任何可迭代对象中的项目。

这包括数组、节点列表、集合、映射、生成器和我们创建的任何其他东西。

例如，我们可以写:

```
for (const a in arr) {
  //..
}
```

这意味着循环遍历条目比使用`while`或`for`循环更容易，因为我们不需要指定任何条件。

它只是遍历 iterable 对象中的所有条目。

# Try 语句

`try`语句运行一个代码块，并捕获该代码块中抛出的异常。

`catch`子句位于`try`子句之后，接收异常对象。

看起来像是:

```
try {
  //...
} catch (ex) {
  //...
}
```

# Throw 语句

`throw`语句用于抛出异常，可以用`try...catch`块捕获异常。

例如，我们可以写:

```
throw new Error('error');
```

我们可以用`throw`语句抛出任何东西，但是 error 对象拥有诸如堆栈跟踪和错误发生的行号之类的信息，而这些信息在其他地方是不存在的。

# 返回语句

`return`语句让我们可以在函数块中的任何地方结束函数。

JavaScript 不允许在`return`和它返回的表达式之间有一条线。

# break 语句

`break`语句用于结束循环或终止`switch`语句。

我们还可以添加一个`label`来结束一个带标签的语句。

`break`和标签必须在同一行。

![](img/017491ce0f5767d23957137e654c5894.png)

Photo by [Martin Jernberg](https://unsplash.com/@martinjernberg?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 表达式语句

表达式语句可以分配变量，也可以分配一个或多个变量或成员。

它还可以运行方法或删除对象的属性。

`=`操作符用于赋值，不要和`===`操作符混淆。

`+=`运算符可以相加或连接。

# 结论

`break`语句用于结束循环和`switch`案例。

`return`用于结束代码中任何地方的函数。

`throw`语句用于将任何对象作为错误抛出，然后被`catch`块捕获。

## **说白了就是**

通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达爱意吧！**