# 用 Node.js 构建 gRPC 服务

> 原文：<https://javascript.plainenglish.io/building-a-grpc-service-with-node-js-370b576d3809?source=collection_archive---------2----------------------->

![](img/cfe548a8fb3354523c4f7db3fc2ed4b8.png)

Photo by [Irvan Smith](https://unsplash.com/@mr_vero?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/javascript?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

有了 gRPC，客户机应用程序可以更容易地直接调用不同机器上的服务器应用程序，就像它是一个本地对象一样。gRPC 的思想是用 Protobuf 文件中的方法定义服务，然后服务器需要实现定义的方法，这样客户端应用程序就能够通过使用定义的方法来调用这些服务。

# 为什么 gRPC 比 REST 好？

## 数据序列化

GRPC 的数据传输采用二进制格式，而其他地方则采用文本格式。GRPC 使用协议缓冲区来序列化数据。与 REST 相比，它可以帮助数据变得更小更快，因为 REST 使用的是可能很大的纯文本。

## 与 HTTP/1 相比，使用 HTTP/2

虽然 REST 可以支持 HTTP/2，但是我们需要使它能够与只能支持 HTTP/2 的 GRPC 相比较。众所周知，HTTP/2 的性能比 HTTP/1 好得多，因为它可以通过一个 TCP 连接并行发送多个数据请求。

# 原蟾蜍

**支付**服务有两种方法，分别是**创建令牌**和**删除令牌**。这些方法中的每一个都提供了请求和响应类型。

# 计算机网络服务器

服务器接收来自客户端的请求，并做出相应的响应。它已经实现了支付服务定义，并开始在端口号为 50051 的本地主机上运行服务器。

# 客户

客户端发送请求并接收响应。它解释了如何与服务器通信。首先，它通过使用端口 50051 拨号到本地主机来建立与服务器的连接。一旦连接建立，它就开始调用像 CreateToken 和 DeleteToken 这样的 RPC 方法。

# 结论

用 JavaScript 实现 Protobuf 并不是特别困难。事实证明，gRPC 比 REST 更快，因为 gRPC 提供了所有的优势，比如数据序列化和 HTTP/2。

总之，这将使 gRPC 的性能优于 REST。

# 参考

[https://grpc.io/docs/languages/node/basics/](https://grpc.io/docs/languages/node/basics/)

[https://www.yonego.com/nl/why-milliseconds-matter/#gref](https://www.yonego.com/nl/why-milliseconds-matter/#gref)