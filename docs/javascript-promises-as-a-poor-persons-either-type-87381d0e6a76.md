# JavaScript 承诺穷人的两种类型

> 原文：<https://javascript.plainenglish.io/javascript-promises-as-a-poor-persons-either-type-87381d0e6a76?source=collection_archive---------6----------------------->

![](img/0c37d19c063d15edf19cc196cbce9d47.png)

我一直在思考如何优雅地处理同步 Javascript 代码中的异常。

我是[或者](https://gcanti.github.io/fp-ts/modules/Either.ts.html)类型和[未来](https://github.com/fluture-js/Fluture)类型的粉丝。我对什么是“正确”的看法受到在生产代码库中使用它们的严重影响。

# 问题是

我不喜欢这段代码有两个原因。

1.  `let`:暗示`validBreed`的值会随时间发生突变。本质上，它在函数上下文中创建了一些状态，并为其他人通过进一步更改`let`来扩展它创造了空间。
2.  `try-catch`:本质上是创建分支逻辑。也降低了代码的可读性，因为函数存在于`catch`。

# 极端的例子

在极端情况下，这两种方法可以使代码看起来像这样:

我们可能最终用`n`变量来保持状态，并有多条退出路径。

我们可以把这些尝试捆绑在一起，但是我们能做得更好吗？

# 输入承诺

我知道承诺是不同步的，但请迁就我一下:

不可否认，从事件循环中不断推迟会使运行速度变慢。(异步代码中的上下文切换成本更高)。有多慢？非常。下面是您可以自己进行的[基准测试](https://jsbench.me/ybkg9y1kdq/1)。你会注意到它几乎慢了一倍。

# 我觉得有些事情是对的

我第一个承认:故意给同步代码增加上下文切换开销听起来很糟糕。

但我也认为这种方法也有一些优点:

1.  使用链式承诺语法可以避免共享可变状态。(从我的代码中消除状态是我的一个成功之处)
2.  它提高了代码的可读性。很容易推断出没有`let`的代码。
3.  不管是同步代码还是异步代码，代码都是一样的。
4.  代码写起来更简单。(如果您需要引入额外的错误处理，只需在承诺链中插入一个额外的`catch`

此外，上下文切换开销在实际应用中可能不是问题:

1.  如果您使用 JavaScript 执行计算密集型任务，那么您可能做错了。(阅读:不要阻塞事件循环)
2.  JavaScript 程序的主线程大部分时间都处于空闲状态。
3.  无论是每秒 50，000，000 次操作还是每秒 25，000，000 次操作，对于 IO 密集型使用情形来说都没有什么区别。

# 你应该这样写代码吗？

这绝对感觉像是效率和可读性之间的权衡。所以，我不确定。

理想情况下，如果一个函数返回 non-T0，那么拥有一个不会延迟的本机 promise 构造就更好了。

*原载于*[*【nadeeshacabral.com】*](https://nadeeshacabral.com/projects/blogging/promises-poor-mans-either-type/)