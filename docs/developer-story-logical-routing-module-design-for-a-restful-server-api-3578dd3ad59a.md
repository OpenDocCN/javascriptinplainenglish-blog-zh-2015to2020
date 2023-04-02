# 开发者故事:RESTful 服务器 API 的逻辑路由模块设计

> 原文：<https://javascript.plainenglish.io/developer-story-logical-routing-module-design-for-a-restful-server-api-3578dd3ad59a?source=collection_archive---------5----------------------->

![](img/53f66fe59f5f99b2ade35162282c2818.png)

在之前的开发者故事条目中，我分享了我的想法和个人技巧，关于如何[设计](https://medium.com/@keithvictordawson/developer-story-designing-a-proper-server-api-structure-5a7a3a7a34fe)一个合适的 RESTful 服务器 API 结构，以及如何[在一个 RESTful 服务器应用中设计](https://medium.com/@keithvictordawson/developer-story-designing-a-proper-server-module-structure-93c03d27808c)一个模块结构。这两篇文章都在应用程序的层面上讨论了其中的每一个主题，但我也想分享一下我设计 *just* 模块的想法和个人技巧，该模块定义了应用程序的所有路由。

根据软件设计原则，应用程序的每个部分都应该服务于一个单一的目的，路由模块的目的应该是为应用程序定义所有的路由，不多也不少。一个简单的想法，但是你会惊讶有多少开发人员在这个关键的模块中加入适当的逻辑。

当考虑路由模块应该如何设计时，首先要问自己的问题是:什么类型的客户机将使用 API？有时答案可能是单一类型的客户端，有时可能有多种类型的客户端。如果应用程序只服务一种类型的客户机，那么模块的形状将比应用程序需要服务多种类型的客户机要简单得多。

考虑路由模块设计时要问自己的第二个问题是:需要向 API 客户机呈现多少不同的逻辑结构？有时答案很简单，以数据库为中心，这意味着 API 是数据库模式的简单的一对一表示。其他时候，答案更复杂，以客户端为中心，这意味着客户端需要数据库内容的非对称表示。

让我们首先讨论划分将要访问应用程序的客户机的问题。这个问题最好先解决，因为答案将帮助您在更高层次上将路由模块划分为逻辑段，然后再考虑在这些段中呈现哪些逻辑结构的细节。

对于许多应用程序来说，这个问题的答案很简单，只有一种类型的客户端会访问应用程序。可以是通过管理门户应用程序访问元数据和其他指标的管理员，也可以是访问公开可用信息的标准用户。

但最终这种类型的客户机只是一个对存储在数据库中的信息有特定视图的用户，API 只提供单一的客户机视图。在这种情况下，产生的路由模块将包含提供该客户端视图的单个段。路由模块将具有非常扁平的结构，看起来如下所示:

```
***routes/***api_resource_a.js
api_resource_b.js
api_resource_c.js
authentication.js
index.js
```

在这个路由模块中，由于 API 只表示一个客户端视图，因此只有一个由一个路由子模块组成的段，每个路由子模块用于客户端需要访问的不同资源，以及一个可选的独立路由子模块，用于验证(如果应用程序提供了这样的功能)。

然而，并不是所有的 API 都服务于单一类型的客户端。有时，API 将为标准用户和管理员提供访问权限，这些用户也可能从不同类型的设备访问 API。

我坚信，这些不同用户类型和环境的组合要求在整个路由模块中有单独的段。如果管理员和用户客户端都从 web 应用程序访问 API，路由模块将具有如下所示的结构:

```
***routes/***admin_api
--> api_resource_a.js
--> api_resource_b.js
--> api_resource_c.js
--> api_resource_d.js
--> api_resource_e.js
--> authentication.js
--> index.js
user_api
--> api_resource_a.js
--> api_resource_b.js
--> api_resource_c.js
--> authentication.js
--> index.js
index.js
```

在这个路由模块中，因为需要有一个由 API 表示的管理员和用户视图，所以有两个部分。与平面路由模块一样，每个段包含一个路由子模块，分别用于管理客户端和用户客户端所需的不同资源，以及一个身份验证路由子模块。

如果管理客户端和用户客户端都从 web 应用程序访问 API，并且用户客户端也从移动应用程序访问 API，则路由模块的结构如下所示:

```
***routes/***mobile_api
--> user_api
    --> api_resource_a.js
    --> api_resource_b.js
    --> api_resource_c.js
    --> authentication.js
    --> index.js
    index.js
web_api
--> admin_api
    --> api_resource_a.js
    --> api_resource_b.js
    --> api_resource_c.js
    --> api_resource_d.js
    --> api_resource_e.js
    --> authentication.js
    --> index.js
--> user_api
    --> api_resource_a.js
    --> api_resource_b.js
    --> api_resource_c.js
    --> authentication.js
    --> index.js
    index.js
index.js
```

除了附加的子模块层之外，该路由模块非常类似于上面先前呈现的版本。从上面服务于 web 应用的两个段包含在 web API 子模块中，而服务于移动应用的段包含在移动 API 子模块中。

无论路由模块以何种方式分段，我认为最重要的事情是确保每个分段为每个上下文的客户端提供底层数据库的唯一视图。这样做最终会使推理将路由连接到数据库的底层业务逻辑变得更加容易。

此外，就实际的业务逻辑以及如何确定哪个业务逻辑最终应用于哪个路由而言，这允许以最小的复杂性适当地组合和构造业务逻辑。

在之前关于[设计](https://medium.com/@keithvictordawson/developer-story-designing-a-proper-server-module-structure-93c03d27808c)一个合适的服务器模块结构的开发者故事条目中，我讨论了我实施这个标准的策略。基本上，整个路由模块分割策略鼓励您将路由模块结构与高级控制器模块结构相匹配，因此两者之间存在一对一的关联。

在讨论了高级路由模块分段策略之后，接下来要讨论的是确定哪些逻辑结构应该反映在每个路由模块段中定义的路由子模块中。使用以数据库为中心的 API，数据库中应该对每个段所服务的客户机可见的各个表或集合应该有自己的 route 子模块。

在上述各种路由模块结构中，`api_resource_a`、`api_resource_b`和`api_resource_c`子模块可以对应于数据库中包含公共可用信息的表格或集合。这就是为什么它们是在用户客户端上下文中定义了路由子模块的唯一的表或集合的原因。

相反地，`api_resource_d`和`api_resource_e`子模块可以对应于数据库中的表或集合，这些表或集合包含只有管理员才被允许查看的特权数据。这就是为什么它们是在管理客户机上下文中定义了路由子模块的唯一的表或集合的原因。

这种在路由模块中划分功能访问的方法很简单，但是根据 API 所服务的客户端所要求的功能，这种方法有时并不合适。在这些情况下，需要以客户端为中心的 API 结构，其中路由子模块被划分为由客户端确定的不同逻辑结构。

由于这种情况下的逻辑结构不依赖于数据库中有限的表或集合列表，所以每个段中的路由子模块列表更加开放，并且由客户端中实现的所有功能驱动。由于在这种情况下可能性是无限的，通过 API 执行的动作的划分更加模糊，并且逻辑地组织动作变得非常困难。

出于这个原因，我通常更喜欢坚持以数据库为中心的 API 设计。这样做允许实现一个[面向资源的设计](https://cloud.google.com/apis/design/resources)模式，应用于每个路由子模块中定义的所有路由。我认为这种设计模式更优越，因为它为客户机指定对哪些数据执行操作提供了一种更快、逻辑上更有效的方式。

我强烈建议您查看面向资源的设计文档，并认真考虑使用它作为在您创建的任何 API 中定义路线的指南。我相信这种模式可以给混乱和不均衡的 API 开发世界带来更多的理智和逻辑。

在讨论了路由模块分段和分段结构策略之后，我想做的最后一件事就是为其中一个路由子模块提供一些示例代码。

正如我在本文开始时提到的，我看到许多开发人员都在为他们放入路由子模块的大量逻辑而奋斗。要实现上述任何一个不明确的路由子模块，代码应该如下所示:

```
***routes/mobile_api/user_api/api_resource_a.js***const express = require('express')
const logger = require('../../../log').app
const util = require('../../../util')const cResourceA = require('../../../controllers').mobile.user.api_resource_aconst api_resource_a = express.Router()api_resource_a.delete('/:api_resource_a_id', async (req, res) => {
  const FUNCTION_NAME = 'DELETE /mobile/user/:api_resource_a_id'
  try {
    const result = await cResourceA.remove(req)
    return result ? res.status(200).json(result) : res.sendStatus(400)
  } catch(e) {
    const error = util.err.createError(e, FUNCTION_NAME)
    logger.error(error.display)
    return res.sendStatus(error.code)
  }
})api_resource_a.get('/', async (req, res) => {
  const FUNCTION_NAME = 'GET /mobile/user/'
  try {
    const result = await cResourceA.retrieveMany(req)
    return result ? res.status(200).json(result) : res.sendStatus(404)
  } catch(e) {
    const error = util.err.createError(e, FUNCTION_NAME)
    logger.error(error.display)
    return res.sendStatus(error.code)
  }
})api_resource_a.get('/:api_resource_a_id', async (req, res) => {
  const FUNCTION_NAME = 'GET /mobile/user/:api_resource_a_id'
  try {
    const result = await cResourceA.retrieveOne(req)
    return result ? res.status(200).json(result) : res.sendStatus(404)
  } catch(e) {
    const error = util.err.createError(e, FUNCTION_NAME)
    logger.error(error.display)
    return res.sendStatus(error.code)
  }
})api_resource_a.put('/:api_resource_a_id', async (req, res) => {
  const FUNCTION_NAME = 'PUT /mobile/user/:api_resource_a_id'
  try {
    const result = await cResourceA.update(req)
    return result ? res.status(200).json(result) : res.sendStatus(400)
  } catch(e) {
    const error = util.err.createError(e, FUNCTION_NAME)
    logger.error(error.display)
    return res.sendStatus(error.code)
  }
})exports = module.exports = api_resource_a
```

在这个代码示例中发生了一些事情，其中一些我已经在以前的开发者故事条目中讨论过了。在整个代码示例中使用`logger`变量是我在关于为 NodeJS 应用程序开发一个[日志](https://medium.com/javascript-in-plain-english/developer-story-always-bring-receipts-ed0ddad4fbcd)解决方案的开发者故事条目中讨论过的。

在我的开发者故事条目中，我讨论了关于为 NodeJS 应用程序开发有效的[错误处理](https://medium.com/javascript-in-plain-english/developer-story-error-handling-fcaa357578cc)解决方案的`util.error`变量的使用以及所有路由定义函数的结构。

除了这些事情之外，我还想在这个代码示例中引起大家的注意。首先，除了每个路由定义函数中的错误处理逻辑之外，每个函数只有两行代码。

第一行代码将接收到的请求传递给`cResourceA`变量上的适当控制器函数，而第二行代码检查从控制器接收到的结果，以确定返回给客户端的适当响应。这实际上是任何路由定义中所需的所有代码。只是简单地将请求信息传递给封装在控制器对象中的业务逻辑，并向客户端返回响应。

当执行到达路线定义时，路线定义的真正重要性已经实现，因为路线本身已经通过使用`express`对象上的函数被定义。

任何额外的逻辑都应该限制在庞大而复杂的业务逻辑层中。这使得所有的 route 子模块简洁明了，当代码中出现问题时，没有混淆的余地:这里由`cResourceA`变量表示的控制器对象。

这里要注意的另一个关键点是，控制器对象在 route 子模块中定义的每条路由上都有一个函数。保持路由子模块和相应控制器子模块之间的这种对称关系还确保了不会混淆错误可能源自何处。

底层逻辑的非对称形状的适当位置(如果有的话)是在业务逻辑层。除了这个简单的路由定义逻辑之外，将客户机请求过滤到数据库并使响应冒泡以返回给客户机所需的一切都包含在这个非常重要的业务逻辑中。路由子模块仅仅是为了定义供客户端访问的接口，因此它们不应该做任何其他事情。

以上总结了我关于如何构建一个合适的路由模块来定义一个逻辑结构化的 API 的想法。这个模块的形状和内容旨在反映类似构造的数据库接口模块，我在我的开发者故事条目中讨论了如何为 NodeJS 应用程序开发一个[单一数据库接口](https://medium.com/swlh/developer-story-single-database-interface-442dd6804424)。

这两个模块都刻意保持精简，因为它们存在的目的很简单。所有复杂和繁重的工作都应该由业务逻辑来完成，业务逻辑构成了 NodeJS 应用程序中的其余代码。

我希望我的路由模块设计策略能够帮助您在自己的服务器 API 应用程序中创建更高质量、更易于理解和维护的路由模块。请继续关注更多的开发者故事条目，因为我将继续我个人项目的进一步进展。