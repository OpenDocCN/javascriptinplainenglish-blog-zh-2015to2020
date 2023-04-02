# 使用 Libuv 线程池提高 Node.js 性能

> 原文：<https://javascript.plainenglish.io/increase-node-js-performance-with-libuv-thread-pool-ac893b7a748e?source=collection_archive---------7----------------------->

![](img/993a9f587a4d89b0149dca9fc0ed6769.png)

在我的“ [Node.js 性能优化](https://www.youtube.com/watch?v=ol56smloW2Q&list=PLISqeoHsXJYAIfu4-mgNY0tloWz2uut1t)”系列的第 5 部分中，我将向您展示如何通过线程池管理来提高 [Node.js](https://nodejs.org/en/) 的性能。我们通过理解 [Libuv](https://libuv.org/) 如何工作、线程池如何工作以及如何根据您的机器规格配置线程数量来实现这一点。

您是一名 Node.js 开发人员，还不熟悉 Node.js 的内部工作方式吗？如果是这样的话，您可能正在使用自安装 Node.js 以来就存在的一些默认配置来部署生产应用程序。在本文中，我将介绍一种鲜为人知的配置，它很可能会使您的应用程序的一些操作的性能翻倍。这将取决于许多因素，但很有可能这对很多人来说都是一个胜利。

# 在 YouTube 上观看视频

# Node.js 运行时环境

Node.js 运行时环境由几个移动部分组成。我们都应该熟悉 [Google V8 引擎](https://v8.dev/)，它负责执行我们的 [JavaScript](https://javascript.info/) 逻辑。然而，有一个不太为人所知的库叫做 [Libuv](https://libuv.org/) ，它负责管理异步 I/O 操作。

这些 I/O 操作也被称为与操作系统相关的“重型任务”。文件和文件夹管理、TCP/UDP 事务、压缩、加密等任务。是通过 Libuv 处理的。

现在，虽然这些操作中的大多数在设计上是异步的，但也有一些是同步的，如果处理不当，可能会导致我们的应用程序被阻塞。正是由于这个原因，Libuv 拥有了所谓的“线程池”。

# Libuv 线程池

Libuv 启动一个由 4 个线程组成的线程池，用于将同步操作卸载到。这样做，Libuv 确保我们的应用程序不会被同步任务不必要地阻塞。

在这里，我们将利用一个设置来更好地适应我们的计算机或我们的应用程序将部署到的虚拟机的规格。这是因为我们可以将 4 个线程的默认值更改为最多 1024 个线程。我们通过设置[**UV _ thread pool _ SIZE**](https://nodejs.org/api/cli.html#cli_uv_threadpool_size_size)节点变量来实现这一点。

# 物理与逻辑 CPU 内核

为了更好地理解将 UV_THREADPOOL_SIZE 设置为什么，我们需要首先了解我们的机器正在运行多少个逻辑内核。如果我们以我的 MacBook Pro 为例，它运行 6 个物理 CPU 内核(英特尔)。

但是，这些内核具有超线程功能，这意味着每个内核可以同时运行 2 个操作。因此，我们将 1 个支持超线程的物理内核视为 2 个逻辑内核。在我的情况下，我的 MacBook Pro 运行 12 个逻辑核心。

# 如何提高节点 JS 性能

建议将 **UV_THREADPOOL_SIZE** 设置为您的机器正在运行的逻辑核心数。在我的例子中，我将线程池大小设置为 12。

将大小设置为超过硬件运行的逻辑核心是没有意义的，实际上可能会导致更差的性能。

# 如何检查逻辑核心

当涉及到部署时，您最不想做的事情就是手动设置 **UV_THREADPOOL_SIZE** ，因为您的应用程序可能会在具有不同机器规格的多个环境中运行。因此，我们需要一种在相关环境中启动应用程序时动态设置线程池大小的方法。

好消息是这很简单，但是必须小心处理。为此，将以下代码添加到节点应用程序的根 JS 文件的顶部:

```
const OS = require('os')
process.env.UV_THREADPOOL_SIZE = OS.cpus().length
```

[**OS**](https://nodejs.org/api/os.html) 模块是 Node JS 原生的。它有一个函数[**CPU()**](https://nodejs.org/api/os.html#os_os_cpus)，返回你的机器正在运行的逻辑核心的总量。更好的是，如果您的 cpu 内核没有超线程，这个函数将返回物理 CPU 内核的数量，这是完美的。

# 结论

我相信这篇文章证明是有价值的。我建议观看嵌入式视频，并在 GitHub 上查看我的[源代码报告](https://github.com/bleedingcode/nodejs-performance-optimizations)，其中有这里提到的所有内容的代码示例。

下次见，干杯

*更多内容看* [***说白了. io***](https://plainenglish.io/)