# 更多 Rxjs 变换运算符-分组和地图

> 原文：<https://javascript.plainenglish.io/more-rxjs-transformation-operators-group-and-map-7a2a8b5fb08?source=collection_archive---------2----------------------->

![](img/150efd175f2b7b93cb2e30f8f06fb7ac.png)

Photo by [Ray Hennessy](https://unsplash.com/@rayhennessy?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Rxjs 是一个用于进行反应式编程的库。创建操作符对于从各种数据源生成供观察者订阅的数据非常有用。

在本文中，我们将看看一些转换操作符，如`groupBy`、`map`、`mapTo`和`mergeMap`。

# 分组依据

`groupBy`操作符获取源观测值发出的值，然后根据我们为它们设置的标准对它们进行分组。

它需要 4 个参数。第一个是`keySelector`，这是一个为每个项目提取密钥的函数。

第二个是`elementSelector`的可选参数，这是一个为每个项目提取返回元素的函数。

第三个参数是`durationSelector`，这是一个可选函数，返回一个可观察值来确定每个组应该存在多长时间。

最后，最后一个参数是`subjectSelector`，它是一个可选函数，返回一个主题。

例如，我们可以如下使用它:

```
import { of } from "rxjs";
import { groupBy, reduce, mergeMap } from "rxjs/operators";const observable = of(
  { id: 1, name: "John" },
  { id: 2, name: "Jane" },
  { id: 2, name: "Mary" },
  { id: 1, name: "Joe" },
  { id: 3, name: "Don" }
).pipe(
  groupBy(p => p.id),
  mergeMap(group$ => group$.pipe(reduce((acc, cur) => [...acc, cur], [])))
);observable.subscribe(val => console.log(val));
```

在上面的代码中，我们调用了 `groupBy(p => p.id)`来对从`of`发出的可被`id`观察到的项目进行分组。

然后我们有:

```
mergeMap(group$ => group$.pipe(reduce((acc, cur) => [...acc, cur], [])))
```

将分组的项目放在一起，由一个可观测的物体发射。

# 地图

`map`操作符让我们将源可观察对象发出的值映射到其他值，并将结果值作为可观察对象发出。

它最多需要两个参数。第一个参数是`project`函数，这是必需的。该函数获取源可观测值的发射值及其索引，然后通过操作这些返回我们想要的结果。

第二个参数是可选的。它是`thisArg`，用于为第一个参数中的`project`函数定义`this`值。

例如，我们可以如下使用它:

```
import { of } from "rxjs";
import { map } from "rxjs/operators";const observable = of(1, 2, 3);
const newObservable = observable.pipe(map(val => val ** 2));
newObservable.subscribe(x => console.log(x));
```

上面的代码从`observable`获取发出的值，然后通过`pipe`操作符将其传递给`map`操作符。在回调中，我们传入了`map`操作符，我们对最初发出的值取 2 的幂。

然后我们可以订阅一个发出新值的可观察对象，我们得到:

```
1
4
9
```

记录在案。

# 地图

`mapTo`算子为任何源可观测的发射值发射给定的常数值。

它需要一个参数，即要发出的值。

例如，我们可以如下使用它:

```
import { of } from "rxjs";
import { mapTo } from "rxjs/operators";const observable = of(1, 2, 3);
const newObservable = observable.pipe(mapTo("foo"));
newObservable.subscribe(x => console.log(x));
```

上面的代码将所有值从`observable`映射到值`'foo'`，所以我们得到了`'foo'` 3 次，而不是 1、2 和 3 次。

![](img/3ff0898beb0be64a943bc8646c2e1fe9.png)

Photo by [Josh Howard](https://unsplash.com/@thejoshhoward?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 合并地图

`mergeMap`操作符从一个源可观察对象获取值，然后让我们将它与另一个可观察对象的值结合起来。

最多需要 3 个参数。第一个是一个`project`函数，用于投射值并返回一个新的可观察值。

第二个参数是一个可选参数，它带有一个`resultSelector`。我们可以传入一个函数来选择从结果中发出的值。

第三个参数是`concurrency`，这是一个可选参数，指定并发订阅的输入可观察值的最大数量。默认是`Number.POSITIVE_INFINITY`。

例如，我们可以如下使用它:

```
import { of } from "rxjs";
import { mergeMap, map } from "rxjs/operators";const nums = of(1, 2, 3);
const result = nums.pipe(mergeMap(x => of(4, 5, 6).pipe(map(i => x + i))));
result.subscribe(x => console.log(x));
```

上面的代码将从从`nums`可观察对象发出的 3 个值中获取值，然后通过`pipe`操作符将这些值传递给`mergeMap`的回调函数。`x`将具有来自`nums`的值。

然后在回调中，我们有了`of(4, 5, 6)`可观察值，这些值来自`nums`可观察值的组合。`i`具有来自`of(4, 5, 6)`的可观测值，因此我们将来自两个可观测值相加。我们得到 1 + 4，1 + 5，1 + 6，2 + 4，2 + 5，2 + 6 等等。

最终，我们应该得到以下输出:

```
5
6
7
6
7
8
7
8
9
```

`groupBy`操作符获取由源可观测值发出的值，然后根据我们为它们设置的标准对它们进行分组。我们可以将它与`mergeMap`结合使用，将由`groupBy`操作符分组的结果组合成一个可观察的结果。

`map`操作符让我们将源可观测值发出的值映射到其他值，并将结果值作为可观测值发出。

`mapTo`算子为任何源可观测的发射值发射给定的常数值。

最后,`mergeMap`运算符从一个源可观测值中提取值，然后让我们将其与另一个可观测值相结合。然后我们得到一个可观测值，两个可观测值结合在一起。