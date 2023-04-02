# rjs 运算符—计算和聚合

> 原文：<https://javascript.plainenglish.io/rxjs-operators-calculation-and-aggregation-fe01e139d714?source=collection_archive---------4----------------------->

![](img/636298e4ed0de8c48c36760cdbbc406e.png)

Photo by [Fas Khan](https://unsplash.com/@fasbytes?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

rjs 是一个进行反应式编程的库。创建运算符对于从各种数据源生成要由观察者订阅的数据非常有用。

在本文中，我们将了解一些计算和聚合运算符，包括`count`、`max`、`min`和`reduce`。

# 数数

`count`运算符返回一个可观测值，该值计算源上的发射数量，并在源完成时发射该数量。

它采用一个可选的`predicate`函数，该函数返回一个带有计数条件的布尔值。

`predicate`有 3 个参数，即源可观测值发出的`value`、`index`是源可观测值从零开始的“索引”,以及`source`是源可观测值本身。

例如，我们可以如下使用它来计算发射值的总数:

```
import { of } from "rxjs";
import { count } from "rxjs/operators";const of$ = of(1, 2, 3, 4, 5, 6);
const result = of$.pipe(count());
result.subscribe(x => console.log(x));
```

没有传递`predicate`功能的`count`操作员将计算可观察源的所有排放。

这应该得到我们 6，因为我们在`of$`可观察到 6 个值。

我们也可以传入一个`predicate`函数，如下所示:

```
import { of } from "rxjs";
import { count } from "rxjs/operators";const of$ = of(1, 2, 3, 4, 5, 6);
const result = of$.pipe(count(val => val % 2 === 0));
result.subscribe(x => console.log(x));
```

那么我们应该得到 3，因为我们只是从`of$`发出的偶数。

# 最大

`max`运算符从发出可与`comparer`函数比较的数字或项目的可观察源获取值，从中获取最大的一个，并与返回的可观察值一起发出。

它带有一个可选的`comparer`功能，我们可以用它来比较两个项目的值。

例如，我们可以用它来获得可观测值发出的最高数值，如下所示:

```
import { of } from "rxjs";
import { max } from "rxjs/operators";of(2, 3, 4, 100)
  .pipe(max())
  .subscribe(x => console.log(x));
```

我们应该得到 100，因为 100 是`of(2, 3, 4, 100)`可观察到的最大数字。

此外，我们还可以将一个函数传递给`max`运算符，以便从对象列表中获得最大值，如下所示:

```
import { of } from "rxjs";
import { max } from "rxjs/operators";const people = [
  { age: 17, name: "Joe" },
  { age: 25, name: "Jane" },
  { age: 19, name: "Mary" }
];of(...people)
  .pipe(max((a, b) => a.age - b.age))
  .subscribe(x => console.log(x));
```

那么我们应该得到:

```
{age: 25, name: "Jane"}
```

来自`console.log`。

`comparer`函数的工作方式类似于数组的`sort`方法的回调。如果`comparer`返回负数，我们保留订单。如果`comparer`返回一个正数，我们就改变顺序。否则，他们是一样的。然后从末端挑出最大的一个。

![](img/c26f2a298e2a8945b3ed97b394f39206.png)

Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 部

`min`运算符与`max`运算符相反。它从源可观测值中获得最小值。

参数与`max`操作符相同。

我们可以像使用`max`操作符一样使用它，如下所示:

```
import { of } from "rxjs";
import { min } from "rxjs/operators";of(2, 3, 4, 100)
  .pipe(min())
  .subscribe(x => console.log(x));
```

那么我们得到 2。

像`max`操作符一样，我们可以使用带有`comparer`功能的`min`操作符，如下所示:

```
import { of } from "rxjs";
import { min } from "rxjs/operators";
const people = [
  { age: 17, name: "Joe" },
  { age: 25, name: "Jane" },
  { age: 19, name: "Mary" }
];
of(...people)
  .pipe(min((a, b) => a.age - b.age))
  .subscribe(x => console.log(x));
```

然后我们得到:

```
{age: 17, name: "Joe"}
```

`comparer`函数的工作方式与我们传递给`max`操作符的方式相同，除了选择第一个而不是最后一个。

# 减少

`reduce`操作符对源可观察对象发出的所有值应用一个累加器函数，将它们组合成一个值，由返回的可观察对象发出。

它最多需要两个参数。第一个是`accumulator`函数，这是我们用来组合值的函数。

第二个参数是可选的`seed`值，它是累加的初始值。

例如，我们可以如下使用它:

```
import { of } from "rxjs";
import { reduce } from "rxjs/operators";
of(2, 3, 4, 100)
  .pipe(reduce((a, b) => a + b, 0))
  .subscribe(x => console.log(x));
```

上面的代码将对来自`of(2, 3, 4, 100)`的可观察值求和，并发出 109，这是 2、3、4 和 100 的和。

`a`和`b`是从可观测源发出的值。

`count`操作符返回一个可观察值，根据我们指定的条件计算源可观察值发出或源可观察值发出某种值的次数。

`min`和`max`用于根据一个排序函数或从分别发出的一组数中得到最小值和最大值。

`reduce`操作符返回一个可观察对象，它从源可观察对象中获取值，并按照我们在传递给它的函数中指定的方式将它们组合成一个。