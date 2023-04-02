# 更有用的 Rxjs 创建运算符

> 原文：<https://javascript.plainenglish.io/more-useful-rxjs-creation-operators-60f18c625349?source=collection_archive---------8----------------------->

![](img/641a1dfb061b2812cd1bdb2bfbc28fda.png)

Photo by [Jonatan Pie](https://unsplash.com/@r3dmax?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Rxjs 是一个用于进行反应式编程的库。创建操作符对于从各种数据源生成供观察者订阅的数据非常有用。

在本文中，我们将研究来自 Rxjs 的更多创建操作符，比如从事件处理程序创建观察值的函数，生成发出数字的观察值的函数，以及让我们有条件地订阅观察值的函数。

# 来自事件

`fromEvent`函数创建一个可观察对象，当它发生在事件目标上时，发出我们指定类型的事件对象。

它最多需要 4 个参数。第一个参数是事件目标。这是必须的。它可以是要附加事件处理程序的 DOM 事件目标、Node.js 事件发射器、jQuery like 事件目标、NodeList 或 HTMLCollection。

第二个参数是事件目标发出的事件名称。

第三个参数是带有一些选项的可选参数。

最后一个参数是可选的结果选择器函数。

每次订阅观察对象时，事件处理函数将在给定的事件类型上注册到事件目标。

当事件触发时，事件对象将由可观察对象发出。

事件目标通过 duck typing 进行检查。如果对象上的对象公开了以下方法，我们可以安全地在对象上使用`fromEvent`:

*   DOM 事件目标，如果它有`addEventLister`和`removeEventListener`方法
*   Node.js 事件发射器，如果它有`addListener`和`removeListener`方法的话
*   jQuery 样式对象，如果它有`on`和`off`方法的话
*   DOM NodeLists 或 HtmlCollection，如果它们有一个由 DOM 节点的`document.querySelectorAll`或`childNodes`属性等方法返回的 DOM 节点列表。

我们可以使用`fromEvent`如下:

```
import { fromEvent } from "rxjs";const clicks = fromEvent(window, "click");
clicks.subscribe(x => console.log(x));
```

上面的代码监视对`document`对象的点击。当我们单击浏览器选项卡上的任意位置时，我们应该会看到记录的`MouseEvent`对象。

# fromEventPattern

这类似于`fromEvent`，除了我们传入了用于添加事件监听器和移除事件监听器的处理函数。

它需要三个参数。前两个分别是添加和删除处理程序。移除处理程序是可选的。最后一个参数是可选的结果选择器函数，用于操作发出的值。

例如，我们可以使用它来检测 ID 为`app`的`div`上的点击，如下所示:

```
import { fromEventPattern } from "rxjs";const app = document.querySelector("#app");function addClickHandler(handler) {
  app.addEventListener("click", handler);
}function removeClickHandler(handler) {
  app.removeEventListener("click", handler);
}const clicks = fromEventPattern(addClickHandler, removeClickHandler);
clicks.subscribe(x => console.log(x));
```

当我们单击 ID 为`app`的 div 上的任意位置时，我们应该会看到记录的`MouseEvent`对象。

# 产生

`generate`函数通过传入初始状态创建一个带有值流的可观察对象，一个带有结束值发送的条件的函数，一个遍历值的函数，一个选择发送结果的结果选择器函数，以及一个改变值发送时间的调度器对象。

只需要初始状态。例如，我们可以通过编写以下内容来创建一个发出值 0 到 9 的可观察对象:

```
import { generate } from "rxjs";const generated = generate(0, x => x < 10, x => x + 1);
generated.subscribe(
  value => console.log(value),
  err => {}
);
```

`generate`函数调用的第一个参数具有要发出的第一个值或初始状态。第二个有结束条件，小于 10。最后一个参数具有指示如何继续前进并发出下一个项目以及向第二个参数中的结束条件前进的功能。

# 间隔

`interval`创建一个在特定时间间隔内发出连续数字的可观察对象。

它有两个可选参数。第一个是由调度程序的时钟决定的毫秒数或时间单位。默认值为 0。

第二个参数是要使用的调度器，默认为`async`。

例如:

```
import { interval } from "rxjs";const numbers = interval(1000);
```

创建每秒发出一个新值的可观察对象。

# 关于

这就产生了一个可观察的论点。

它需要无数个参数。

例如，我们可以如下使用它:

```
import { of } from "rxjs";of(1, 2, 3).subscribe(
  val => console.log(val),
  err => console.log(err),
  () => console.log("end")
);
```

# 范围

我们可以使用`range`来创建一个在指定范围内发出一系列数字的可观察对象。

它有 3 个可选参数，这是开始，默认为 0。要生成的整数个数，默认为`undefined`和要使用的调度程序，默认为`undefined`。

例如，我们可以使用它来创建一个可观察对象，它发出数字 1 到 20，如下所示:

```
import { range } from "rxjs";const numbers = range(1, 20);
numbers.subscribe(x => console.log(x));
```

![](img/345f62c3dd1d5cfe13d4e2382f314185.png)

Photo by [Thiago Cerqueira](https://unsplash.com/@thiiagocerqueira?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 投掷误差

创建一个只发出错误通知的可观察对象。

它最多需要两个参数。第一个是要发出的错误。第二个是可选的调度器参数，让我们选择调度器。默认为`undefined`。

我们可以如下使用它:

```
import { throwError } from "rxjs";const numbers = throwError("error");
numbers.subscribe(() => {}, err => console.log(err));
```

我们订阅了第二个参数中的错误通知。

# 计时器

`timer`创建一个在指定时间后开始发出值并在指定间隔后发出不断增加的数字的可观察对象。

第一个参数是可观测开始发射的时间，默认为 0。数字以毫秒为单位。

第二个参数是发射值的周期，默认为`undefined`。数字以毫秒为单位。

最后一个参数是要使用的调度器，默认为`undefined`。

例如，我们可以创建一个可观察对象，在 2 秒钟后发出值，然后通过编写以下内容每秒发出一次值:

```
import { timer } from "rxjs";const numbers = timer(2000, 1000);
numbers.subscribe(x => console.log(x));
```

# 间接免疫荧光

让我们创建一个观察对象，它决定在订阅时订阅哪个观察对象。

最多需要 3 个参数。第一个是选择可观察对象的条件。第二个和第三个参数是分别在条件为真和假时选择的可观察值。

例如，我们可以如下使用它:

```
import { iif, of } from "rxjs";let wantFoo;
const fooBar = iif(() => wantFoo, of("foo"), of("bar"));wantFoo = true;
fooBar.subscribe(val => console.log(val));wantFoo = false;
fooBar.subscribe(val => console.log(val));
```

在上面的代码中，如果`wantFoo`是`true`，当`of('foo')`被订阅时。否则，`of('bar')`就订阅了。

我们可以用`fromEvent`创建操作符创建处理 DOM 或 Node.js 事件的 Observables。

运算符让我们可以从任何对象列表中创建可观测量。

`throw`只抛出一个错误，其他什么都不做。

`generate`、`interval`和`range`让我们创建发出数字范围的可观测量。

`timer`让我们创建定时观察值，而`iif`让我们创建条件观察值。