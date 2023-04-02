# 使用 Fastify 的服务器端开发—错误处理程序、路由列表和解析内容

> 原文：<https://javascript.plainenglish.io/server-side-development-with-fastify-error-handler-route-list-and-parsing-content-43f3da9f00ba?source=collection_archive---------4----------------------->

![](img/66c66549b5d0d2f90bec0f1574c1cc71.png)

Photo by [Harley-Davidson](https://unsplash.com/@harleydavidson?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Fastify 是一个用于开发后端网络应用程序的小型 Node 框架。

在本文中，我们将了解如何使用 Fastify 创建后端应用程序。

# 设置错误处理程序

我们可以调用`setErrorHandler`作为错误处理程序。

例如，我们可以写:

```
const fastify = require('fastify')({
  logger: true
})fastify.setErrorHandler(function (error, request, reply) {
  this.log.error(error)
  reply.status(409).send({ ok: false })
})fastify.get('/', async (request, reply) => {  
  return 'hello world'
})const start = async () => {
  try {
    await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)
    await fastify.close()
    process.exit(1)
  }
}
start()
```

用我们的处理程序调用`setErrorHandler`。

我们可以通过`error.statusCode`属性得到状态码:

```
const fastify = require('fastify')({
  logger: true
})fastify.setErrorHandler(function (error, request, reply) {
  const { statusCode } = error.statusCode
  if (statusCode >= 500) {
    this.log.error(error)
  } else if (statusCode >= 400) {
    this.log.info(error)
  } else {
    this.log.error(error)
  }  
  reply.status(409).send({ ok: false })
})fastify.get('/', async (request, reply) => {  
  return 'hello world'
})const start = async () => {
  try {
    await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)
    await fastify.close()
    process.exit(1)
  }
}
start()
```

我们在错误处理程序中检查`statusCode`。

# 打印路线列表

我们可以通过`fastify.printRoutes`方式打印路线列表。

例如，我们可以写:

```
const fastify = require('fastify')({
  logger: true
})fastify.ready(() => {
  console.log(fastify.printRoutes())
})fastify.get('/', async (request, reply) => {  
  return 'hello world'
})const start = async () => {
  try {
    await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)    
    process.exit(1)
  }
}
start()
```

打印`fastify.ready`处理程序中的路由监听。

我们得到:

```
└── / (GET)
```

因为我们用`get`定义了`/`路线。

# 内容类型解析器

我们可以用`fastify.addContentTypeParser`方法添加自己的内容类型解析器。

例如，我们可以写:

```
const fastify = require('fastify')({
  logger: true
})fastify.addContentTypeParser('text/json', { asString: true }, fastify.getDefaultJsonParser('ignore', 'ignore'))fastify.get('/', async (request, reply) => {  
  return 'hello world'
})const start = async () => {
  try {
    await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)    
    process.exit(1)
  }
}
start()
```

我们为`text/json` MIME 类型添加了内容类型处理程序。

# 错误处理程序

我们可以用`fastify.errorHandler` 方法添加一个错误处理程序。

例如，我们可以写:

```
const fastify = require('fastify')({
  logger: true
})fastify.get('/', {
  errorHandler: (error, request, reply) => {
    if (error.code === 'SOMETHING_SPECIFIC') {
      reply.send({ custom: 'response' })
      return
    }
    fastify.errorHandler(error, request, response)
  }
}, (request, reply) => {  
  return 'hello world'
})const start = async () => {
  try {
    await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)    
    process.exit(1)
  }
}
start()
```

添加带有错误处理程序的`/`路由。

我们检查`error.code`属性以检查引发的错误类型。

当发生错误时，我们调用`reply.send`返回响应。

# 结论

我们可以设置一个错误处理程序，内容类型解析器，并使用 Fastify 打印一个路由列表。

喜欢这篇文章吗？如果是这样，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获得更多类似的内容！**