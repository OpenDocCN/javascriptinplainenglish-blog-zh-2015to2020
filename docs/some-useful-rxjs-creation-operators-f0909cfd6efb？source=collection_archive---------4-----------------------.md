# 一些有用的 Rxjs 创建操作符

> 原文：<https://javascript.plainenglish.io/some-useful-rxjs-creation-operators-f0909cfd6efb?source=collection_archive---------4----------------------->

![](img/1f4a2b3be274e51cb2d5cd96d0ff2b81.png)

Photo by [Bench Accounting](https://unsplash.com/@benchaccounting?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Rxjs 是一个用于进行反应式编程的库。创建操作符对于从各种数据源生成供观察者订阅的数据非常有用。

在本文中，我们将看看来自 Rxjs 的一些创建操作符。

# 埃阿斯

我们可以使用`ajax()`操作符来获取从 API 返回的响应对象。

例如，我们可以如下使用它:

```
const observable = ajax(`[https://api.github.com/meta`).pipe(](https://api.github.com/meta`).pipe()
  map(response => {
    console.log(response);
    return response;
  }),
  catchError(error => {
    console.log(error);
    return of(error);
  })
);observable.subscribe(res => console.log(res));
```

我们用`map`操作符从响应中得到`pipe`数据。此外，我们可以用`catchError`操作符捕捉 HTTP 错误。

同样，我们可以使用`ajax.getJSON()`来简化操作如下:

```
import { ajax } from "rxjs/ajax";
import { map, catchError } from "rxjs/operators";
import { of } from "rxjs";const observable = ajax.getJSON(`[https://api.github.com/meta`).pipe(](https://api.github.com/meta`).pipe()
  map(response => {
    console.log(response);
    return response;
  }),
  catchError(error => {
    console.log("error: ", error);
    return of(error);
  })
);observable.subscribe(res => console.log(res));
```

注意，在这两个例子中，我们在传递给`map`操作符的`map`的回调中返回了`response`。

它还适用于帖子请求:

```
import { ajax } from "rxjs/ajax";
import { map, catchError } from "rxjs/operators";
import { of } from "rxjs";const observable = ajax({
  url: "[https://jsonplaceholder.typicode.com/posts](https://jsonplaceholder.typicode.com/posts)",
  method: "POST",
  headers: {
    "Content-Type": "application/json"
  },
  body: {
    id: 1,
    title: "title",
    body: "body",
    userId: 1
  }
}).pipe(
  map(response => console.log("response: ", response)),
  catchError(error => {
    console.log("error: ", error);
    return of(error);
  })
);observable.subscribe(res => console.log(res));
```

正如我们看到的，我们可以设置请求的头部和主体，所以`ajax`可以处理大多数 HTTP 请求。

错误也可以用我们在`pipe`中使用的`catchError`操作符来捕获:

```
import { ajax } from "rxjs/ajax";
import { map, catchError } from "rxjs/operators";
import { of } from "rxjs";const observable = ajax(`[https://api.github.com/404`).pipe(](https://api.github.com/404`).pipe()
  map(response => {
    console.log(response);
    return response;
  }),
  catchError(error => {
    console.log(error);
    return of(error);
  })
);observable.subscribe(res => console.log(res));
```

# 绑定回调

`bindCallback`将回调 API 转换为返回可观察值的函数。

它可以通过发出参数将带参数的函数转换为可观察的函数。

它需要三个参数。第一个是函数，它将回调函数作为参数。传入回调函数的任何内容都将由可观察对象发出。

第二个参数是可选的`resultSelector`。我们可以传入一个函数来选择这里发出的结果。

最后一个参数是可选的调度程序。如果我们想改变第一个参数中回调函数的调用方式，我们可以传入一个调度器。

```
import { bindCallback } from "rxjs";const foo = fn => {
  fn("a", "b", "c");
};const observableFn = bindCallback(foo);
observableFn().subscribe(res => console.log(res));
```

然后我们会看到`'a'`、`'b'`和`'c'`，因为我们将它们传递给了我们的`fn`回调函数，这是`foo`的一个参数。

然后我们用`bindCallback`函数返回一个返回可观察值的函数。然后我们可以订阅返回的可观察值。

![](img/b8c8839c31e631fb4d8d886f0c834c79.png)

Photo by [Allef Vinicius](https://unsplash.com/@seteales?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 推迟

`defer`让我们创建一个仅在订阅时创建的可观察对象。

它需要一个参数，这是一个可观察的工厂函数。例如，我们可以写:

```
import { defer, of } from "rxjs";const clicksOrInterval = defer(() => {
  return Math.random() > 0.5 ? of([1, 2, 3]) : of([4, 5, 6]);
});
clicksOrInterval.subscribe(x => console.log(x));
```

然后，我们可以有一个可观测值，它或者订阅`of([1, 2, 3])`或者订阅`of([4, 5, 6])`，这取决于`Math.random()`是否返回小于或等于 0.5 或者大于 0.5。

# 空的

创建一个除了完整通知之外不向观察者发出任何信息的可观察对象。

它有一个可选参数，即我们想要使用的调度程序。

例如，我们可以如下使用它:

```
import { empty } from "rxjs";const result = empty();
result.subscribe(x => console.log(x));
```

那么我们应该看不到任何记录。

另一个例子是，当从原始可观测值发出奇数时，发出值`'odd'`:

```
import { empty, interval, of } from "rxjs";
import { mergeMap } from "rxjs/operators";const interval$ = interval(1000);
const result = interval$.pipe(
  mergeMap(x => (x % 2 === 1 ? of("odd") : empty()))
);
result.subscribe(x => console.log(x));
```

# 从

`from`从数组、类数组对象、承诺、可迭代对象或类可观察对象创建可观察对象。

它有两个参数，一个是数组，一个类似数组的对象，一个 promise，iterable 对象或者一个类似 Observable 的对象。

另一个参数是可选参数，它是一个调度程序。

例如，我们可以如下使用它:

```
import { from } from "rxjs";const array = [1, 2, 3];
const result = from(array);result.subscribe(x => console.log(x));
```

我们还可以使用它将承诺转换为可观察值，如下所示:

```
import { from } from "rxjs";const promise = Promise.resolve(1);
const result = from(promise);result.subscribe(x => console.log(x));
```

这对于我们想要这样做的情况是很方便的，比如将`fetch` API 承诺转换成可观察的。

正如我们所看到的，创建操作符对于将各种数据源转化为可观察数据非常有用。

我们有用于获取 HTTP 请求响应的`ajax`操作符。`bindCallback`函数将回调参数转化为可观察的数据。`defer`当某些东西订阅了由`defer`操作符返回的可观测量时，让我们动态地创建可观测量。

最后，我们用`empty`操作符创建一个不发射任何东西的可观察对象，用`from`操作符从数组、类数组对象、promise、iterable 对象或类可观察对象创建可观察对象。