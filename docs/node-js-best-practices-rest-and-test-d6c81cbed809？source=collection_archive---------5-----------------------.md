# Node.js 最佳实践— REST 和 Test

> 原文：<https://javascript.plainenglish.io/node-js-best-practices-rest-and-test-d6c81cbed809?source=collection_archive---------5----------------------->

![](img/9e1fcb49621614be0169d983d088bffb.png)

Photo by [Johann Siemens](https://unsplash.com/@johannsiemens?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# 使用 HTTP 方法和 API 路由

当我们创建端点时，我们应该遵循 RESTful 约定。

我们应该使用名词作为标识符。

例如，我们有这样的路线:

*   `POST /article`或`PUT /article/:id`创建新文章
*   `GET /article`检索文章列表
*   `GET /article/:id`检索文章
*   `PATCH /article/:id`修改现有文章
*   `DELETE /article/:id`删除一篇文章

# 正确使用 HTTP 状态代码

状态代码应该正确地告诉我们的响应状态。

我们可以有以下内容:

*   `2xx`，如果一切顺利。
*   `3xx`，如果资源已经移动
*   `4xx`，如果请求因客户端错误而无法实现
*   `5xx`，如果 API 这边出了问题。

客户端错误是指无效输入或未经授权的凭证。

服务器端错误是指不管什么原因在服务器端抛出的异常。

我们可以使用`res.status`在 Express 中用状态代码进行响应。

例如，我们可以写:

```
res.status(500).send({ error: 'an error occurred' })
```

我们用 500 状态代码和一条消息来响应。

# 使用 HTTP 头发送元数据

HTTP 头让我们发送带有请求和响应的元数据。

它们可以包括诸如分页、速率限制或身份验证等信息。

我们可以通过在关键字前加上前缀`X-`来添加自定义标题。

例如，我们可以发送一个带有`X-Csrf-Token`请求头的 CSRF 令牌。

HTTP 没有对报头定义任何大小限制。

但是，Node 将最大心脏大小限制为 80KB。

# Node.js REST API 的正确框架

我们应该为我们的 REST API 选择正确的框架。

有 Koa，Express，哈比神，Resify，Nest.js 等等。

我们可以使用前 4 个构建简单的 rest 服务。

如果需要更完整的解决方案，可以使用 Nest.js。

它内置了 ORM 和测试等功能。

# 黑盒测试我们的 Node.js REST APIs

为了测试我们的节点 REST APIs，我们可以向我们的 API 发出请求并检查结果。

我们可以使用像 Supertest 这样的专用 HTTP 客户端来测试我们的 API。

例如，为了测试获取一篇文章，我们可以写:

```
const request = require('supertest')describe('GET /user/:id', () => {
  it('returns a user', () => {
    return request(app)
      .get('/article')
      .set('Accept', 'application/json')
      .expect(200, {
        id: '1',
        title: 'title',
        content: 'something'
      }, done)
  })
})
```

我们向`article`端点发出 HTTP 请求。

我们调用`set`来设置一些请求头。

然后我们调用`expect`分别检查状态码和响应体。

数据将被填充到只在运行单元测试时使用的数据库中。

每次测试后，它们都会被重置。

这确保了我们有干净的数据来测试。

除了黑盒测试，我们还应该对服务等其他部分进行单元测试。

![](img/62d9f5c8c21915ac504a9eb702bfb807.png)

Photo by [niko photos](https://unsplash.com/@niko_photos?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该为我们的 API 遵循 RESTful 约定。

此外，测试对任何应用程序都很重要。

## 简单英语的 JavaScript

你知道我们有三份出版物和一个 YouTube 频道吗？在 [**寻找一切的链接 plainenglish.io**](https://plainenglish.io/) ！