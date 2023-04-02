# 在 Azure 函数中实现中间件模式

> 原文：<https://javascript.plainenglish.io/implement-middleware-pattern-in-azure-functions-d8e9f94626a5?source=collection_archive---------6----------------------->

## 如何在 Azure 函数中实现中间件模式来保持我们的函数整洁

![](img/8cf58b9b91de9b53a623751b19941a58.png)

Movement — Ph: [Burak K](https://www.pexels.com/@weekendplayer)

*也可用* [*西班牙语*](https://medium.com/@emanuelcasco/implementando-middlewares-enazure-functions-f3b1d13881ac) *。*

# 介绍

我写这篇文章来分享我在 [Azure Functions](https://azure.microsoft.com/en-us/services/functions/) 中实现中间件模式的经验。

Azure Functions 是一种无服务器的计算服务，使您能够按需运行代码*而无需显式管理基础架构。*

无服务器计算的最大优势是您可以**专注于构建应用** **而不必担心服务器的供应或维护**。您可以只为对您的业务真正重要的事情编写代码。

但是在现实世界的应用程序中，您必须处理业务逻辑之外的一些常见技术问题，比如输入解析和验证、输出序列化、错误处理等等。

通常，所有这些必要的代码最终会污染您的功能中的纯业务逻辑代码，**使得代码更难阅读和维护**。

像 [Express](http://expressjs.com/) 、 [Fastify](http://fastify.io/) 或者[哈比神](https://hapijs.com/)这样的 Web 框架，已经利用[中间件模式](https://en.wikipedia.org/wiki/Middleware)解决了这个问题。这种模式允许开发人员将这些常见的技术问题隔离成**修饰*主要业务逻辑代码的*。**

# **履行**

**在决定在我的项目中实现这个模式之后，我做了一个小的搜索来检查是否有人已经实现了类似的解决方案。**

**可惜我找到的几个解决方案都不符合我的需求，所以我决定自己实现。**

**Azure 中间件就是这样诞生的。**

**[](https://www.npmjs.com/package/azure-middleware) [## azure 中间件

### Azure 函数的 Node.js 中间件引擎

www.npmjs.com](https://www.npmjs.com/package/azure-middleware) 

# 它是如何工作的

## 确认

在无服务器架构中，**必须能够将每个功能的正确行为确定为单独的代码段**。因此，为了避免意外行为，重要的是确保函数输入属于它的域。

为了完成这个任务[，Azure 中间件](https://www.npmjs.com/package/azure-middleware)使用了 [Joi](https://github.com/hapijs/joi) 。它允许我们定义一个模式并检查输入消息是否有效。

使用`validate`方法，您可以定义用于验证消息的方案。如果你的函数被无效消息调用，那么会抛出一个异常，你的函数不会被执行。

```
module.exports = new MiddlewareHandler()
   .validate(invalidJoiSchema)
   .use(functionHandler)
   .catch(errorHandler)
   .listen();
```

## 函数链接

`use`方法用于链接不同的函数处理程序或中间件，如*“步骤”*。它需要一个函数处理程序作为参数。

每个中间件按照定义功能的顺序依次执行。当调用`context.next`时，信息流传递到链的下一个元素。

```
module.exports = new MiddlewareHandler()
   .validate(schema)
   .use((ctx, msg) => {
      ctx.log.info('Print first');
      ctx.next();
   })
   .use((ctx, msg) => {
      ctx.log.info('Print second');
      ctx.done();
   })
   .catch(errorHandler)
   .listen();
```

> `next`是注入`context`的方法。它用于迭代中间件链。

## 错误处理

错误处理是非常相似的，因为它在像 [Express](http://expressjs.com/) 这样的 web 框架中工作。当抛出异常时，将执行中间件链中的第一个错误处理程序。而之前的所有函数处理程序都将被忽略。

此外，您可以使用`next`跳转到下一个错误处理程序。如果这个方法接收一个参数作为第一个参数，那么它将被作为一个错误处理。

此外，您可以使用`context.next`跳转到下一个错误处理程序。如果此方法接收一个非零值作为第一个参数，它将被作为错误处理。

与函数处理程序不同，错误处理程序接收错误作为第一个参数。

```
module.exports = new MiddlewareHandler()
   .use((ctx, msg) => {
      ctx.log.info('Hello world');
      ctx.next('ERROR!');
   })
   .use((ctx, msg) => {
      ctx.log.info('Not executed :(');
      ctx.next();
   })
   .catch((error, ctx, msg) => {
      ctx.log.info(errors); // ERROR!
      ctx.next();
   })
   .listen();
```

# 包裹

这个包还在开发中，我有一些改进它的想法。但是，如果你有任何建议，请不要怀疑，联系我，让我知道它！

[](https://www.npmjs.com/package/azure-middleware) [## azure 中间件

### Azure 函数的 Node.js 中间件引擎

www.npmjs.com](https://www.npmjs.com/package/azure-middleware) 

感谢阅读。如果你对此有想法，一定要留下评论。

你可以在 [Twitter](https://www.linkedin.com/in/emanuelcasco/) 、 [Github](https://github.com/emanuelcasco) 或者 [LinkedIn](https://www.linkedin.com/in/emanuelcasco/) 上关注我。**