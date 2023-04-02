# 快递将主宰未来十年。原因如下。

> 原文：<https://javascript.plainenglish.io/express-will-dominate-the-coming-decade-heres-why-fef06ca89501?source=collection_archive---------0----------------------->

## 随着稳定版本完成第七个月，Express.js 来自卑微的起点，它将挑战旧的国王。

![](img/ffb9e2c92c88f0b0baaf5ccafbd79590.png)

Express 是一个用于 Node.js 的快速、非个人化、极简的 web 框架。它是快速、健壮和异步的。该框架的最初发布是在本世纪初，随着 2010 年的结束，Express 发布了它的第一个稳定版本。

# 为什么用快递？

这可能过于简化了，但是 express 对于 node.js 就像 bootstrap 对于 CSS 和 web 设计一样。Express 已经是 NodeJS 最流行的框架之一，如果不是最流行的话。有时，它几乎是 NodeJS 的同义词。原因如下:

*   非常快的 I/O(输入输出):这是一个顶上有 NodeJS 的樱桃。
*   异步的
*   易于理解的路线

下面是一个要点，它显示了在 Express 中设置路线是多么容易。将其与其他框架进行比较，您将会看到不同之处。

# 表演

Express 绝对不是最快的框架。如果速度是你想要的，那就选择 fastify。这里有一个博客，对 express 和 fastify 进行了彻底的比较。

[](https://medium.com/@onufrienkos/express-vs-fastify-performance-4dd5d73e08e2) [## 快速与快速性能

### 本文的目标是找出哪种 web 框架为 simple Node.js 微服务提供了更好的性能。

medium.com](https://medium.com/@onufrienkos/express-vs-fastify-performance-4dd5d73e08e2) 

然而，express 非常容易使用、理解和实现。我们可以采用不同的策略来提高应用程序的性能。提高 express 性能的最佳实践包括:

*   使用 [gzip](https://www.npmjs.com/package/compression) 。
*   尽可能避免同步函数。
*   承诺是异常处理的必备条件！
*   有一个非常聪明的方法来分配负载。在负载平衡器的帮助下，我们可以运行多个实例。参见 [Nginx](http://nginx.org/en/docs/http/load_balancing.html) 。

除了以上所有内容， [Express 4](https://expressjs.com/en/guide/migrating-4.html) 对这个框架进行了一些突破性的改变。这意味着 Express 2 和 3 应用程序将不再工作，这是一件好事！Express 4 在路由、速度和安全性方面引入了大量重大改进。

有人可能会问，如果有更快的框架可用，那么为什么要学习更慢的呢？从速度上来说，fastify 不是更优的候选表达吗？

我可能会问 Python 同样的问题。如果 C++是更快的语言，那么为什么 Python 如此流行？这两个问题的答案是一样的。Express 和 Python 非常容易使用，并且对开发人员友好。在 Python 中，“Hello World”只是:

```
print(“Hello World!”)
```

这是连二年级学生都能理解的事情。如果你告诉他:

```
cout<<“Hello World”<<endl;
```

他会问什么是 cout，那些“箭头式”符号是什么，什么不是？快递也是一样。它不是最快的框架，但却是最简单的。

快速不是你所说的缓慢。可能没有 fastify 快，但还是很快。此外，由于非阻塞 I/O 流，express with NodeJS 可以同时处理大量请求。

我们可以使用集群模块来允许 JavaScript 在多个 CPU 上并行运行。用户可以通过消息队列、无服务器功能或简单的另一个 API 将任何 CPU 密集型任务卸载到辅助服务器，这样节点服务只需向消息队列发出非阻塞请求，工作就以阻塞方式在完全不同的服务器上完成。

当 Express 推出时，它为许多开发者挠了痒痒。这是第一个具有直观路由声明的框架，并修复了许多过去的问题。开发者只是蜂拥到这个框架中。但是，快递还有很多事情要做，还有很长的路要走！最后，我只能说，罗马不是一天建成的。:-)