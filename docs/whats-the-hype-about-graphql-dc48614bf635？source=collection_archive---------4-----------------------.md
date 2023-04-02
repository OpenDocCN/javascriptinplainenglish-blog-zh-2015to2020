# GraphQL 有什么好宣传的

> 原文：<https://javascript.plainenglish.io/whats-the-hype-about-graphql-dc48614bf635?source=collection_archive---------4----------------------->

## *graph QL 初学者入门*

![](img/38073def40bd6e784f930a9e82aad03d.png)

Photo by [Michael Browning](https://unsplash.com/@michaelwb?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 为什么我们首先需要它

从前有一篇学术论文告诉我们 API“应该”如何设计，因为我们喜欢一致性，所以它成了创建它们的正式方式。

到那时，我们应该感到休息得很好，但事实证明休息并不十分安宁。

## 休息的问题

如果您在知道每个资源都应该有自己的端点之前使用过 Rest 规则。

例如，如果我们有小狗和玩具，我们必须让小狗穿上:`/api/puppies`

如果我们想要玩具，我们就必须找到它们:`/api/toys`

现在想象这样一个场景，我们想要得到一只小狗的玩具，类似地，我们将获取端点:`/api/puppies/1`

但是我们想要玩具，为什么我们会收到小狗的性别和其他我们不关心的信息？

另一个场景是当我们想要得到两个对象时，我们必须向服务器发送两个请求。

您可能已经注意到在 Rest 模式中有一点冗余**。**

## **GraphQL 如何取代 REST**

**在更大的范围内，尤其是在 facebook，他们注意到这种做事方式并不是很有成效，所以他们不得不采取行动来克服 RESTful 风格 API 的所有缺陷。**

**GraphQL 来拯救我们了，下面是它的样子:**

**就是这样，不多也不少，你只要求你在乎的，忘记其他的。**

## **什么是 GraphQL**

**GraphQL 是一个很长的规范文档，它描述了一种声明式查询语言，您的客户可以使用这种语言向 API 请求他们想要的确切数据。**

## **魔术背后的方法**

**GraphQL 的总体情况由以下元素构成:**

*   **Schema:它负责指定一组**类型**，您将允许您的客户机查询这些类型。**
*   **解析器:它负责指定如何获取客户机查询的数据**

## **整体情况**

**如前所述，GraphQL 只是一个规范，因此我们需要实现来使用它。**

**您可以自己实现，也可以使用用户领域中最好的实现之一。**

**客户端:在客户端，您所关心的是指定您想要在查询中获得的数据，并做一些**缓存**，我们将在接下来的文章中讨论。**

**服务器端:这是所有工作开始的地方，**

**首先，您在服务器上指定模式，它应该如下所示:**

**查询类型包含您允许您的客户端查询的所有内容，在我们的示例中，我们允许客户端查询 Puppy，该查询应该返回 Puppy 类型的元素数组，bang 表示每个元素的返回值应该显式为 Puppy 类型。**

**其次，现在我们有了需要指定如何解析该模式类型的模式，解析器应该如下所示:**

**您可以将解析器视为 MVC 模式中类似控制器的东西。**

**解析器顾名思义就是解析数据。**

**因为 GraphQL 是强类型的，所以解析器的结果将根据客户端请求的字段类型进行验证。**

## **你应该知道的事情**

**GraphQL 有标量类型:String、Int、Boolean、Float、ID。**

1.  **标量被称为叶节点，这意味着 GraphQL 将继续解析返回值，直到找到标量。**
2.  **除了查询类型之外，还有两种其他类型:允许执行创建新元素或更新现有元素等操作的变化，订阅是 GraphQL 实时获取数据的方法。**
3.  **解析器接收三个主要参数，其父类型的结果、由客户端传递给查询的参数、上下文、信息。要更深层次地理解解析器，请查阅这篇精彩的文章。**

**[](https://medium.com/paypal-engineering/graphql-resolvers-best-practices-cd36fdbcef55) [## GraphQL 解析器:最佳实践

### GraphQL 解析器可能很容易构建，但很难做好。以下是 PayPal 在以下方面的最佳实践……

medium.com](https://medium.com/paypal-engineering/graphql-resolvers-best-practices-cd36fdbcef55) 

## GraphQL 与 Rest 的现实

尽管 GraphQL 看起来很神奇，但它也有陷阱。

GraphQL 和 Rest 不应该作为“非此即彼”或“或”的主语来比较，这取决于用例。

为了更实际地比较这些，请查阅本文:

[](https://phil.tech/2017/graphql-vs-rest-overview/) [## GraphQL 与 REST:概述

### 几个月前，我为 Smashing 杂志写了一篇 RPC 和 REST 的比较，现在我想谈谈…

菲尔科技](https://phil.tech/2017/graphql-vs-rest-overview/) 

这是对 GraphQL 的简单介绍。敬请关注关于这个话题的更多细节，黑客快乐！**