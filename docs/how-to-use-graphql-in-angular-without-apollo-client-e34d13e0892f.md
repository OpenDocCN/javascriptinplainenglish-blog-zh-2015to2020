# 如何在没有 apollo-client 的情况下在 Angular 中使用 GraphQL

> 原文：<https://javascript.plainenglish.io/how-to-use-graphql-in-angular-without-apollo-client-e34d13e0892f?source=collection_archive---------7----------------------->

![](img/1726c3525f02b25c7758457a68965fbe.png)

Photo by [Sarah Pflug](https://burst.shopify.com/@sarahpflugphoto?utm_campaign=photo_credit&utm_content=Free+Reference+Books+On+Shelves+Photo+%E2%80%94+High+Res+Pictures&utm_medium=referral&utm_source=credit) from [Burst](https://burst.shopify.com/books?utm_campaign=photo_credit&utm_content=Free+Reference+Books+On+Shelves+Photo+%E2%80%94+High+Res+Pictures&utm_medium=referral&utm_source=credit)

我们可能会在应用程序中使用一些第三方库，原因如下:

*   我们需要的功能没有本地实现
*   靠我们自己很难实现
*   它很稳定，并且经过了很好的测试
*   它给了我们一些很好的 API，所以我们可以使用它而不会有任何进一步的复杂性

据我们所知，GraphQL 客户端还没有正式实现，可能是因为脸书团队没有时间做这个(哈哈，真有趣)，或者根本不需要任何客户端实现。

只看简单的 GraphQL 请求(你可以在这里阅读更多关于[的内容)。](https://graphql.org/graphql-js/graphql-clients/)

这只是一个本地 HTTP 请求。我们唯一要做的是，用变量创建一个查询参数，然后马上发送它。

除此之外，我们可以添加任意多的头，例如授权头。我的意思是我们可以使用标准 Ajax 请求可以拥有的所有属性。如果一切都这么简单，那为什么阿波罗客户端会存在？

# 阿波罗客户端

你可以把 apollo-client 想象成这个简单方法的巨大包装器。它在此基础上增加了一些特性，如状态管理、缓存、查询订阅、错误处理等。它给了你一个拥有单一库的机会，你可以用它来做任何与状态和 API 相关的事情。

它还带来了一些有趣的概念，比如使用 GraphQL 查询从本地状态获取数据。

此外，它还附带了非常强大的开发工具。

但凡事都有代价，对吗？。也有一些缺点。

## 复杂性

apollo-client 中最复杂的主题之一是配置，你必须配置几乎所有的东西。首先你必须安装 6-7 个依赖项，然后你需要用缓存和链接参数(最基本的)配置 apollo-client。有一个中间件的概念，每当你想传递一些头，比如一个请求，你必须使用中间件。然后，如果你想有错误处理，你必须安装一个额外的依赖。此外，还有一个状态管理概念，默认情况下所有请求都被缓存。最令人恼火的是，为了弄清楚 Apollo-client 究竟是如何发送哪怕是一个简单的请求的，您必须大量阅读

# 额外功能

还有一些很棒的状态管理库，比如 [ngrx](https://ngrx.io/) 、 [ngxs](https://www.ngxs.io/concepts/state/) 、 [Akita](https://github.com/datorama/akita) ，有时候(几乎每次)你都想使用 GraphQL 作为 Rest API 的替代，只是为了发送请求和检索响应。在这种情况下，使用 apollo 客户端有点不知所措。apollo-client 就像一个框架，大多数情况下，你只是想要一些效用函数。

# 使用本机实现还是 apollo-client？

这要看情况。如果您想简单地将 GraphQL 用于请求/响应，那么请使用本机实现。否则使用 apollo-client。

我开始在 ngrx 上使用 apollo-client，几周后，我发现我在 apollo 上使用的方法只有`query`和`mutate`，所以我移除了 apollo-client，开始使用原生的。如果有类似的情况，一定要使用本机实现。

# Angular 中的本机实现

Angular 中的本机实现和`fetch`请求实现一样简单。我们所需要的是`HttpClient`和一个包装 GraphQL 请求的服务

如你所见，我们有一个简单的服务，有两个方法`mutate`和`query`，它们所做的就是向服务器发送一个`POST`请求，并映射响应