# 更多 rjs 转换运算符-扫描和窗口

> 原文：<https://javascript.plainenglish.io/more-rxjs-transformation-operators-scan-and-window-191a18e5c4e7?source=collection_archive---------5----------------------->

![](img/c47f4ac9a97291b4d3669c9a722faa99.png)

Photo by [George Lucian Rusu](https://unsplash.com/@rgeorgelucian?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

rjs 是一个进行反应式编程的库。创建运算符对于从各种数据源生成要由观察者订阅的数据非常有用。

在本文中，我们将了解如何使用转换运算符`scan`、`switchMap`、`switchMapTo`和`window`。

# 扫描

`scan`运算符通过组合发射值对源观测值应用累加器函数。然后，它返回每个中间结果，并带有一个可选的种子值。

它最多包含 2 个参数。第一个是`accumulator`函数，它是将到目前为止累积的值与新发射值相结合的函数。

第二个是可选参数，即`seed`或初始累加值。

例如，我们可以按如下方式使用它:

```
import { of } from "rxjs";
import { scan } from "rxjs/operators";const of$ = of(1, 2, 3);
const seed = 0;
const count = of$.pipe(scan((acc, val) => acc + val, seed));
count.subscribe(x => console.log(x));
```

上面的代码以`of`可观测值和`seed`初始值 0 开始。然后我们`pipe`将`of$`可观察到的值传给`scan`算子，该算子具有累加功能，将累加的值加到新发射的值上。我们还将`seed`设置为 0。

然后我们同意由此产生的`count`可观察值。

# switchMap

`switchMap`运算符将每个源值投影到一个可观测值，然后该可观测值被合并到一个输出可观测值中。仅发射最近投影的可观测值。

它最多包含 2 个参数。第一个是`project`函数，它将源可观测值的发射值作为参数，然后从中返回一个新的可观测值。

第二个是可选的`resultSelector`参数。这是一个让我们从新的可观测值中选择结果的函数。

我们可以按如下方式使用它:

```
import { of } from "rxjs";
import { switchMap } from "rxjs/operators";const switched = of(1, 2, 3).pipe(switchMap(x => of(x, x * 2, x * 3)));
switched.subscribe(x => console.log(x));
```

上面的代码将`of(1, 2, 3)`可观察到的值取出后`pipe`将结果输入`switchMap`运算符，该运算符具有映射`of(1, 2, 3)`可观察到的值并返回`of(x, x * 2, x * 3)`的功能，其中`x`是从`of(1, 2, 3)`可观察到的 1、2 或 3。

这意味着我们得到的`1`、`1*2`和`1*3`分别是 1、2 和 3。然后对 2 做同样的处理，所以我们得到`2`、`2*2`和`2*3`，分别是 2、4 和 6。最后得到`3`、`3*2`、`3*3`，其中 3、6、9。

因此，我们得到以下输出:

```
1
2
3
2
4
6
3
6
9
```

![](img/3ddd8f7eaadc05159fb458c355ea3c5d.png)

Photo by [Flavio Gasperini](https://unsplash.com/@flaviewxvx?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# switchMapTo

`switchMapTo`运算符将每个源值投影到同一个可观察值上。然后，通过输出可观察值中的`switchMap`,可观察值被平坦化多次。

它最多需要两个参数。第一个是一个可观测值，我们用它替换源可观测值的每个发射值。

第二个是可选参数，即`resultSelector`函数，它让我们从新的可观察值中选择值。

例如，我们可以如下使用它:

```
import { of, interval } from "rxjs";
import { switchMapTo, take } from "rxjs/operators";const switched = of(1, 2, 3).pipe(switchMapTo(interval(1000).pipe(take(3))));
switched.subscribe(x => console.log(x));
```

上面的代码将把可观察到的`of(1, 2, 3)`的发射值映射到`interval(1000).pipe(take(3))`中，后者以 1 秒的间隔发射从 0 到 3 的值。

结果将是我们得到 0、1、2 和 3 作为输出。

# 窗户

当`windowBoundaries`可观察值发出时，`window`操作符将源可观察值分支为嵌套可观察值。

它采用一个参数，即用于完成前一个窗口并开始一个新窗口的`windowBoundaries`可观察值。

像`buffer`一样，它缓冲发出的值，然后在满足某些条件时一次发出所有的值，但发出的是可观察值而不是数组。

例如，我们可以如下使用它:

```
import { interval, timer } from "rxjs";
import { window, mergeAll, map, take } from "rxjs/operators";const timer$ = timer(3000, 1000);
const sec = interval(6000);
const result = timer$.pipe(
  window(sec),
  map(win => win.pipe(take(2))),
  mergeAll()
);
result.subscribe(x => console.log(x));
```

上面的代码有`timer(3000, 1000)` Observable，它从初始化后的 3 秒钟开始每秒发出值。

我们还有一个每 6 秒钟发射一次数字的`sec`观测器。我们使用`window`操作符来创建窗口。这意味着来自`timer$`可观测值的值将发出值，然后这些值将被`pipe` d 到`window`算子。

然后，这将通过管道传递给`map`操作符，该操作符将获取从`window`操作符发出的前 2 个值。然后所有的结果会用`mergeAll`合并在一起。

最后，我们从每 6 秒发射的`timer$`中得到 2 个数字。

`scan`操作符通过组合发射值对源观测值应用累加器函数。然后，它返回每个中间结果，并带有一个可选的种子值。

`switchMap`将每个源值投影到一个可观测值，然后合并成一个输出可观测值。只有来自最近预测的可观测值被发出。

像`switchMap`，`switchMapTo`项目每个源值一个可观察值。但与`switchMap`不同的是，它投射到同样的可观察物体上。然后，通过输出可观察值中的`switchMap`,可观察值被平坦化多次。

当用于关闭和打开窗口的`windowBoundaries`可观察值发出时，`window`操作符将源可观察值分支为嵌套可观察值。