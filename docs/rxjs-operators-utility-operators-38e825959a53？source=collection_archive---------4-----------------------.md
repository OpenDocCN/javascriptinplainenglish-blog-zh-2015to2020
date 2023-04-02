# Rxjs 操作员—公用事业操作员

> 原文：<https://javascript.plainenglish.io/rxjs-operators-utility-operators-38e825959a53?source=collection_archive---------4----------------------->

![](img/ad6074bf78eece33ba05d8d8f6667b84.png)

Photo by [George Lucian Rusu](https://unsplash.com/@rgeorgelucian?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Rxjs 是一个用于进行反应式编程的库。创建操作符对于从各种数据源生成供观察者订阅的数据非常有用。

在本文中，我们将看看一些帮助我们做各种事情的实用操作符，包括`tap`、`delay`、`delayWhen`和`dematerialize`操作符。

# 轻敲，水龙头

`tap`运算符返回一个可观测值，让我们对源可观测值的每次发射执行一些副作用，并返回来自源的相同发射值。

它需要 3 个可选参数。第一个是一个`nextOrObserver`对象，它是一个普通的可观察对象或对`next`的回调。第二个是`error`回调，这是对源代码中错误的回调。最后是`complete`回调，当源可观察性完成时调用。

它让我们通过运行一个函数来拦截污染源的排放。然后，它返回与源可观测值相同的输出。

例如，我们可以如下使用它:

```
import { of } from "rxjs";
import { tap } from "rxjs/operators";
const of$ = of(1, 2, 3);
const result = of$.pipe(
  tap(
    val => console.log(`value: ${val}`),
    error => console.log(error),
    () => console.log("complete")
  )
);
result.subscribe(x => console.log(x));
```

那么我们应该得到:

```
value: 1
1
value: 2
2
value: 3
3
complete
```

因为我们记录了发出的值:

```
val => console.log(`value: ${val}`)
```

当`of$`观察完成时，我们记录了`'complete'`,通过写:

```
() => console.log("complete")
```

# 耽搁

`delay`运算符返回一个可观测值，该可观测值将可观测源的项目发射延迟指定的`delay`值或直到给定日期。

它最多需要两个参数。第一个是`delay`，它是以毫秒为单位的延迟持续时间，或者是一个`Date`，直到源项目的发射被延迟。

第二个参数是可选的`scheduler`，默认为`async`。它让我们通过返回的可观察值来改变发送值的计时机制。

例如，我们可以如下使用它:

```
import { of } from "rxjs";
import { delay } from "rxjs/operators";
const of$ = of(1, 2, 3);
const result = of$.pipe(delay(1000));
result.subscribe(x => console.log(x));
```

例如，我们可以如下使用它，通过`delay(1000)`将来自`of$`的可观察值的发射延迟一秒。

我们应该在延迟后得到 1、2 和 3。

此外，我们可以将发射延迟到特定日期，如下所示:

```
import { of } from "rxjs";
import { delay } from "rxjs/operators";
import moment from "moment";
const of$ = of(1, 2, 3);
const result = of$.pipe(
  delay(
    moment()
      .add(5, "seconds")
      .toDate()
  )
);
result.subscribe(x => console.log(x));
```

上述代码将从当前日期和时间起延迟 5 秒钟从`of$`发出值。

输出应该保持不变。

# 延迟时间

`delayWhen`运算符返回一个可观测值，该可观测值将可观测源的发射延迟一个给定的时间间隔，该时间间隔由另一个可观测值的发射决定。

它最多需要两个参数。第一个是`delayDurationSelector`函数，它为源可观察对象发出的每个值返回一个可观察对象。当这个函数返回的可观察对象发出时，那么来自源可观察对象的值也将发出。

第二个参数是`subscriptionDelay`的可选参数。它是一个可观察对象，一旦它发出任何值，就会触发对源可观察对象的订阅。

例如，我们可以如下使用它:

```
import { of, interval } from "rxjs";
import { delayWhen } from "rxjs/operators";
const of$ = of(1, 2, 3);
const result = of$.pipe(delayWhen(val => interval(Math.random() * 5000)));
result.subscribe(x => console.log(x));
```

上面的代码将延迟发射每一个由`of$`发出的可观察到的值`Math.random() * 5000`毫秒。这意味着每个值将在`interval(Math.random() * 5000)`发出时发出。

结果将是 1、2 和 3 将以随机顺序发射。

![](img/7f255706a3eb4deca18fb3f53dcf64fd.png)

Photo by [Hudson Hintze](https://unsplash.com/@hudsonhintze?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 消失

`dematerialize`返回一个可观察对象，将`Notification`对象的可观察对象转化为它们所代表的发射。

它不需要参数。

例如，我们可以如下使用它:

```
import { of, Notification } from "rxjs";
import { dematerialize } from "rxjs/operators";const notifA = new Notification("N", 1);
const notifB = new Notification("N", 2);
const notifE = new Notification("E", undefined, new Error("error"));
const materialized = of(notifA, notifB, notifE);
const upperCase = materialized.pipe(dematerialize());
upperCase.subscribe(x => console.log(x), e => console.error(e));
```

那么我们应该得到:

```
1
2
Error: error
```

因为我们将`Notification`对象的值转换成了由返回的可观察对象发出的值。

传递给`Notification`构造函数的第二个参数是为发出的正常值发出的值。`error`是`Notification`构造函数的第三个参数。

`tap`返回一个可观测值，让我们对源可观测值的每次发射执行一些副作用，然后从源发射相同的值。

`delay`返回一个可观测值，该可观测值将可观测源的项目发射延迟指定的`delay`值或直到给定的`Date`。

`delayWhen`返回一个可观测值，该可观测值将源可观测值的发射延迟一个给定的时间间隔，该时间间隔由另一个可观测值的发射决定。

`dematerialize`返回一个可观察对象，它发出由源可观察对象发出的`Notification`对象中设置的值。