# RxJS:延迟重试—您将需要构建这个操作符

> 原文：<https://javascript.plainenglish.io/rxjs-retry-with-delay-youll-want-to-build-this-operator-3591261ff5c9?source=collection_archive---------2----------------------->

## 在穿越 RxJS 的土地时，我经常发现自己渴望同一个操作员。延迟重试似乎是一种普遍需要的模式。但是在 RxJS 里是不是无处可寻？

> 你可能是来寻求快速解决方案的。npm 包 [rxjs-boost](https://www.npmjs.com/package/rxjs-boost) 已经提供了一个`retryWithDelay`操作符(也已经过全面测试。)

![](img/c48ba062c16689dc32d003db2e171ce0.png)

Photo by [Alexander Schimmeck](https://unsplash.com/@alschim?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 运营商应该怎么做？

它应该重试`x`次，延迟`y`。在回顾其他方法时，我提出了这些明确的标准:

1.  如果出现错误，重试`x`次。
2.  每次重试前有一个`y`的延迟。
3.  如果没有错误发生，就像它不存在一样发出。
4.  在超过`x`尝试次数的情况下发出上一次抛出的错误。很多解决方案都在为此苦苦挣扎。

# 我们将使用什么来构建操作符？

我们将把 4 个现有的 RxJS 操作符放在一起构建我们自己的`retryWithDelay`操作符:

*   [retryWhen](https://rxjs-dev.firebaseapp.com/api/operators/retryWhen) :镜像源可观测值。将错误映射到作为参数提供的通知程序函数中。如果通知函数发出一个值，则操作符重新订阅源可观测值。转发通知程序函数的错误和完成。
*   [扫描](https://rxjs-dev.firebaseapp.com/api/operators/scan):对发射值应用累加器功能。提供对累加器函数的最后一个值的访问。
*   [轻击](https://rxjs-dev.firebaseapp.com/api/operators/map):对每一项进行一次副作用。
*   [延迟](https://rxjs-dev.firebaseapp.com/api/operators/delay):将接收到的项目延迟给定的超时时间。

# 我们如何构建我们的自定义操作符？

我们将把可观察到的输入输入到`retryWhen`操作符中。在这里，我们重用了另外 3 个运算符:

1.  首先，我们使用`scan`计算操作员重试的次数。我们将每次增加一个计数。我们还会每次覆盖错误属性，并跟踪最后一个错误:

> `MonoTypeOperatorFunction`类型描述了一个 RxJS 操作符，它不修改管道的值类型。正是我们需要的！

2.接下来，如果超过了最大重试次数，我们需要抛出最后一个错误。金额由`count`参数提供。我们可以使用`tap`操作符抛出一个错误:

3.此时，我们已经有了一个工作的`retry`参数。我们只需要添加`delay`操作符。我们已经有一个参数叫做`delay`。为了防止名称隐藏，我们可以将导入重命名为`delayOperator`。然后简单地将它连接到我们的管道上:

# 尝试一下🎉

尝试它很简单。下面的脚本会一直抛出一个错误，直到`i`至少为 4。控制台输出显示每个执行都被传入的参数延迟了。此外，计数选项工作完美！

We use the [defer](https://rxjs-dev.firebaseapp.com/api/index/function/defer) operator here to make sure the observable is newly created on each subscription. You could use [ts-node](https://www.npmjs.com/package/ts-node) to execute this script.

**retryWithDelay(1000，3):**

```
Start at 1
Start at 1009
Start at 2011
Start at 3011
Error at 3011
```

**retryWithDelay(2000，4):**

```
Start at 2
Start at 2011
Start at 4012
Start at 6016
Start at 8017
Complete at 8018
```

## 那么，就我们的起始标准而言，我们的表现如何？

1.  如果出现错误，重试`x`次。✅
2.  每次重试前有一个延迟`y`。✅
3.  如果没有错误发生，就像它不存在一样发出。✅
4.  在超过`x`尝试次数的情况下，发出上一次抛出的错误。许多解决方案都在努力解决这个问题。✅

> 太好了，看来我们完成了！

# 额外收获:全面的测试覆盖

在某些时候，您可能希望在某些生产环境中使用它。您很有希望进行测试，并且也想测试这段漂亮的代码？

我掩护你。 [**该文件**](https://github.com/NiklasPor/rxjs-boost/blob/master/src/operators/retry-with-delay.spec.ts) 包含了完整的测试覆盖(包括延迟等。操作员[的)。](https://github.com/NiklasPor/rxjs-boost/blob/master/src/operators/retry-with-delay.spec.ts)只需从那里复制粘贴即可。测试使用的是 [rxjs-marbles](https://www.npmjs.com/package/rxjs-marbles) ，所以如果你使用不同的弹球库，你可能需要做一些调整。

谢谢你一直坚持到我的文章结束。您刚刚创建了自己的 RxJS 操作符——甚至是一个非常有用的操作符！如果你发现自己有任何未解决的问题，请随时在 Twitter 上联系我或者发表评论👏

祝您愉快！

尼克拉斯