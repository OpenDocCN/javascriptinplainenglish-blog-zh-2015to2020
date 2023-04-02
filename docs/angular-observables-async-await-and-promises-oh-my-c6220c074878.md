# Angular: Observables，async/await，and Promises，oh my！

> 原文：<https://javascript.plainenglish.io/angular-observables-async-await-and-promises-oh-my-c6220c074878?source=collection_archive---------1----------------------->

![](img/218fb0bb83b9344f3940d8d8264b7407.png)

Angular 来自 Angular2 之前的 Angular.js 世界，Angular(在撰写本文时已经是第 5 版)坚持使用[观察者/可观察设计模式](https://en.wikipedia.org/wiki/Observer_pattern)，看起来令人生畏。无论你往哪里看，事物似乎都返回一个 [RxJS 可观察](http://reactivex.io/rxjs/class/es6/Observable.js~Observable.html)而不是我们都知道的美好熟悉的承诺(甚至可能是爱？).当尝试学习 Angular 时，这非常令人沮丧，我的本能反应是使用 Observables 提供的非常方便的 toPromise()方法，并采取简单的方法，但我说服自己学习它，因为我确信这一切都是有原因的。现在我想我终于明白了，我想分享我的发现。

所以让我们从基础开始——为什么要使用可观察的，而不是完全依赖承诺？嗯，这个很简单——你可以在 stackoverflow 上阅读[这个更详细的答案，但是它的要点是，Observables 允许你取消一个正在进行的任务，它们允许你返回多个东西，并且允许你有多个订阅者订阅一个单独的 Observable 实例。它还简化了重试。](https://stackoverflow.com/questions/37364973/angular-promise-vs-observable)

假设您有以下基于承诺的代码:

```
doAsyncPromiseThing()
  .then(() => console.log("I'm done!"))
  .catch(() => console.log("Error'd out"));
```

在“可观察的土地”中，这实际上并不复杂:

```
doAsyncObservableThing()
  .subscribe(
     () => console.log("I'm done!"),
     () => console.log("Error'd out")
  )
```

# 你自己的第一次观察

我读过的很多指南都是直接用 observables 进行 HTTP 请求，因为这实际上是你最常使用它们的地方，但是我想在这篇文章中做一些不同的事情，了解 Observables 的本质，而不是试图马上使它变得实用。那么，我们如何使用 Observable 以异步方式简单地返回值呢？有两种方法——我们可以使用它的构造函数，或者使用 create 方法，这两种方法做同样的事情(它们是彼此的别名)。

顺便说一下，本文的其余部分将基于这里的一些 helper 方法，因此对于第一个示例，我将发布完整的组件源代码，以便您可以更容易地理解 Plnkr 之类的内容。

```
//our root app component
import {Component, NgModule} from '@angular/core'
import {BrowserModule} from '@angular/platform-browser'
import {Observable} from 'rxjs/Observable';@Component({
  selector: 'my-app',
  template: `
    <div>
      <h2>Observable Example</h2>
      <ul>
        <li *ngFor="let message of messages">{{message}}</li>
      </ul>
    </div>
  `,
})
export class App {
  constructor() {
    this.initialTime = Date.now();
    this.messages = [];
    this.log = (m) => {
      const dateDifference = Date.now() - this.initialTime;
      this.messages.push(`${dateDifference}: ${m}`);
    };
    this.doAsyncObservableThing = new Observable(observer => {
      observer.next('Hello, observable world!');
      observer.complete();
    });

    this.doAsyncObservableThing.subscribe(
      this.log
    );
  }
}@NgModule({
  imports: [ BrowserModule ],
  declarations: [ App ],
  bootstrap: [ App ]
})
export class AppModule {}
```

所以当你运行这个程序时，你应该会看到“你好，可观测的世界！”因为可观察的事物会立即完成。请注意，我的助手“log”方法将应用程序加载后的微秒数添加到每条消息中，这样我们就可以对下一个例子更感兴趣:需要一些时间才能完成的事情。在我们到达那里之前，请注意我们的观察者有一个下一个完整的方法，我们在上面的例子中使用。虽然很诱人的看法”。subscribe()"类似于"。然后()”的一个承诺，这是远离真理的。事实上，next()可以被多次调用，因为一个可观察对象可以返回多个结果。其实有 ***无限和有限的*** 。顾名思义，有限的可观测量返回固定数量的结果，而无限的可观测量可以永远继续下去。

我还想在这里补充一点，对于这些常见任务中的许多，有非常简单的简化方法，就像我们刚刚在上面所做的那样——例如，不用像我在那里所做的那样定义可观察对象，人们可以简单地做:

```
this.doAsyncObservableThing = Observable.of('Hello, observable world!');
```

它做的事情和上面完全一样，而且简单得多。这类似于 Angular.js 世界中的$q.when('Hello ')。请注意，您需要将以下导入添加到文件的顶部，以便在 Angular 中访问这些帮助器:

```
import 'rxjs/add/observable/of';
```

这里要注意的另一个有趣的事情是[你不需要退订有限的可观测量](https://stackoverflow.com/questions/38008334/angular-rxjs-when-should-i-unsubscribe-from-subscription)——RxJS 会帮你处理好的。如果您尝试以下操作，看看会发生什么:

```
this.doAsyncObservableThing = new Observable(observer => {
  observer.next('Started');
  setTimeout(() => {
    observer.next('Hello, observable world!');
  }, 1000);
  setTimeout(() => {
    observer.next('Done');
    observer.complete();
  }, 2000);
});
```

您将看到以下内容:

*   0:已开始
*   1001:你好，可观测的世界！
*   2001 年:完成

请注意，根据具体情况，您可能会更好地使用类似于 [delay()](https://www.learnrxjs.io/operators/utility/delay.html) 的东西，而不是 setTimeout 来为您的可观测量计时；我只是在这里使用 setTimeout 来说明一个观点。

因此，因为我们仍然使用 subscribe 方法，所以我们将保持订阅状态，并在每次我们期望的时候调用我们的 log 函数，但是我们没有办法知道可观察对象何时完全完成，即使这是一个有限的可观察对象，我们在这里调用“observer.complete()”方法。因此，为了纠正这一点，让我们稍微修改一下我们的订阅:

```
this.doAsyncObservableThing = new Observable(observer => {
  observer.next('Started');
  setTimeout(() => {
    observer.next('Hello, observable world!');
  }, 1000);
  setTimeout(() => {
    observer.complete();
  }, 2000);
});

this.doAsyncObservableThing.forEach(
  this.log
).then(() => {
  this.log('Done');
});
```

在上面的例子中，我将 subscribe()方法更改为 forEach()。forEach()方法返回…一个承诺！因此，我们可以简单地对 forEach()的结果执行一个. then()，当可观察对象完全完成时，就会调用 forEach()。经验法则是，当你期望某件事情发生一次并完成时，你可能应该使用 subscribe()，如果你期望多个结果，你可能应该使用 forEach()。

# 链接可观测量

当我开始使用 Observable 时，最让我头疼的事情之一是如何将它们链接在一起——具体来说，我有一个场景，其中我有两个需要顺序发生的 HTTP 请求，中间有一些结果处理。

简单的解决方法是在订阅中简单地订阅:

```
this.doAsyncObservableThing = new Observable(observer => {
  setTimeout(() => {
    observer.next('Hello, observable world!');
    observer.complete();
  }, 1000);
});

this.doAsyncObservableThing.subscribe((val) => {
  this.log(val);
  this.doAsyncObservableThing.subscribe((val) => {
    this.log(val);
  });
});
```

请注意，上述内容将会起作用，您将会看到以下内容:

*   1001:你好，可观测的世界！
*   2001 年:你好，可观测的世界！

然而，代码非常难看，任何稍微复杂一点的东西都会很快变得不可维护。那么我们能做什么呢？另一个简单的解决方案是对可观测量使用 toPromise()方法，并以这种方式将它们链接在一起，但这也是一种简单的方法，并没有真正考虑可观测量。那怎么办呢？你可以做几件事，但在此之前，重要的是要明白**与承诺不同，你不能像链接 then()的**那样一直链接 subscribes()。相反，有一些解决方案取决于你在寻找什么。

# 立即开始所有任务，逐个获得结果(合并)

首先，我们尝试使用 import 'rxjs/add/operator/merge '导入 merge；

接下来让我们试试

```
this.doAsyncObservableThing('First')
.merge(this.doAsyncObservableThing('Second'))
.subscribe((v) => {
  this.log(v);
});
```

我们的结果看起来像这样:

*   1002:第一
*   1002:第二

本实验的关键是 subscribe()中的回调被调用两次，一次用于“第一次”,一次用于“第二次”,但是时间间隔是从同一时间开始的——计时确认两次都在一秒钟后完成。

# 序列中的可观察值，使用异步/等待

现在，下一个将使用 toPromise()，因为我还没有找到更好的方法来解决这个问题，但是它使用得很少，并且仍然是我所发现的最干净的方法来完成我们对嵌套的 subscribe()所做的事情，而不实际嵌套它们，那就是使用 async 和 await 关键字。首先，我们必须将我们的代码移到 NgOnInit 方法中，因为构造函数不能是异步的，这就是我们到目前为止的代码；接下来，让我们简单谈谈 async 和 await 关键字。

Async 是一个关键字，表示允许一个方法使用 await 关键字，并且它返回一个承诺(您必须确保指定一个与承诺兼容的返回值，例如承诺<yourresult>)。await 关键字挂起异步函数的执行，直到承诺被解析或拒绝—它必须始终跟有一个计算结果为承诺的表达式。让我们来看看实际情况:</yourresult>

```
this.log(await this.doAsyncObservableThing('First').toPromise());
this.log(await this.doAsyncObservableThing('Second').toPromise());
```

请注意，对 this.log()的调用将被挂起，直到可以对涉及 await 的表达式求值，这只是在承诺解析之后。这意味着我们的结果看起来像这样:

*   1005:第一
*   2009:第二

在我们预期的时间内，第二个可观察到的现象直到第一个结束才开始。

# 怪癖

# route.queryParams 不能与 await 一起使用

当您回读当前路线中的查询参数时，必须使用 observable:route . query params；这里是官方文件。然而，对该可观察对象的 toPromise()执行 await 不起作用——您的执行将在该点停止，并且不会继续。我花了比我愿意承认的更长的时间才弄明白这一点，但事实证明 queryParams 是一个无限可观察的例子——订阅它将导致您的订阅在每次查询参数改变时被调用，而不是只在那一刻获得当前的查询参数。这意味着 observer.complete()永远不会被其中的内部机制调用，这意味着对它的 await 操作永远不会完成。如果您只想获得一次查询参数，这里的技巧是导入 take 或 first 运算符，以便只获得第一个结果(执行时的结果)，如下所示:

```
import 'rxjs/add/operator/take';
// ...
const queryParams = await this.route.queryParamMap.take(1).toPromise();
// access queryParams as needed
```

感谢阅读，希望这篇文章对你有所帮助！如果你想看上面的代码运行，看看[这个 plnkr](http://embed.plnkr.co/FpBcaV/?show=preview,src%2Fapp.ts) 。

# 进一步阅读

以下是学习观察的一些其他主题，并推荐进一步阅读:

*   取消可观测量
*   取消订阅观察
*   处理错误和捕捉异常
*   编写包含可观察性的单元测试
*   使用[管道()对可观察的结果应用 map()、reduce()和 filter()](https://blog.hackages.io/rxjs-5-5-piping-all-the-things-9d469d1b3f44)
*   [“冷”和“热”可观测量的概念](https://blog.thoughtram.io/angular/2016/06/16/cold-vs-hot-observables.html)(例如，只有在有订户时才开始做事情的可观测量与有订户或没有订户都立即做事情的可观测量)
*   [distinctUntilChanged()](https://www.learnrxjs.io/operators/filtering/distinctuntilchanged.html) 和[去抖时间()](https://www.learnrxjs.io/operators/filtering/debouncetime.html)

*注:本文原载于我的旧博客 2018/01/09，移至此处。*