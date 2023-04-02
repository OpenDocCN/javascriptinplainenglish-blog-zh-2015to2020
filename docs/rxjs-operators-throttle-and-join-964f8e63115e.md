# Rxjs 运算符—节流和连接

> 原文：<https://javascript.plainenglish.io/rxjs-operators-throttle-and-join-964f8e63115e?source=collection_archive---------5----------------------->

![](img/669bb05a86f07e060605a7e413ae45f2.png)

Photo by [Nathan Anderson](https://unsplash.com/@nathananderson?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Rxjs 是一个用于进行反应式编程的库。创建操作符对于从各种数据源生成供观察者订阅的数据非常有用。

在本文中，我们将看看 throttle 和 join 操作符，包括`throttle`、`throttleTime`、`combineLatest`和`concatAll`。

# 过滤运算符

## 喉咙

`throttle`运算符从源可观测值中发出值，然后在由另一个可观测值决定的时间内忽略随后从源发出的值，然后重复这个过程。

它最多需要两个参数。第一个是`durationSelector`，它是一个函数，从源可观察值中获取一个值，然后返回一个可观察值或承诺，计算每个源值的调节持续时间。

第二个参数是一个可选的`config`对象，用于定义`leading`和`trailing`行为。默认值为`defaultThrottleConfig`。

它从源返回一个执行节流操作的可观察对象。

例如，我们可以如下使用它:

```
import { interval } from "rxjs";
import { throttle } from "rxjs/operators";const interval$ = interval(1000);
const result = interval$.pipe(throttle(ev => interval(5000)));
result.subscribe(x => console.log(x));
```

上面的代码有一个每秒发出一个数字的`interval$`可观察对象。来自它的值被`pipe` d 到`throttle`操作符中，该操作符接受一个返回`interval(5000)`可观察值的函数，该函数每 5 秒发出一个数字。

因此，自从我们的`throttle`回调函数返回`interval(5000)`以来，`throttle`操作符将每 5 秒从`interval$`发出一次值。

那么我们应该在`console.log`中记录每 5 个数字。

# 节流时间

`throttleTime`从源可观测值发出一个值，然后在`duration`毫秒内忽略随后发出的值，然后重复该过程。

最多需要 3 个参数。第一个是`duration`，是在发出上一个值之后，发出另一个值之前等待的时间。它以毫秒或可选的`scheduler`的时间单位计量。

第二个参数是可选的`scheduler`，默认为`async`。它用于设定发射的时间。

最后一个参数是`config`，它是可选的。默认为`defaultThrottleConfig`。我们可以传入一个对象来定义`leading`和`trailing`的行为。默认值是`{ leading: true, trailing: false }`。

例如，我们可以如下使用它:

```
import { interval } from "rxjs";
import { throttleTime } from "rxjs/operators";const interval$ = interval(1000);
const result = interval$.pipe(throttleTime(5000));
result.subscribe(x => console.log(x));
```

上面的工作类似于我们之前的`throttle`示例，除了我们将`throttle(ev => interval(5000))`改为`throttleTime(5000)`，它们做同样的事情。

我们每 5 秒钟从`$interval`发出一次数字。

然后我们得到与上面例子中相同的数字。

![](img/047a674bbd7998eaf03bce1fcb56ff7c.png)

Photo by [Marius Masalar](https://unsplash.com/@marius?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 连接运算符

## 组合所有

`combineAll`运算符通过在可观察值完成时应用`combineLatest`来展平可观察值。

它带有一个可选参数，这是一个`project`函数，用于将发出的每个值映射到我们想要的东西。

一旦外部可观察对象完成，它就订阅所有收集的可观察对象，并按如下方式组合它们的值:

*   每次内部可观察对象发出时，输出可观察对象也会发出
*   当返回的可观察对象发出并且指定了一个`project`函数时，当值到达时`project`被调用，它将使用`project` 函数处理每个值
*   如果没有`project`函数，那么最近的值由返回的可观察值发出

例如，我们可以如下使用它:

```
import { of } from "rxjs";
import { map, combineAll } from "rxjs/operators";const of$ = of(1, 2, 3);
const higherOrderObservable = of$.pipe(map(val => of("a", "b", "c")));
const result = higherOrderObservable.pipe(combineAll());
result.subscribe(x => console.log(x));
```

上面的代码将`of$`可观察值映射到子可观察值。它为每个由`of$`发出的值返回`of(“a”, “b”, “c”)`。

然后我们使用`combineAll`将`higherOrderObservable`中每个子观测值的最新值组合成一个数组。然后这些阵列中的每一个都由可观测的`result`发出。

然后我们得到:

```
["c", "c", "a"]
["c", "c", "b"]
["c", "c", "c"]
```

作为`console.log`输出。前两个可观测量在最后一个之前完成，所以我们从它们那里得到`'c'`。然后随着第三个发出的，加上数组中的最后一个值，所以我们从它们分别得到`'a'`、`'b'`和`'c'`。

## concatAll

`concatAll`运算符通过按顺序连接内部可观测量来转换高阶可观测量，

它不接受任何参数，并返回一个可观察对象，该对象发出所有内部可观察对象的值。

例如，我们可以如下使用它:

```
import { of } from "rxjs";
import { map, concatAll } from "rxjs/operators";const of$ = of(1, 2, 3);
const higherOrderObservable = of$.pipe(map(val => of("a", "b", "c")));
const result = higherOrderObservable.pipe(concatAll());
result.subscribe(x => console.log(x));
```

`of$`可观测值的发射值是`pipe` d，来自`of$`的每个值被映射到`of(“a”, “b”, “c”)`可观测值。

然后我们`pipe`来自`higherOrderObservable`中的内部可观测量的结果是从`of$`映射的 3 个`of(“a”, “b”, “c”)`可观测量，它们的发射值由`concatAll`组合。

最后，我们得到:

```
a
b
c
a
b
c
a
b
c
```

`throttle`运算符从源可观测值发出值，然后在由另一个可观测值确定的持续时间内忽略随后从源发出的值，并重复。

`throttleTime`从源可观测值发出一个值，然后在给定的时间内忽略随后发出的值，并重复。

`combineAll`当可观察值完成时，通过合并最新值来拉平可观察值。

`concatAll`运算符按顺序组合内部可观测值的发射值。