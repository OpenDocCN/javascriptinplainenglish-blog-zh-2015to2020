# Node.js 最佳实践—缓存和 REST

> 原文：<https://javascript.plainenglish.io/node-js-best-practices-caching-and-rest-e678c92fe121?source=collection_archive---------4----------------------->

![](img/0e11dd185ed63aceec2610cd35eb4538.png)

Photo by [Joshua Hoehne](https://unsplash.com/@mrthetrain?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# 使用 Redis 启用缓存

我们可以使用 Redis 启用缓存来加速我们的 Express 应用程序。

为此，我们可以通过运行以下命令来安装 Redis:

```
apt update
apt install redis-server
```

然后在`/etc/redis/redis.conf`我们改变:

```
supervised no
```

收件人:

```
supervised systemd
```

那么 Redis 将在 Systemd 下运行。

然后，我们重新启动 Redis 以使更改生效:

```
systemctl restart redis
systemctl status redis
```

然后，我们通过运行以下命令来安装 redis NPM 模块:

```
npm i redis
```

然后我们可以通过写来使用它:

```
const express = require('express')
const app = express()
const redis = require('redis')
​
const redisClient = redis.createClient(6379)
​
async function getData(req, res, next) {
  try {
    //...
    redisClient.setex(id, 3600, JSON.stringify(data))
    res.status(200).send(data)
  } catch (err) {
    console.error(err)
    res.status(500)
  }
}
​
function cache (req, res, next) {
  const { id } = req.params
​
  redisClient.get(id, (err, data) => {
    if (err) {
      return res.status(500).send(err)
    }
    if (data !== null) {
      return res.status(200).send(data)
    }
    next()
  })
}
​
​
app.get('/data/:id', cache, getData)
app.listen(3000, () => console.log(`Server running on Port ${port}`))
```

我们使用 Redis 客户端的节点应用程序来创建 Redis 客户端。

然后我们创建`cache`中间件，如果 Redis 缓存存在的话，它从 Redis 缓存中获取数据。

如果 Redis 存在，我们就从它发送响应数据。

如果不是，那么我们调用我们的`getData`路由中间件从数据库中获取数据。

这是通过`redisClient.setex`方法设置数据来完成的。

# 启用虚拟机/服务器范围的监控和日志记录

我们可以使用各种工具实现服务器或虚拟机监控和日志记录。

通过这种方式，我们可以观察应用程序中出现的任何问题。

# NODE_ENV 的隐藏功能

`NODE_ENV`可以使我们的节点应用程序的性能有很大的不同。

如果我们将其设置为`production`，则启用缓存，以便缓存数据。

我们不希望在开发中出现这种情况，因为我们总是希望看到最新的数据。

视图在生产中缓存，但不在开发节点中缓存。

我们可以用给定的`NODE_ENV`运行它:

```
NODE_ENV=<environment> node server.js
```

其中`server.js`是我们 app 的入口点。

随着生产模式的开启，Express 应用程序不会一直忙于处理 Pug 模板。

由于缓存，CPU 可以自由地做其他事情。

我们可以通过运行以下命令来设置`NODE_ENV`:

```
export NODE_ENV=production
```

在 Linux 和 Mac OS 中。

在 Windows 中，我们可以运行:

```
SET NODE_ENV=production
```

我们可以跑:

```
NODE_ENV=production node my-app.js
```

在所有平台中。

# 使用 HTTP 方法和 API 路由

我们应该使用与 REST 约定相匹配的 HTTP 方法和 API 路由。

例如，我们可以写:

*   `POST /article`或`PUT /article:/id`创建新文章
*   `GET /article`检索文章列表
*   `GET /article/:id`检索文章
*   `PATCH /article/:id`修改现有的`articlerecord`
*   `DELETE /article/:id`删除文章

如果我们通过 ID 得到，我们在最后有 ID 参数。

![](img/f87f56908f098ea9ae584128c2fbf9e4.png)

Photo by [Jan Padilla](https://unsplash.com/@janpadilla?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

缓存并将 NODE_ENV 设置为`production`可以加速我们的应用程序。

REST 约定对于保持我们的 API 一致非常有用。

## 简单英语的 JavaScript

你知道我们有三份出版物和一个 YouTube 频道吗？在 [**寻找一切的链接 plainenglish.io**](https://plainenglish.io/) ！