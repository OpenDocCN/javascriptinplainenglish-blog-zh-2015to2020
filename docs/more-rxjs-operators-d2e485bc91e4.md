# 更多 Rxjs 运算符

> 原文：<https://javascript.plainenglish.io/more-rxjs-operators-d2e485bc91e4?source=collection_archive---------3----------------------->

![](img/56786dca29dd4167321a424e8b252ee4.png)

Photo by [Håkon Helberg](https://unsplash.com/@hakonbrakon?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Rxjs 是一个用于进行反应式编程的库。创建操作符对于从各种数据源生成供观察者订阅的数据非常有用。

在本文中，我们将研究一些连接创建操作符，将来自多个可观察对象的数据组合成一个可观察对象。我们将看看`merge`、`race`和`zip`连接创建操作符，以及`buffer`和`bufferCount`转换操作符。

# 连接创建运算符

这些运算符将多个观察器发出的值组合成一个值。

## 合并

`merge`操作符获取多个可观察值，并同时从每个给定的输入可观察值中发出所有值。

它接受一个观察值数组或一个逗号分隔的观察值列表作为参数。

例如，我们可以如下使用它:

```
import { merge, of } from "rxjs";const observable1 = of(1, 2, 3);
const observable2 = of(4, 5, 6);
const combined = merge(observable1, observable2);
combined.subscribe(x => console.log(x));
```

另一个例子是组合多个定时观察值，如下所示:

```
import { merge, interval } from "rxjs";const observable1 = interval(1000);
const observable2 = interval(2000);
const combined = merge(observable1, observable2);
combined.subscribe(x => console.log(x));
```

我们会看到第一个`observable1`会先发出一个值，然后是`observable2`。然后`observable1`会继续每秒发出数值，`observable2` 会每 2 秒发出数值。

## 人种

`race`操作符接受多个观察值，并返回从参数中发出一个项目的观察值。

它以逗号分隔的可观察列表作为参数。

例如，我们可以如下使用它:

```
import { race, of } from "rxjs";const observable1 = of(1, 2, 3);
const observable2 = of(4, 5, 6);
const combined = race(observable1, observable2);
combined.subscribe(x => console.log(x));
```

我们有`observable1`，在`observable2`之前发出数据。我们应该得到输出:

```
1
2
3
```

因为`observable`先发出数值。

## 活力

`zip`运算符组合多个可观察值并返回一个可观察值，其值是根据每个输入可观察值的顺序计算出来的。

它以一系列可观察的事物作为参数。我们可以如下使用它:

```
import { zip, of } from "rxjs";const observable1 = of(1, 2, 3);
const observable2 = of(4, 5, 6);
const combined = zip(observable1, observable2);
combined.subscribe(x => console.log(x));
```

然后我们得到以下结果:

```
[1, 4]
[2, 5]
[3, 6]
```

我们还可以将它们映射到对象，如下所示，以使一个可观察值更容易与另一个区分开来。

为此，我们可以编写以下代码:

```
import { zip, of } from "rxjs";
import { map } from "rxjs/operators";const age$ = of(1, 2, 3);
const name$ = of("John", "Mary", "Jane");
const combined = zip(age$, name$);
combined
  .pipe(map(([age, name]) => ({ age, name })))
  .subscribe(x => console.log(x));
```

![](img/95a8a8c4db6fc2cfed96cb2ff71a2e72.png)

Photo by [Baptist Standaert](https://unsplash.com/@baptiststandaert?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 变换运算符

## 缓冲器

`buffer`操作器缓冲源可观测值，直到`closingNotifier` 发出。

它需要一个参数，即`closingNotifier`。它是一个可观测值，表示输出可观测值上要发出的缓冲信号。

例如，我们可以如下使用它:

```
import { fromEvent, timer } from "rxjs";
import { buffer } from "rxjs/operators";const observable = timer(1000, 1000);
const clicks = fromEvent(document, "click");
const buffered = observable.pipe(buffer(clicks));
buffered.subscribe(x => console.log(x));
```

在上面的代码中，我们有一个由`timer`操作符创建的可观察对象，它在等待 1 秒钟后每秒钟发出一个数字。然后，我们将结果输入到`clicks` Observable 中，当文档被点击时，这个 Observable 就会发出。

这意味着当我们点击页面时，由`buffer`操作符缓冲的发出的数据将发出被缓冲的数据。同时，这意味着当我们点击我们的文档时，我们将得到任何东西，从一个空数组到一个在点击之间发出的值数组。

# 缓冲计数

`bufferCount`与`buffer`略有不同，它会缓冲数据，直到大小达到最大值`bufferSize`。

它有两个参数，一个是`bufferSize`，它是缓冲的最大大小，另一个是`startBufferEvery`参数，它是一个可选参数，表示启动一个新缓冲区的时间间隔。

例如，我们可以如下使用它:

```
import { fromEvent } from "rxjs";
import { bufferCount } from "rxjs/operators";
const clicks = fromEvent(document, "click");
const buffered = clicks.pipe(bufferCount(10));
buffered.subscribe(x => console.log(x));
```

上面的代码将在我们点击 10 次后发出缓冲到数组中的`MouseEvent`对象，因为这是我们 10 个`MouseEvent`对象被原始可观察对象发出的时候。

正如我们所看到的，连接创建操作符让我们能够以多种方式组合 Observables 发出的数据。我们可以选择最先发出的，我们可以将所有发出的数据合并成一个，我们可以同时得到它们。

此外，我们可以缓冲 Observable 发出的数据，并在缓冲了给定的量或者触发事件将发出缓冲区中的数据时发出它们。