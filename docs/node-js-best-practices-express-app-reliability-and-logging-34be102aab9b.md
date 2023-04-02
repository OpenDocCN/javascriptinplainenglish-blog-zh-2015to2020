# Node.js 最佳实践-快速应用可靠性和日志记录

> 原文：<https://javascript.plainenglish.io/node-js-best-practices-express-app-reliability-and-logging-34be102aab9b?source=collection_archive---------3----------------------->

![](img/833809af6055aa00098b7aacebcb3538.png)

Photo by [Radek Grzybowski](https://unsplash.com/@rgrzybowski?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# 结构化表达应用

我们的快递应用应该有以下文件夹结构:

```
src/
  config/
  controllers/
  providers/
  services/
  models/
  routes.js
  db.js
  app.js
test/
  unit/
  integration/
server.js
(cluster.js)
test.js
```

我们有`src`文件夹，里面有在生产中运行的代码。

`config`已配置。

`controllers`有控制者。

`providers`具有控制器路由的逻辑。

`services`有商业逻辑。

`models`有数据库模型。

`routes.js`装载所有路线。

`db.js`具有数据库路由。

`app.js`加载快速应用程序。

`test`已通过检测。

`unit`已通过单元测试。

`integration`已通过整合测试。

`server.js`在快递应用的入口点。

`cluster.js`是创建集群的可选文件。

`test.js`是运行`test`目录下所有测试的主测试文件。

# 提高 Express.js 的性能和可靠性

有几种方法可以提高性能和可靠性。

# 将 NODE_ENV 设置为生产

我们应该将`NODE_ENV`环境变量设置为`production`，这样我们就可以从生产配置中获益。

这是 dev 的 3 倍。

这是因为有压缩和缓存来让我们的应用程序更快。

我们可以运行以下任一程序:

```
export NODE_ENV=production
```

设置环境变量

或:

```
NODE_ENV=production node server.js
```

设置环境变量并同时运行应用程序。

# 启用 Gzip 压缩

我们可以对资产启用 gzip 压缩，以便对我们的资产进行压缩。

我们可以通过运行以下程序来安装`compression`中间件:

```
npm i compression
```

然后我们可以通过书写来使用它:

```
const compression = require('compression')
const express = require('express')
const app = express()
app.use(compression())
```

这不是进行 gzip 压缩的最佳方式，因为它使用了 Express 应用程序上的资源。

相反，我们可以在 Nginx 中启用 gzip，将工作转移到反向代理上。

# 始终使用异步函数

如果我们有一些简单的操作以外的东西，我们可能应该使用异步代码。

我们应该在大部分时间里做出承诺，或者干脆异步/等待。

例如，我们可以写:

```
(async () => {
  const foo = () => {
    //...
    return val
  }
​
  const val = await asyncFunction;
})()
```

我们有一个`asyncFunction`来回报承诺，所以我们用`await`来得到结果。

我们不能在不同的线程上运行同步函数，因为 Node 是单线程的。

因此，我们只能使用异步代码来运行长期运行的操作。

我们还可以生成子进程或创建应用程序的多个实例，以便在不同的进程上运行不同的任务。

# 正确记录

我们应该在一个中心位置收集我们的日志。

这样，我们可以在需要的时候找到它们。

[Winston](https://www.npmjs.com/package/winston) 和 [Morgan](https://www.npmjs.com/package/morgan) 以及有用的日志包，可以集成其他服务进行集中日志记录。

我们也可以使用一些类似于 [Sematext](https://sematext.com/) 的服务来记录日志。

例如，我们可以写:

```
const { stLogger, stHttpLoggerMiddleware } = require('sematext-agent-express')
​
const express = require('express')
const app = express()
app.use(stHttpLoggerMiddleware)
​

app.get('/api', (req, res, next) => {
  stLogger.info('An info.')
  stLogger.debug('A debug.')
  stLogger.warn('A warning.')
  stLogger.error('An error.') res.send('Hello World.')
})
```

我们有一个`sematext-agent-express`包，它有一个日志记录器来记录 Sematext 服务。

然后，我们通过服务在一个漂亮的仪表板中获取日志。

![](img/c0e8c645e23beffed76e766c6752890c.png)

Photo by [🇨🇭 Claudio Schwarz | @purzlbaum](https://unsplash.com/@purzlbaum?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以更好地构建 Express 应用程序，并在生产模式下运行 production Express 应用程序。

此外，使用集中的日志服务可以很容易地进行日志记录。

## **用简单英语写的 JavaScript**

你知道我们有三份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)找到所有内容的链接！