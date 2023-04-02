# Rxjs 运算符—更多连接运算符

> 原文：<https://javascript.plainenglish.io/rxjs-operators-more-join-operators-61aebe60baa7?source=collection_archive---------5----------------------->

![](img/b698ea172059fb71744c383d82b49c4e.png)

Photo by [Bogdan Todoran](https://unsplash.com/@todoranb_26?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Rxjs 是一个用于进行反应式编程的库。创建操作符对于从各种数据源生成供观察者订阅的数据非常有用。

在本文中，我们将看看一些连接操作符，包括`exhaust`、`mergeAll`、`startsWith`和`withLatestFrom`。

# `exhaust`

`exhaust`算子通过在前一个内部可观测量未完成时丢弃内部可观测量，将高阶可观测量转换为一阶可观测量。

它不接受参数，返回一个可观察对象，该可观察对象接受源可观察对象，并从第一个可观察对象独占地传播值，直到它在订阅下一个可观察对象之前完成。

例如，我们可以如下使用它:

```
import { of, interval } from "rxjs";
import { map, exhaust, take } from "rxjs/operators";const of$ = of(1, 2, 3);
const higherOrderObservable = of$.pipe(map(val => interval(500).pipe(take(3))));
const result = higherOrderObservable.pipe(exhaust());
result.subscribe(x => console.log(x));
```

上面的代码将来自`of$`可观测值的值映射到`interval(500).pipe(take(3))`可观测值，后者每半秒发出高达 2 的数字。

然后我们把`pipe`的`interval(500).pipe(take(3))`可观测量交给`exhaust`的操作者。然后，除了第一个`interval(500).pipe(take(3))`之外的所有可观测量都被丢弃，因为它已经完成发射，而下一个可观测量将被执行。

然后我们得到:

```
0
1
2
```

从`console.log`输出。

# mergeAll

`mergeAll`将高阶可观测量转换为一阶可观测量，一阶可观测量同时传递内部可观测量发出的所有值。

它采用可选的`concurrent`参数，默认为`Number.INFINITY`来表示并发订阅的内部可观察对象的最大数量。

`mergeAll`订阅高阶可观测值中的所有内部可观测值，并将它们的所有值传递给输出可观测值。只有当所有内部可观测量完成时，返回的输出可观测量才完成。

任何由内部可观测量发出的错误都会立即导致由返回的可观测量发出的错误。

例如，我们可以如下使用它:

```
import { of } from "rxjs";
import { map, mergeAll } from "rxjs/operators";const of$ = of(1, 2, 3);
const higherOrderObservable = of$.pipe(map(val => of("a", "b", "c")));
const result = higherOrderObservable.pipe(mergeAll());
result.subscribe(x => console.log(x));
```

上面的代码将每个发射值从`of$`可观察值映射到`of(“a”, “b”, “c”)`可观察值。

然后`mergeAll`操作符订阅所有的`of(“a”, “b”, “c”)`观察值，然后订阅每个观察值，然后发出每个观察值的值。

然后我们得到:

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

来自`console.log`。

我们还可以通过向`mergeAll`操作符传递一个数字来改变`concurrency`。

例如，我们可以写:

```
import { of, interval } from "rxjs";
import { map, mergeAll, take } from "rxjs/operators";const of$ = of(1, 2, 3);
const higherOrderObservable = of$.pipe(
  map(val => interval(1000).pipe(take(2)))
);
const result = higherOrderObservable.pipe(mergeAll(1));
result.subscribe(x => console.log(x));
```

让`mergeAll`订阅从`map`操作符的回调中返回的每个子可观察值，这将使我们:

```
0
1
0
1
0
1
```

来自`console.log`。

或者我们可以将 1 改为 5，如下所示:

```
import { of, interval } from "rxjs";
import { map, mergeAll, take } from "rxjs/operators";const of$ = of(1, 2, 3);
const higherOrderObservable = of$.pipe(
  map(val => interval(1000).pipe(take(2)))
);
const result = higherOrderObservable.pipe(mergeAll(5));
result.subscribe(x => console.log(x));
```

然后我们得到:

```
(3) 0
(3) 1
```

并发订阅时从`console.log`输出。

![](img/5f98d13da325ece909b905a12dea250a.png)

Photo by [Andy Holmes](https://unsplash.com/@andyjh07?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 开始于

`startsWith`返回一个可观察对象，在它开始从源可观察对象发出项目之前，发出我们想要发出的项目。

它接受一个参数，这是一个`array`项，我们希望在返回的可观察对象的源可观察值之前发出这些项。

例如，我们可以如下使用它:

```
import { of } from "rxjs";
import { startWith } from "rxjs/operators";of(1)
  .pipe(startWith("foo", "bar"))
  .subscribe(x => console.log(x));
```

然后我们得到:

```
foo
bar
1
```

作为`console.log`输出。

# withLatestFrom

`withLatestFrom`运算符将源可观测值与其他可观测值组合在一起，创建一个可观测值，仅当源发出时，该可观测值才根据各自的最新值计算得出。

它需要一个参数列表，这是一个可以用来组合值的可观察对象。

例如，我们可以如下使用它:

```
import { fromEvent, interval } from "rxjs";
import { withLatestFrom } from "rxjs/operators";const clicks = fromEvent(window, "click");
const timer = interval(1000);
const result = clicks.pipe(withLatestFrom(timer));
result.subscribe(x => console.log(x));
```

我们用`fromEvent`操作符跟踪浏览器标签的点击。然后我们通过使用`withLatestFrom`操作符将`timer`发出的结果与它结合起来。

最后，`result`可观察对象将发出数组，将来自`clicks`可观察对象的`MouseEvent`对象作为第一个值，将来自`timer`可观察对象的数字作为第二个值。

`exhaust`算子通过在前一个内部可观测量未完成时丢弃内部可观测量，将高阶可观测量转换为一阶可观测量。

`mergeAll`将高阶可观测量转换为一阶可观测量，一阶可观测量同时传递内部可观测量发出的所有值。

`startsWith`在源可观察对象开始发出值之前，返回发出我们想要发出的项目的可观察对象。

`withLatestFrom`运算符将源可观测值与另一个可观测值组合起来，并返回一个可观测值，该可观测值发出来自两者的最新值。