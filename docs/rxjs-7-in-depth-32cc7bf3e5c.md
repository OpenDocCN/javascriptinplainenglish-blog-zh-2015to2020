# RxJS 7 深入

> 原文：<https://javascript.plainenglish.io/rxjs-7-in-depth-32cc7bf3e5c?source=collection_archive---------1----------------------->

## RxJS 7 正处于后期测试阶段，发布时间即将到来。让我们发现什么是新的和闪亮的！

![](img/3c704128bb94766b6e52c25cddbb5107.png)

RxJS 是 JavaScript/TypeScript 生态系统中最酷的库之一。它彻底改变了我们在应用程序中处理数据流的方式。

在本文中，我们将发现 RxJS 7 将带来的变化。我们将关注即将到来的特性、错误修复和突破性的变化。

我们开始吧！

# 新的`lastValueFrom` 和`firstValueFrom`方法

当我在 2016 年第一次了解 RxJS 时，我立即知道它提供的承诺完全是虚假的。不幸的是，到目前为止，Observables 仍然没有被纳入 EcmaScript 规范，而且有数以千计的基于 promise 的库。此外，许多人仍然不知道什么是可观的，只知道承诺。这是一种可悲的状态！因此，就目前而言，我们仍然必须处理这个问题，从可观察到的到承诺，反之亦然。

在 RxJS 7 中，有两个新的方法可用:`lastValueFrom`和`firstValueFrom`。增加它们是因为，就目前情况来看，我们都熟悉的`toPromise`方法是有缺陷的。

承诺提供了一个严格的保证，即一个操作要么产生一个值，要么出错。对于`toPromise`，我们无法确定发生了什么。这里有两个例子，摘自本期杂志:

在第一种情况下，我们无法区分在完成之前没有发出值的情况和最后发出未定义值的情况。

有了 RxJS 7，我们将能够更清楚地陈述我们想要从源可观测值中提取出哪个值。

`firstValueFrom`方法通过订阅可观察对象并返回一个承诺，将可观察对象转换为承诺，该承诺将在可观察对象的第一个值到达时立即解决。然后，订阅被关闭。如果可观察的流在发出任何值之前完成，那么返回的承诺将被拒绝，并带有一个`EmptyError`。如果可观察的流发出一个错误，那么返回的承诺将拒绝这个错误。

这里有一个例子:

`lastValueFrom`的工作方式非常相似，除了它等待可观察对象完成，并用来自被观察流的最后一个值解析返回的承诺。同样，如果可观察流在发出任何值之前完成，那么返回的承诺将通过`EmptyError`拒绝，如果可观察流发出错误，那么返回的承诺将通过该错误拒绝。

