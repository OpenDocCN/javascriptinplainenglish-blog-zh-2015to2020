# RxJS:过滤掉重复值

> 原文：<https://javascript.plainenglish.io/rxjs-filter-repetitive-values-out-a0afcf42264b?source=collection_archive---------8----------------------->

![](img/b1bd7b895eb02dfb9f9876f013a1061f.png)

RxJS 有一堆很酷的运营商。即使你已经可以用`map`和`filter`做很多事情，了解你工具箱里所有的工具会让你有空闲时间，写出更清晰、更简洁的代码。

如果您正在处理一个发出可能重复值的可观察对象，但只对新的值感兴趣，您可以构建一个带有`temp`变量、`map`和`filter`的东西，以便跳过与您之前收到的值相同的值。您也可以使用`distinctUntilChanged`运算符。

`*distinctUntilChanged*` *仅在当前值不同于上一次*时发出

这意味着可观察到以下情况:

```
from([1, 1, 2, 1, 2, 3, 3]).pipe(distinctUntilChanged());
```

订阅时会发出`1, 2, 1, 2, 3`。

对于对象，类似的例子就没那么好了:

```
from([
  { value: 1 }, 
  { value: 1 },
  { value: 2 },
]).pipe(distinctUntilChanged());
```

即使第一个和第二个对象是相同的，这个对象也会发出所有三个对象。这是因为`distinctUntilChanged`使用了精确的比较运算符`===`，所以只有当对象具有相同的引用时才会匹配它们。在我们的例子中情况并非如此。

这并不意味着在比较简单值时只能使用运算符。对于需要比较对象的情况，有两种选择。

首先你可以先定义自己的`compare`函数。假设原始的可观察对象发出一个用户列表，如果这个值与前一个值具有相同的姓和名，那么您可以决定跳过这个值。为此，您可以将比较函数作为参数传递给`distinctUntilChanged`操作符。

```
const areUsersEqual = (
  user1,
  user2,
): boolean => user1.lastname === user2.lastname &&
             user1.firstname === user2.firstname;from([
  { firstname: 'Tom', lastname: 'Johnson' }, 
  { firstname: 'Tom', lastname: 'Johnson' },
  { firstname: 'Martin', lastname: 'Johnson' },
]).pipe(distinctUntilChanged(areUsersEqual));
```

这将按预期工作，并且仅发射`Tom Johnson`一次。

对于像我们前面的例子这样简单的情况，它可能看起来有很多代码:

```
from([
  { value: 1 }, 
  { value: 1 },
  { value: 2 },
]).pipe(distinctUntilChanged()); 
```

如果您只能从一个属性的值中区分两个元素，有一个更简洁的解决方案:`distinctUntilKeyChanged`。您可以将想要用来比较发出值的属性作为参数传递给运算符。如果两个连续发射的对象具有相同的属性值，则只发射第一个对象。

如果用户有`id`，我们可以与他们一起使用:

```
from([
  { id: '1', firstname: 'Tom', lastname: 'Johnson' }, 
  { id: '1', firstname: 'Tom', lastname: 'Johnson' },
  { id: '2', firstname: 'Martin', lastname: 'Johnson' },
]).pipe(distinctUntilKeyChanged('id'));
```

这里再次说明，`Tom Johnson`只发出一次。

RxJS 操作符通常很容易掌握，可以节省几行代码。但是它们太多了，有时候很难找到你要找的那一个。听说过`distinctUntilChanged`和`distinctUntilKeyChanged`并且知道无论你比较的值是什么形式，你都可以使用它们，下次你遇到类似的问题时，你会想到它。