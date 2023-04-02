# Node.js 最佳实践—快速应用

> 原文：<https://javascript.plainenglish.io/node-js-best-practices-express-apps-8de9023402e4?source=collection_archive---------7----------------------->

![](img/aef4facce6c4599b18389aa12b1d483e.png)

Photo by [Ethan McArthur](https://unsplash.com/@snowjam?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# 使用 Gzip 压缩

我们应该使用 gzip 压缩来压缩我们的资产。

这样，用户需要下载较少的数据到他们的计算机上。

我们可以使用带有 Express 的`compression`中间件来实现这一点:

```
const compression = require('compression')
const express = require('express')
const app = express()
app.use(compression())
```

对于高流量的网站，我们应该在反向代理中启用 gzip 压缩。

# 不要使用同步函数

对于应用程序的许多部分来说，同步功能绝对不是一个好主意。

任何可能长期运行的代码都应该是异步的。

这样，他们就不会阻止应用程序的其余部分运行

同步调用会大大增加和减慢我们的应用程序。

每当同步代码运行时，我们可以使用`--trace-sync-io`命令行标志来打印警告和堆栈跟踪。

这对于在开发环境中跟踪同步代码很有用。

# 正确记录

为了正确记录，我们应该使用比`console.log`或`console.error`更好的记录器。

一些像 Winston 这样的库可以帮助记录日志。

它比`console`方法有更好的特性。

`debug`模块对于记录调试消息非常有用。

我们可以用`DEBUG`环境变量来启用调试。

# 正确处理异常

我们可以用 try-catch 处理异常，或者用承诺处理`catch`。

此外，我们应该确保我们的应用程序在遇到未处理的错误时自动重启。

最好的方法是避免我们的应用程序崩溃。

我们不应该做的是听`uncaughtException`事件。

当一个异常一路冒泡回到事件循环时，就会发出这个异常。

向`uncaughtException`事件添加一个事件监听器将改变流程的默认行为，而不是遇到异常。

尽管出现异常，该进程仍将继续运行。

因此，流程的状态变得不可靠和不可预测。

我们可以对同步代码使用 try-catch:

```
app.get('/search', function(req, res) {
  const jsonStr = req.query.params
  try {
    const jsonObj = JSON.parse(jsonStr)
    res.send('Success')
  } catch (e) {
    res.status(400).send('Invalid JSON')
  }
})
```

我们可以打电话给`catch`承诺:

```
app.get('/', function (req, res, next) {
  queryDb()
    .then((data)  => {
      // ...
      return makeCsv(data)
    })
    .then(function (csv) {
      // ...
    })
    .catch(next)
})app.use(function (err, req, res, next) {
  // handle error
})
```

我们在`catch`中有`next`，当一个错误发生时它被调用。

这将调用它下面的中间件。

# 将 NODE_ENV 设置为“生产”

我们应该将`NODE_ENV`设置为`production`，以便 Express 缓存视图模板，缓存 CSS 文件，并生成更少的详细警告。

这些变化将使应用程序的速度提高 3 倍。

# 确保我们的应用程序自动重启

当出现错误时，我们的应用程序应该会自动重启。

这样崩溃的时候就不会下线了。

我们可以通过流程管理器来实现这一点。

当节点应用程序崩溃时，它会重新启动应用程序。

我们的操作系统提供的 init 系统可以在没有进程管理器的情况下重启进程。

![](img/a333ef6b484328e4fb2f2b8ecaef47cb.png)

Photo by [Sangga Rima Roman Selia](https://unsplash.com/@sxy_selia?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该做一些像 gzip 压缩这样的基本改变，确保自动重启，并捕捉错误。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**