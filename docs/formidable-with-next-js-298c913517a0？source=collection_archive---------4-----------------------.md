# 使用强大的 Next.js API 路由消费文件数据

> 原文：<https://javascript.plainenglish.io/formidable-with-next-js-298c913517a0?source=collection_archive---------4----------------------->

## 改善 API 中的数据流

![](img/fbd5bd94cb7fe9def389d81f383a573b.png)

Photo by [Daria Nepriakhina](https://unsplash.com/@epicantus?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/send?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我们将检查如何在不使用定制服务器的情况下使用强大，从客户端发送表单数据到我们的后端

此示例假设您已经习惯于将文件从客户端发送到后端，从客户端发送文件的基本示例如下:

# Next.js API

关于 Next.js API routes 的好处是，您可以在不使用任何定制服务器的情况下完成许多事情。我们将设置 api 路由来处理传入的表单数据。

这是 Next.js 中 api 路由的基本设置，其中我们有 **req(** request **)** ，其中有内置的中间件帮助我们解析请求，以便访问 cookies、请求正文、请求中的查询等等。

然后我们还有 **res(** response **)** ，它是我们的响应助手，在我们对发送到这个路由的请求做了任何我们想做的事情之后，它会发回一个响应。

如果你到达终点； */api/uploadApi* ，你会得到‘文件已收到’的响应。

Next.js 还为我们提供了一个选项来配置我们的 api，关于被解析的主体的最大大小，我们还可以告诉它我们是否希望我们的主体被解析，在这种情况下，我们需要我们的表单数据作为一个流，因此我们必须禁用 bodyparser

现在我们的设置已经准备好了，我们可以安装获取表单数据所需的所有依赖项，这些数据是微小而强大的。

我们的文件现在看起来像这样:

Micro 将为我们解析数据，而 davidle 将处理所有传入的流，然后我们将所有数据存储到我们的“数据”变量中。如果您想将它发送到数据库，您可以创建一个可读的流并将其附加到表单数据中，然后发送它。