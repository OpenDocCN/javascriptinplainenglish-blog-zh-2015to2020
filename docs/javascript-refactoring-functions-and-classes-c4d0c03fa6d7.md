# JavaScript 重构—函数和类

> 原文：<https://javascript.plainenglish.io/javascript-refactoring-functions-and-classes-c4d0c03fa6d7?source=collection_archive---------7----------------------->

![](img/ac23f4c33c007cfcb5c1307cf24e6b5c.png)

Photo by [Syed Hussaini](https://unsplash.com/@syhussaini?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们可以清理我们的 JavaScript 代码，这样我们可以更容易地使用它们。

在本文中，我们将研究一些与清理 JavaScript 条件相关的重构思想。

# 引入参数对象

如果我们的函数有多个参数，我们可以将它们组合成一个对象参数，然后使用它。

例如，如果我们有以下代码:

```
const getAmountFromRange = (start, end) => {
  //...
}
```

我们可以改为编写以下代码:

```
const getAmountFromRange = ({
  start,
  end
}) => {
  //...
}
```

我们需要的参数越少，我们的功能就越好。

# 移除设置方法

如果我们有额外的 setters 从来没有使用过，那么我们可以把它们从我们的类中删除。

例如，代替编写下面的类:

```
class Employee {
  setUselessValue() {
    //,..
  }
}
```

我们可以删除无用的 setter，如下所示:

```
class Employee {
  //...
}
```

# 用工厂方法替换构造函数

如果我们想做比一个简单的构造函数更多的事情，那么我们可以用一个工厂方法来代替它。

例如，不是只有下面的类:

```
class Employee {
  constructor(type) {
    this.type = type;
  }
  //...
}
```

我们可以有一个工厂函数，它根据传入的类型实例化该类:

```
class Employee {
  constructor(type) {
    this.type = type;
  }
  //...
}const createEmployee = type => new Employee(type);
```

这让我们可以用一个功能创建所有类型的员工。

# 用异常替换错误代码

我们可以用异常替换错误代码，这样当错误发生时，我们就更容易发现它。

例如，如果我们有以下函数:

```
const deposit = (amount) => {
  if (amount < 0) {
    return -1;
  }
  //...
}
```

我们可以抛出一个异常，如下所示:

```
const deposit = (amount) => {
  if (amount < 0) {
    throw new Error('amount must be bigger than or equal to 0');
  }
  //...
}
```

异常比返回代码更明显，所以我们应该使用抛出异常。

像-1 这样的错误代码如果不解释或者不赋给常量是没有意义的。

# 用测试替换异常

我们也可以用条件来代替异常来检查值，这样我们就不必到处使用`try...catch`块。

例如，不写:

```
const getItemByIndex = (arr, index) => {
  try {
    return arr[index].foo;
  } catch (ex) {
    console.error(ex);
  }
}
```

我们可以这样写:

```
const getItemByIndex = (arr, index) => {
  if (typeof arr[index] === 'undefined') {
    return;
  }
  return arr[index].foo;
}
```

这样，我们就不必通过引入`try...catch`来使我们的代码变得更复杂，这样会使工作流不那么线性。

![](img/75be87cf2bb07d47dd6243b39d4485b3.png)

Photo by [Marvin Meyer](https://unsplash.com/@marvelous?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 拉起字段

如果两个或更多的子类有相同的字段，那么我们可以把它们提升到超类。

例如，不用编写下面的代码:

```
class Employee {}class Cook extends Employee {
  constructor(name) {
    this.name = name;
  }
}class Manager extends Employee {
  constructor(name) {
    this.name = name;
  }
}
```

我们写道:

```
class Employee {
  constructor(name) {
    this.name = name;
  }
}class Cook extends Employee {}class Manager extends Employee {}
```

这更好，因为`name`在`Employee`类中，而不是在多个子类中重复。

# 拉高方法

同样，如果我们有两个或两个方法在多个子类中重复，我们可以将它们提升到超类。

例如，不用编写下面的代码:

```
class Employee {}class Cook {
  speak() {
    //...
  }
}class Manager {
  speak() {
    //...
  }
}
```

我们可以改为编写以下内容:

```
class Employee {
  speak() {
    //...
  }
}class Cook extends Employee {
  //...
}class Manager extends Employee {
  //...
}
```

同样，我们消除了重复，这很好。

# 结论

我们可以通过将重复的字段和方法移动到超类来消除重复。

此外，我们可以将多个参数分组到对象中，以便减少函数中的参数数量。

我们也不必总是抛出异常，我们可以在继续执行代码之前检查我们在寻找什么。

然而，当我们返回错误代码时，有时我们可能想抛出异常。

# **用简单英语写的便条**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页 [**plainenglish.io**](https://plainenglish.io/) 找到所有这些内容——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**