# 通过待定承诺回收器提高承诺绩效

> 原文：<https://javascript.plainenglish.io/increasing-promise-performance-with-pending-promise-recycler-52fe6e65d545?source=collection_archive---------12----------------------->

## JavaScript 中最快的承诺是已经解决的承诺！

npm 包[未决承诺回收器](http://npmjs.com/package/pending-promise-recycler)跟踪处于未决状态的承诺；这意味着尚未兑现的承诺。

如果有人试图创建这些待定承诺之一的新实例，待定承诺回收器将**重用当前待定的**而不是不必要地创建一个重复的**。**

# 如何使用待定承诺回收器？

简单地用 pending-promise-recycler 包装任何返回承诺的函数:

```
const **recycle** = require('pending-promise-recycler');const func = () => { return Promise.resolve(); }
const *recycledFunc* = **recycle**(func);*recycledFunc*().then(...);
```

**对** `**recycledFunc**` **的任何额外调用在它仍处于挂起状态时不会导致对** `**func**`的额外调用；相反，未决承诺将被回收并在任何后续调用中再次使用。

# 快速应用程序的实际例子

让我们看一个更现实的例子。下面的代码片段显示了一个简单的 express 服务器，该服务器带有一个调用非常慢且昂贵的函数的端点:

如果我们调用这个端点，函数`someSlowAndExpensiveOperation`将被调用一次。

```
🍕 node index.js &
[1] 81740
Express server up and running!✦ 🍕 curl [http://localhost:3000/](http://localhost:3000/)
Starting a very slow operation!
Slow operation is over!
{"foo":"bar"}
Request: 1517.251ms
```

但是如果我们同时发出 10，000 个呼叫，我们也会呼叫`someSlowAndExpensiveOperation` 10，000 次！即使我们缓存了操作的第一个响应，我们仍然会调用`func` 10，000 次，因为在第二个请求到达时，缓存的响应还没有出现！

## 使用待定承诺回收器改进我们的快递应用程序

如果我们回收`someSlowAndExpensiveOperation`会发生什么？让我们来看看以下改进版的 express 应用程序:

我们现在正在回收缓慢而昂贵的操作。

如果我们使用`apache-bench`向我们的应用程序发出 5 个并发请求…

```
🍕 ab -n 5 -c 5 -k [http://localhost:3000/](http://localhost:3000/)
```

我们的应用程序日志将如下所示:

```
>>> Incoming request
**Starting a very slow operation!**
>>> Incoming request
>>> Incoming request
>>> Incoming request
>>> Incoming request
**Slow operation is over!**
<<< Outgoing response
<<< Outgoing response
<<< Outgoing response
<<< Outgoing response
<<< Outgoing response
```

**我们用函数** `**someSlowAndExpensiveOperation**`中的一个承诺解析来响应所有这些并发请求，而不是重复调用这个函数 5 次。

![](img/6bd4bfd6420709fb5190709f1b6b0ae5.png)

模块待定承诺回收器在 [npm](https://www.npmjs.com/package/pending-promise-recycler) 和 [GitHub](https://www.npmjs.com/package/pending-promise-recycler) 中可用。

非常欢迎投稿和问题报告！