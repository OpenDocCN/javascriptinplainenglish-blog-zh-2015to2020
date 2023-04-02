# Node.js 最佳实践—流程管理器

> 原文：<https://javascript.plainenglish.io/node-js-best-practices-process-managers-bbfe7ab0f44b?source=collection_archive---------6----------------------->

![](img/1e3dfb9fe33a501e7775355264c915d5.png)

Photo by [Valerie Blanchett](https://unsplash.com/@valerieblanchett?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# 使用流程管理器

我们应该使用进程管理器来保持我们的节点应用程序运行，即使它崩溃了。

崩溃会导致节点应用程序停止运行。

重启之前它会一直离线。

所以，不能只是用`node app.js`或者类似的东西来运行。

流程管理器帮助我们的应用保持高可用性。

它让我们深入了解运行时性能和资源消耗。

可以动态修改设置以提高性能。

我们还可以使用 StrongLoop PM 和 PM2 来控制集群。

StrongLoop PM 具有针对生产部署的功能。

我们可以在本地构建和打包应用，并将其安全地部署到我们的生产系统中。

此外，它会自动重启我们的应用程序，如果它崩溃。

它还允许我们远程管理集群。

CPU 配置文件和堆快照让我们可以检查内存泄漏和 CPU 使用情况。

我们可以通过 Nginx 负载平衡器的集成控制扩展到多台主机。

# 使用初始化系统

init 系统提供了更高的可靠性，因为它确保应用程序在服务器重启时启动。

它们可能会因为许多原因而关闭，因此我们应该确保我们的应用程序在启动时再次启动。

我们可以在进程管理器中运行我们的应用程序，并将进程管理器作为服务安装在 init 系统中。

当应用程序崩溃时，流程管理器会重启我们的应用程序。

当操作系统崩溃时，init 系统将重启进程管理器。

我们也可以直接在 init 系统中运行我们的应用程序。

这更简单，但是我们没有使用进程管理器的额外特权。

两个主要的初始化系统是 [systemd](https://wiki.debian.org/systemd) 和 [Upstart](http://upstart.ubuntu.com/) 。

# 使用节点的集群模块

Node 的集群模块允许我们创建一个应用程序的多个实例。

它使主进程能够产生工作进程，并在工作进程之间分配传入的连接。

我们可以使用 StrongLoop PM 创建一个集群，而无需修改应用程序代码。

StrongLoop PM 自动在工作线程数量等于系统中 CPU 核心数量的集群中运行。

我们可以使用 slc 程序手动更改集群中工作进程的数量，而无需停止应用程序。

例如，我们可以通过编写以下代码来运行具有给定集群大小的应用程序:

```
slc ctl -C http://prod.example.com:8888 set-size my-app 8
```

我们将集群大小设置为 8，末尾是数字 8。

PM2 还允许我们在不修改应用程序代码的情况下创建集群。

我们必须确保 are app 是无状态的。

这意味着不应该在会话、WebSocket 连接等流程中存储本地数据。

然后，我们可以通过运行以下命令来启用集群模式:

```
$ pm2 start app.js -i 4
$ pm2 start app.js -i max
```

我们运行一个包含 4 个工作进程的集群，最后是 4。

或者我们可以对所有的 CPU 使用`max`并启动那么多的工作进程。

要添加更多的工人，我们可以使用加号:

```
$ pm2 scale app +3
$ pm2 scale app 2
```

我们使用`scale`来改变工人的数量。

![](img/6a9ade16fe393e28dc3142cd7321f5cd.png)

Photo by [Theme Inn](https://unsplash.com/@themeinn?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用进程管理器来创建集群、重启应用程序以及监控硬件使用情况。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**