查看[合并公关](https://github.com/ReactiveX/rxjs/pull/5295)获取更多示例。

这将有望使我们的生活更轻松。注意`toPromise`可能会被弃用。

# 重命名的运算符

这个“新的”`combineLatestWith`操作符实际上是现已废弃的`combineLatest`操作符的[重命名版本](https://github.com/ReactiveX/rxjs/pull/5251)。

这个新名字确实使它更清晰:它创建了一个可观察对象，将所有传递的可观察对象和源的最新值组合成数组并发出它们。当您订阅该操作符返回的可观察对象时，订阅了源可观察对象以及作为参数提供的所有源。一旦所有源发出至少一个值，那么所有最新的值将作为一个数组发出。

[改名为](https://github.com/ReactiveX/rxjs/pull/5249)的另一个算子是`zip`算子，现在叫做`zipWith`。`race`也是一样，现在叫`raceWith`。

重构我们的代码来使用新的操作符应该很容易；-)

# 重试操作员配置

我们现在可以将一个配置选项传递给 retry 操作员，以便在成功时重置计数器:

整洁！

# fromFetch 的选择器支持

`fromFetch`操作符现在让我们[定义一个选择器函数](https://github.com/ReactiveX/rxjs/pull/5306)来从响应中提取我们想要的信息(或者任意数据，如果我们想要的话)。

如果没有定义选择器，那么`fromFetch`简单地按原样返回响应(就像之前一样):

现在，我们也可以提取我们想要的东西:

最后，我们还可以返回任意数据:

# groupBy 的类型保护支持

我们现在可以使用[打字稿类型的警卫](https://basarat.gitbook.io/typescript/type-system/typeguard)和`groupBy`操作符:

这对于类型推断来说非常强大，因为它允许使用像上面的`isNumber`保护一样的自定义类型保护。注意，当然也可以定义内嵌类型的保护。

# 支持带有组合测试的可观察字典

从 RxJS 7 开始，我们将能够向`combineLatest`操作符传递一个字典。当我们这样做时，我们将收到相同的字典结构:

这是一个非常有用的附加功能，可以匹配使用`forkJoin`操作符的可能性。由于这一点，我们将能够编写更多可读的代码，而且能够将`combineLatest`与`pluck` [操作符](https://www.learnrxjs.io/learn-rxjs/operators/transformation/pluck)结合起来。

# 一次暂停来统治所有人

在 RxJS 7 中，我们将获得对超时的改进支持。

首先，我们将能够通过它获得更强大的`TimeoutConfig`:

如您所见，该配置支持不同的选项来精确控制超时:

*   `each`:触发超时之前，源排放之间允许的时间
*   `first`:源头首次排放的最后期限
*   `with`:一个工厂，当超时发生时，我们可以传入它来创建可观察对象。这使得`timeout`操作符可以像另一个操作符`timeoutWith`一样使用
*   …

由于这种新的配置类型，可以非常精确地配置`timeout`操作器。以前，不可能配置超时来进行不同的“第一次”超时检查和随后的“每次”超时检查，或者在第一个值没有及时到达时超时，等等。

查看[该提交](https://github.com/reactivex/rxjs/commit/def1d346b43008bc413a3ac985e1611bbbf62003)以了解更多详细信息。

# 测试调度程序改进

`TestScheduler`现在有一个动画“运行模式”辅助对象，可用于指定何时“绘制”所请求的动画帧。

`animate`助手接受一个大理石图，图中的每个值发射指示“绘制”发生的时间。

你可以在这里了解更多信息。

# 更好的内存使用

RxJS 7 中的内存使用应该得到改善，因为[大多数操作符不再保留外部值](https://github.com/ReactiveX/rxjs/commit/bff18272dca23938a5f5b57cec6eb8d8be5bfddf)。

例如，`mergeMap`中的每个内部订阅以前都保留外部值，如果外部值是一个大数组，那么很快就会出现内存使用问题。

# 大量错误修复

RxJS 7 带来了大量的错误修复。我不会详细介绍每一个，因为这需要花很长时间，但以下是迄今为止的列表:

*   ajax:传递给 progressSubscriber 的部分观察器将不再出错( [25d279f](https://github.com/reactivex/rxjs/commit/25d279f0b45d07f39bfb87b19bc7e2279df8b542)
*   ajax:不可解析的响应将不再阻止抛出完整的 Ajax error([605 ee55](https://github.com/reactivex/rxjs/commit/605ee550e5efc266b5dc5d3a9756c7c3b3968a61))
*   animationFrames:从 rAF 的回调中发出时间戳( [#5438](https://github.com/reactivex/rxjs/issues/5438) ) ( [c980ae6](https://github.com/reactivex/rxjs/commit/c980ae65ee1b585e8ed66a366eb534ac3e50c205) )
*   确保内部订户的退订/拆除是等幂的([# 5465](https://github.com/reactivex/rxjs/issues/5465))([3e 39749](https://github.com/reactivex/rxjs/commit/3e39749a58ca663c17f5f0354b0f27532fb6d319))，关闭 [#5464](https://github.com/reactivex/rxjs/issues/5464)
*   超时:延迟错误创建直到超时发生([# 5497](https://github.com/reactivex/rxjs/issues/5497))([3be 9840](https://github.com/reactivex/rxjs/commit/3be98404fafd5a8de758deb4e0d103a7b60aa31e))，关闭 [#5491](https://github.com/reactivex/rxjs/issues/5491)
*   perf:确保内部订户的退订/拆除是幂等的([# 5465](https://github.com/reactivex/rxjs/issues/5465))([3e 39749](https://github.com/reactivex/rxjs/commit/3e39749a58ca663c17f5f0354b0f27532fb6d319))，关闭 [#5464](https://github.com/reactivex/rxjs/issues/5464)
*   超时:延迟错误创建直到超时( [#5497](https://github.com/reactivex/rxjs/issues/5497) ) ( [3be9840](https://github.com/reactivex/rxjs/commit/3be98404fafd5a8de758deb4e0d103a7b60aa31e) )，关闭 [#5491](https://github.com/reactivex/rxjs/issues/5491)
*   依赖项:将 typedoc 上的意外依赖项移动到 dev-dependencies。( [#5566](https://github.com/reactivex/rxjs/issues/5566) ) ( [45702bf](https://github.com/ReactiveX/rxjs/commit/45702bf6cd1b4a150f47b2a1d273f1ee31ca2482) )
*   null:运算符因空/未定义的输入而中断。([# 5524](https://github.com/reactivex/rxjs/issues/5524))([c5f 6550](https://github.com/reactivex/rxjs/commit/c5f65508505cf1f90560e6be76425e09c455bec3))
*   shareReplay:不再错过来自源的同步值( [92452cc](https://github.com/reactivex/rxjs/commit/92452cc20021141aa0f047c7e5af569a413143e5) )
*   互操作:正确连锁互操作/安全订户取消订阅([# 5472](https://github.com/reactivex/rxjs/issues/5472))([98 ad0eb](https://github.com/reactivex/rxjs/commit/98ad0eba6bc079851b44951f3963e8aae0abf861))，关闭 [#5469](https://github.com/reactivex/rxjs/issues/5469) 、 #5311 、 #2675
*   finalize:用 finalize([# 5239](https://github.com/reactivex/rxjs/issues/5239))([04ba 662](https://github.com/reactivex/rxjs/commit/04ba6621fe9e09238e1796217d04107e52dd36d5))对互操作进行链订阅，关闭 [#5237](https://github.com/reactivex/rxjs/issues/5237) [#5237](https://github.com/reactivex/rxjs/issues/5237)
*   animationFrameScheduler:不同时执行重新安排的动画帧和 asap 动作( [#5399](https://github.com/reactivex/rxjs/issues/5399) ) ( [33c9c8c](https://github.com/reactivex/rxjs/commit/33c9c8cf7e247d4ad4d7318bfd02e8e5bedb0f40) )，关闭 [#4972](https://github.com/reactivex/rxjs/issues/4972) [#5397](https://github.com/reactivex/rxjs/issues/5397)
*   iterables:从 iterables 抛出的错误现在可以正确传播了( [#5444](https://github.com/reactivex/rxjs/issues/5444) ) ( [75d4c2f](https://github.com/reactivex/rxjs/commit/75d4c2f33d2e2121b2a316849044ad17ab28dbaf) )
*   finalize:在源可观察对象被拆除后将调用回调。( [0d7b7c1](https://github.com/reactivex/rxjs/commit/0d7b7c14e34eed43fb2ad1386281800fa3ae8aec) )，关闭 [#5357](https://github.com/reactivex/rxjs/issues/5357)
*   通知:打字改进([# 5478](https://github.com/reactivex/rxjs/issues/5478))([96868 AC](https://github.com/reactivex/rxjs/commit/96868ac754c0147a9aa61182185f27224eb7f11a))
*   TestScheduler:支持空订阅弹珠([# 5502](https://github.com/reactivex/rxjs/issues/5502))([e 65696 e](https://github.com/reactivex/rxjs/commit/e65696e2f7f7338659a873f6653026b33b9011a9))，关闭 [#5499](https://github.com/reactivex/rxjs/issues/5499)
*   扩展:现在可以与异步调度程序( [294b27e](https://github.com/reactivex/rxjs/commit/294b27eb6a96e8edee3af35e6aaaef50628376e4) )一起正常工作
*   订阅:允许无穷大作为有效延迟([# 5500](https://github.com/reactivex/rxjs/issues/5500))([cd7d 649](https://github.com/reactivex/rxjs/commit/cd7d64901e82fd7fb5e8407f1f30828906fac420))
*   主题:解决主题构造函数错误地允许参数( [#5476](https://github.com/reactivex/rxjs/issues/5476) ) ( [e1d35dc](https://github.com/reactivex/rxjs/commit/e1d35dc258edea0237ef49a31f7b34c058755969) )的问题
*   主题:无默认通用( [e678e81](https://github.com/reactivex/rxjs/commit/e678e81ba80f5bcc27b0e956295ce2fc8dfe4576) )
*   defer:不再允许()= > undefined to observable factory(# 5449)([1ae 937 a](https://github.com/reactivex/rxjs/commit/1ae937a8e594aef96b93313bb3c68ea910e6f528))，关闭 [#5449](https://github.com/reactivex/rxjs/issues/5449)
*   single:修正了空观察点上 single(() => false)的行为。(#5325) ( [27931bc](https://github.com/reactivex/rxjs/commit/27931bcfd2aa864e277d3e72128c57e807b28bb0) )，关闭 [#5325](https://github.com/reactivex/rxjs/issues/5325)
*   take/takeLast:在运行时正确断言数字类型(#5326) ( [5efc474](https://github.com/reactivex/rxjs/commit/5efc474161c9196dbdf4803a9cc444a547067549) )，关闭 [#5326](https://github.com/reactivex/rxjs/issues/5326)
*   mergeMapTo:删除冗余/未使用的通用([# 5299](https://github.com/reactivex/rxjs/issues/5299))([d67b 7 da](https://github.com/reactivex/rxjs/commit/d67b7dafbacb3aac8f4dd7f215fe2d2c602f0d36))
*   ajax: AjaxTimeoutErrorImpl 扩展了 Ajax error([# 5226](https://github.com/reactivex/rxjs/issues/5226))([A8 da 8 DC](https://github.com/reactivex/rxjs/commit/a8da8dcc899342d3bb6d2d913247d9e734095287))
*   延迟:尽快发出完整通知( [63b8797](https://github.com/reactivex/rxjs/commit/63b8797fbeed09eb675ea64b0b83607cef1367a9) )，关闭 [#4249](https://github.com/reactivex/rxjs/issues/4249)
*   endWith:将正确键入 N 个参数( [#5246](https://github.com/reactivex/rxjs/issues/5246) ) ( [81ee1f7](https://github.com/reactivex/rxjs/commit/81ee1f72408854f4017615fe7949edf5dd50533b) )
*   fetch:不要泄漏添加到传入信号中的事件监听器( [#5305](https://github.com/reactivex/rxjs/issues/5305) ) ( [d4d6c47](https://github.com/reactivex/rxjs/commit/d4d6c47d8abccc8cbe17e46192fc1eaa42d2d023) )
*   TestScheduler:子类化 TestScheduler 需要 run helpers([# 5138](https://github.com/reactivex/rxjs/issues/5138))([927 d5d 9](https://github.com/reactivex/rxjs/commit/927d5d90ab5f12a79cd50f7290b4f8df1e83ecfc))
*   管道:对 0-arg 情况的特殊处理。([# 4936](https://github.com/reactivex/rxjs/issues/4936))([290 fa 51](https://github.com/reactivex/rxjs/commit/290fa51c44881f25f2fe4cf9885028396c7fd74c))
*   pull:修复 pull 的无所不包签名以获得更好的类型安全性([# 5192](https://github.com/reactivex/rxjs/issues/5192))([E0 C5 b7c](https://github.com/reactivex/rxjs/commit/e0c5b7c790bb9d99fa8bee26c805b5e70c1e456b))
*   拨动:参数类型现在接受数字和符号( [9697b69](https://github.com/reactivex/rxjs/commit/9697b695c23c3dcb614e6a70be63a94ffcd86ed9)
*   startWith:接受 N 个参数并返回正确的类型([# 5247](https://github.com/reactivex/rxjs/issues/5247))([150 ed8b](https://github.com/reactivex/rxjs/commit/150ed8b75909b0e0bb9dc8928287ebdc47e19c51))
*   combineLatestWith:和 zipWith 从 n 参数中推断类型([# 5257](https://github.com/reactivex/rxjs/issues/5257))([3e 282 a5](https://github.com/reactivex/rxjs/commit/3e282a58b1baf7aa03b17142f858bca09a542adf))
*   race:支持静态 race 中的 N 个参数，并确保返回可观察值([# 5286](https://github.com/reactivex/rxjs/issues/5286))([6d 901 CB](https://github.com/reactivex/rxjs/commit/6d901cbb0c0f2aa3fc5a02ef895cc9e9a7a09243))
*   toPromise:更正 toPromise 返回类型( [#5072](https://github.com/reactivex/rxjs/issues/5072) ) ( [b1c3573](https://github.com/reactivex/rxjs/commit/b1c35738204b5b1a5d325a16e70cdbf25b523976) )
*   fromFetch:不要在 from fetch([# 5234](https://github.com/reactivex/rxjs/issues/5234))([37d2d 99](https://github.com/reactivex/rxjs/commit/37d2d99762264ef5faabc0ce4f56d7aab51806dc))中重新分配关闭的参数，关闭 [#5233](https://github.com/reactivex/rxjs/issues/5233) [#5233](https://github.com/reactivex/rxjs/issues/5233)

我还没有被这些攻击过(或者没有意识到)，但是如果你被攻击过，你会很高兴看到这些现在已经被修复了。为团队出色的工作向他们致敬！

顺便说一句，有一个“修复”我没有看到提到，也找不到回来，但我相信过滤操作符的类型推断将在 RxJS 7 中更好地工作:

```
source$.pipe(
  filter((user) => isNotNullOrUndefined(user)),
)
...
```

对于 RxJS 6，在过滤之后，即使我们过滤掉了那些值，推断的类型仍然包括`null | undefined`。用 RxJS 7，应该没问题。

# 重大变化

接下来是不太好的消息。

正如你所猜测的，由于这是一个主要版本，有…突破性的变化！以下是这些问题的概要:

*   `toPromise`操作符现在返回`T | undefined`。这更符合现实，但可能会破坏一些应用程序(温和地)
*   `lift`方法不再暴露。这是 RxJS 的内部实现细节，因此可以从用户代码中访问。它有多个问题，无论如何都不应该被使用。变通方法包括[重写你的操作符](https://rxjs.dev/guide/operators)，使它们返回`new Observable`或者将你的可观察值转换为`any`并访问`lift`，但是当然选择#1 是更好的。选项 2 可能有助于快速修复，但在版本 8 中可能会崩溃
*   当使用超过 7 个参数和一个调度程序调用时,`startWith`操作符返回不正确的类型。此外，不推荐将调度程序传递给该操作符
*   `timestamp`操作符现在接受一个`TimestampProvider`，它是带有返回数字的`now()`方法的任何对象。此[可能会导致`TestScheduler`运行模式出现问题](https://github.com/ReactiveX/rxjs/issues/5553)
*   当提供了一个调度器后,`ReplaySubject`不再调度排放。如果您目前依赖于该行为，那么您需要将它与`observeOn`操作符:`new ReplaySubject(2, 3000).pipe(observeOn(asap))`结合起来
*   对于无效的参数,`takeLast`操作符现在抛出`TypeError`。例如，不带参数或用`NaN`调用它将抛出一个`TypeError`
*   `take`操作符现在为负参数或`NaN`参数抛出运行时错误
*   对于传入的值不存在或者与提供的谓词不匹配的情况，`single`操作符现在将抛出
*   `defer`不再允许工厂退回`void`或`undefined`。所有传递给 defer 的工厂现在必须返回一个正确的`ObservableInput`(例如`Observable`或`Promise`)。要获得与之前相同的行为，您可以使用工厂提供的`return EMPTY`或`return of()`
*   `Notification`和`dematerialize`有更好的类型签名，这可能会破坏现有的代码
*   `Notification.createNext(undefined)`不再每次都返回完全相同的引用
*   `ajax`已放弃对 IE10 及更低版本的支持。是时候放下过去了！

# 毕竟没有与异步变量的互操作性

RxJS 7 的第一个测试版引入了[一流的不可控互操作性](https://github.com/reactivex/rxjs/commit/4fa9d016a83049d014d77b89c56301e42db16b4d)。不幸的是，这种支持在最近的测试版本中被删除了，因为有太多的边缘情况。不过，如果你对这个特性感兴趣，你应该看看 [Ben Lesh](https://twitter.com/BenLesh) 的 [rxjs-for-await](https://github.com/benlesh/rxjs-for-await) 库。

为了记录在案，这里有一点关于该功能可能允许的背景。

顾名思义，异步可迭代对象是我们可以迭代的对象..异步。听起来不错？应该的！从 [ES2018](https://medium.com/@bramus/javascript-whats-new-in-ecmascript-2018-es2018-17ede97f36d5) 开始，我们可以这样写循环:

```
for **await** (variable of iterable) {
  statement
}
```

多亏了异步迭代器和异步可迭代对象，这才成为可能。异步迭代器[的`next()`方法返回一个承诺](https://exploringjs.com/es2018-es2019/ch_asynchronous-iteration.html)，我们可以使用`await`关键字消费它。干净利落。

但是可观测量呢？有了 rxjs-for-await，我们可以使用不同的策略做同样的事情；各有利弊。这些优点和缺点肯定是这个特性暂时不会进入 RxJS 的原因…

# 结论

在这篇文章中，我列出了我可以通过查看不同 RxJS 7 测试版的[变更日志](https://github.com/ReactiveX/rxjs/blob/master/CHANGELOG.md)了解到的不同东西。

希望最终版本会很快发布。有一些很酷的新功能，我迫不及待地想尝试一下。不幸的是，对带有异步 iterables 的互操作的支持被放弃了，这有点令人难过，但我确实理解为什么；安全总比后悔好！

再次向 Ben 和令人敬畏的 RxJS 团队致敬，他们让我们的代码感觉更好。

今天到此为止！

现在，积极地分享这篇文章；-)

*PS:如果你想了解大量关于软件/Web 开发、TypeScript、Angular、React、Vue、Docker/Kubernetes 和其他酷主题的其他酷东西，那么不要犹豫，去* [*拿一本我的书*](https://www.amazon.com/Learn-TypeScript-Building-Applications-understanding-ebook/dp/B081FB89BL) *并订阅* [*我的简讯*](https://mailchi.mp/fb661753d54a/developassion-newsletter) *！*