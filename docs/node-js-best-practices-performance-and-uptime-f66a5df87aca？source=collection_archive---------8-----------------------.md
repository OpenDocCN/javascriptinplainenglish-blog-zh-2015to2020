# Node.js 最佳实践—性能和正常运行时间

> 原文：<https://javascript.plainenglish.io/node-js-best-practices-performance-and-uptime-f66a5df87aca?source=collection_archive---------8----------------------->

![](img/3a508652c54908c5cda4077e0e8fd8eb.png)

Photo by [Alex Machado](https://unsplash.com/@alexmachado?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# **监控**

我们必须监控我们的应用程序，以确保它处于健康状态。

重启次数、资源使用情况等表明了我们的应用程序的运行情况。

我们使用这些工具来很好地了解生产中正在发生的事情

# **将任何可能的事情委托给反向代理**

凡是反向代理能做的事，就应该由它来做。

所以我们可以用 Nginx 这样的一个来做 SSL、gzipping 等事情。

否则，我们的应用程序将忙于处理网络任务，而不是处理应用程序的核心任务。

Express 为所有这些任务提供了中间件，但最好将它们卸载到反向代理，因为 Node 的单线程模型会让我们的应用程序忙于执行网络任务。

# **利用 SSL**

SSL 是已知的，因为我们不希望攻击者窥探我们的客户端和服务器之间的通信通道。

我们应该为此使用反向代理，这样我们就可以将此委托给 it，而不是使用我们的 SSL 应用程序。

# **智能记录**

我们可以使用日志平台来使日志记录和搜索变得更加容易。

否则，我们的应用程序所做的一切都是一个黑盒。

如果出现问题，我们将很难解决问题，因为我们不知道发生了什么。

# **锁定依赖关系**

应该锁定依赖项，以便在没有显式更改版本的情况下不会安装新版本。

每个环境中的 NPM 配置应该具有确切的版本，而不是每个软件包的最新版本。

我们可以使用`npm shrinkwrap`来更好地控制依赖关系。

它们被 NPM 默认锁定，所以我们不必担心这个。

# **确保符合错误管理最佳实践**

应该处理错误，以便我们有一个稳定的应用程序。

同步和异步代码的错误处理是不同的。

我们必须了解他们的不同之处。

承诺调用`catch`来捕捉错误。

同步代码使用`catch`块来捕捉错误。

我们不希望我们的应用程序在进行解析无效 JSON、使用未定义变量等基本操作时崩溃。

# **使用正确的工具保护流程正常运行时间**

必须防止节点进程出现故障。

这意味着当它们崩溃时必须自动重启。

像 PM2 这样的流程经理会为我们做这些，甚至更多。

它还为我们提供了集群管理功能，而无需修改我们的应用程序代码。

还有像 Forever 这样的其他工具也在做同样的事情。

# **使用所有 CPU 内核**

我们应该使用所有 CPU 内核来运行我们的节点应用程序。

Node.js 只能在一个没有集群的 CPU 核上运行。

这意味着默认情况下，它只会让其他内核空闲。

我们可以使用 Docker 或基于 Linux init 系统的部署脚本来复制这个过程。

![](img/295d48cf62ab2bb32317d152982d75ea.png)

Photo by [Pero Kalimero](https://unsplash.com/@pericakalimerica?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以做很多事情来最大化我们的节点应用程序的性能。

我们可以改进日志记录和监控，使故障排除更加容易。

## **用简单英语写的 JavaScript**

你知道我们有三份出版物和一个 YouTube 频道吗？在[T3【plain English . io找到一切的链接！](https://plainenglish.io/)