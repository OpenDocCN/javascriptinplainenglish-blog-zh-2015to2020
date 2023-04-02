# 每次需要 Node.js 中的文件时会发生的 5 件事

> 原文：<https://javascript.plainenglish.io/5-things-that-happen-every-time-you-require-a-file-in-node-b654472e0fcb?source=collection_archive---------7----------------------->

![](img/e916245411d92164e7541fdac97ba8d6.png)

每次您需要 node 中的文件时，会发生以下 5 种情况:

*   解决
*   负荷
*   包装
*   评价
*   隐藏物

# 解决

节点将尝试把给定给`require()`的字符串映射到文件系统上的路径。该路径可以是 node 的本地路径，在 node _ modules 文件夹下，或者是父目录下的节点模块或任何其他路径。

# 负荷

节点会将该文件的内容加载到内存中

# 包装

用生命包装文件的内容(我在另一篇博客文章[中讨论过这种生命如何在节点中工作](https://nikhilvijayan.com/how-require-module-exports-in-node-works)

# 评价

然后，节点将评估该文件(例如:使用 V8 引擎)

# 隐藏物

一旦它完成评估文件，它将缓存它。下次您需要相同的文件时，这些步骤将不会发生，它将直接从缓存中读取该文件。

**注**:我是从 Sameer Buna 的[“你不知道 node”](https://www.youtube.com/watch?v=oPo4EQmkjvY)forward js San Francisco 2017 的演讲中得知这一点的。