# 一见钟情

> 原文：<https://javascript.plainenglish.io/graphql-love-at-second-sight-6aa0b33c20bb?source=collection_archive---------10----------------------->

![](img/9fc3c79116af75cd6619cc7a517ae220.png)

我的第一个 GraphQL 项目是在几年前。我是作为一个承包商工作的，我的角色是开发一个反应灵敏的网站，对后端 API 有一点了解。这是几个月的项目，我对 GraphQL / Apollo Client 了解不多。当时，GraphQL 没有给我留下太多印象，直到去年才再次使用。

去年，我加入了 [Clevertech](https://www.clevertech.biz/) 。我开始使用 GraphQL 和 Apollo(服务器/客户端)一起进行一个项目。最初的几周很艰难，但这次 GraphQL 和生态系统给我留下了深刻的印象。

多年来，我一直使用 REST 来开发 API。那么，从不同的背景来看，为什么 GraphQL 第二次引起了我的注意呢？

**首先:**在 GraphQL Schema 中定义 API 操作的可能性。该模式定义了可能的查询(用于请求数据)、突变(用于修改)以及输入和响应的类型。客户端应用程序将在没有任何外部文档的情况下知道哪些查询/突变应该使用，预期的输入是什么，响应，他们可以询问、获取哪些字段等。当客户端执行查询或变异时，GraphQL 服务器将根据定义的模式验证客户端请求，如果它请求无效字段，或者调用缺失/错误的查询/变异，将会失败。

**第二:**客户端(移动应用、网络应用等)。)可以询问他们需要什么，并将得到什么。这是 GraphQL 的主要好处之一:GraphQL 服务器将只执行和返回客户端要求的内容，不多不少。这不仅对客户端有好处，而且服务器只会获取、处理和返回客户端需要的东西，仅此而已。

例如，我们的用户/聊天消息模式可以是这样的:

```
type Query {
  users: [User]
}type User {
  id: ID!
  name: String!
  email: String!
  receivedMessages: [ChatMessage]!
  sentMessages: [ChatMessage]!
}type ChatMessage {
  id: ID!
  text: String!
  sender: User!
  recipient: User!
}
```

想象一下，一个移动应用程序调用一个用户查询，但只需要两个字段:

```
{
  query {
    users {
      id
      name
    }
  }
}
```

管理网站可以调用相同的用户查询并询问更多信息，例如，id、名称、电子邮件和包含发件人信息的接收邮件:

```
{
  query {
    users {
      id
      name
      email
      receivedMessages {
        id
        text
        sender {
          id
          name
        }
      }
    }
  }
}
```

此功能有助于解决多个客户端访问相同的 API 并需要不同信息的问题。有了 GraphQL，我们将不会有取多或取少的问题。此外，它可以在一个请求中获得更多资源:将获得一个用户列表，每个用户一个消息列表，每个用户一个发送者。

**第三** : API 版本控制。这在 REST APIs 中可能很难，通常开发人员最终会添加“v1”、“v2”等。因为否则我们可能会在 API 中有突破性的变化。在 GraphQL 中，我们只需要在现有类型中添加新的类型和新的字段，而不需要创建中断更改。

最后但同样重要的是，生态系统。通过在 web 和移动应用程序上使用 Apollo Client，我获得了良好的开发体验，并提高了工作效率。Apollo 客户端支持开箱即用:查询数据、改变数据、状态管理和缓存。在多年处理不同的状态管理、网络访问、缓存等工具之后。这是我迄今为止发现的最大好处之一。

我对 GraphQL、Apollo 服务器/客户端感到非常兴奋，但与每一个工具一样，它可能会给我们的日常工作带来一些问题和挑战。在接下来的帖子中，我将讨论一些艰难学习的特性，比如模式设计、N+1 问题以及我在这个过程中发现的其他问题。