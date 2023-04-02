# Apollo、GraphQL 和 React🚀还有什么比这更简单的呢？

> 原文：<https://javascript.plainenglish.io/apollo-graphql-react-what-could-be-easier-bf34fbaed0c7?source=collection_archive---------1----------------------->

![](img/c329da9d1ee2f78b61bdfbe22ee31f66.png)

**Apollo Client** 是使用 **GraphQL** 构建客户端应用的最佳方式。

有了 **React Apollo** ，我们现在也可以只使用组件轻松管理自己的数据。对我来说，用组件思考是许多令人惊奇的事情之一👍

客户端设置再简单不过了。😃我们将启动一个客户端，并将其 URI 提供给我们的 GraphQL 服务器端点，然后使用 **ApolloProvider** 组件使该客户端在我们的应用程序组件中可用。

**让我们首先**导入必要的依赖项并创建一个**Apolloсclient:**

**💡需要注意的几件事:**

*✔️我们用一个指向我们的****graph QL****端点的****http link****初始化客户端，然后还指定 Apollo 的****inmemorycache****作为缓存实用程序。*

*✔️我们使用****Apollo provider****组件，将它作为道具传递给我们的客户端，然后将其包裹在我们的****app****组件周围。*

# 查询组件

现在一切就绪，我们可以开始针对 **GraphQL** 端点运行查询。

为此，我们将使用 React Apollo 的查询组件，它利用 render *proppattern* 从查询中返回**数据**给我们。

让我们来看看这里发生的一些事情:

*️️* ✔️ React Apollo 的 **<查询/ >** 组件使用一个必需的查询属性和一个已经使用 graphql-tag 的 **gql** 解析的 **GraphQL** 查询

*(* 💡*查询还需要一个必需的子道具，应该是一个函数。)*

数据属性✔️保存从查询返回的**数据**，**错误**属性保存错误对象，如果有的话，并且当查询正在进行时**加载**属性将为真。

⚛️:这就对了！

开始使用 React 和 GraphQL 并开始运行查询的简单方法。

😃**如果有任何问题，可以马上发过来！**