# 对于您的节点应用程序，您肯定应该使用 NestJS

> 原文：<https://javascript.plainenglish.io/be-cool-get-started-with-nestjs-a-modern-web-framework-for-node-js-31b25e6348d5?source=collection_archive---------0----------------------->

![](img/c43128a90c21a5e3f0a20c6eb0018300.png)

Photo by [Baher Khairy](https://unsplash.com/@baher366?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我将以一个**粗体**语句开始。*用* ***NestJS*** *超级爽*。

它的创造者(作者[*Kamil myliwiec*](https://github.com/kamilmysliwiec)*和其他开源贡献者)从其他伟大的框架中获得了灵感。*

*他们足够聪明，窃取了一些最好的想法:Angular 的模块、存储库、服务、Spring 的实体注释等等。*

***Nest** 是一个使用*类型脚本*的开源 web 框架。*

*让我们开始吧。*

*![](img/af7f0fea0f8c4fd9e1f04502f12f2c44.png)*

*Nest’s bold logo*

# *不管怎样，我为什么需要它？*

*嗯，这很简单。因为这对你很有帮助！*

*想为您的数据库创建一个表吗？没有更简单的了。
只需创建一个 JavaScript 类，并用 **@Entity()对其进行注释/修饰。瞧吧！表已创建。***

*A table in a database now exists. No SQL involved!*

*想要创建一个不错的 API，以便 React 应用程序可以访问您通过 NestJS 应用程序管理的内容吗？*

*创建一个 JavaScript 类，用 **@Controller()** 修饰。*

*现在，您可以在该类中进一步注释您的函数，以响应 HTTP 请求(@ **Get** ， **@Post** 等)。).*

*这些功能的结果然后被发送回客户端(前端网页、手机 App、智能冰箱等等)。*

# *快递呢？*

*每个开始学习 Node.js 的人都会被介绍到 [Express](https://expressjs.com/) ，一个“*极简的 Node*web 框架”。*

*理解 Express 和 NestJS 不是竞争对手是至关重要的。*

*Express 为我们提供了一种公开端点和响应 HTTP 请求的方法。这很好，但是 Nest 做的远不止这些。*

*对了， **Nest 是基于 Express** 的。*

# *Nest 能做什么？让我吃惊！*

1.  *以抽象的方式轻松地与数据库交互。您只需用 decorators 创建和注释类。这被称为对象关系映射(“T39”ORM”听起来可能更熟悉)*
2.  *轻松创建服务层类来处理应用程序的业务逻辑。然后，在 NestJS 的依赖注入的帮助下，在其他服务中注入(阅读:“*使用*”)这些服务*
3.  *创建所谓的“控制器”，它可以很容易地响应 HTTP 请求。这是你与你的 Nest 应用程序的客户端进行交互的主要方式( *React？Vue？手机 app？另一项服务？任何事。*)*
4.  *使用控制器保护您创建的端点。Nest 可以很容易地保护你的应用程序的 API，所以只有(例如)登录的用户可以访问它们(无论是基本认证。、JWT 等。)*
5.  *用 Nest 创建微服务。酷毙了。*
6.  *轻松构建 GraphQL 接口*
7.  *将消息队列集成到您的应用程序中(也许您需要进行一些异步数据处理？)*
8.  *…还有[更多](https://docs.nestjs.com/first-steps)*

# *看看这里…*

*NestJS 在 JavaScript 2019*的*[*状态的**后端技术预测**部分获得第一名。干净利落。*](https://2019.stateofjs.com/awards/)***

**Nest 使用 TypeScript，这是 JavaScript 的静态类型超集。在处理大型复杂的项目时，这很方便。**

**越来越多的开发人员现在使用 Nest 作为他们首选的服务器端 JavaScript web 框架，并且在其中构建的项目数量也在不断增长。**

**一定要试一试！**

# **从这里去哪里…**

**NestJS 的[官方文档](https://docs.nestjs.com/)确实不错。**

**我也开始写一个关于 Nest 的迷你系列，在这里查看第一部分:[https://medium . com/JavaScript-in-plain-English/exploring-nestjs-installing-nestjs-and-getting-started-FB 2 e 4 f 36 b 596](https://medium.com/javascript-in-plain-english/exploring-nestjs-installing-nestjs-and-getting-started-fb2e4f36b596)。**

**干杯。**