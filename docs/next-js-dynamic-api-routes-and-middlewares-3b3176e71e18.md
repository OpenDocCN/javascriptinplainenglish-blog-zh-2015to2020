# Next.js —动态 API 路由和中间件

> 原文：<https://javascript.plainenglish.io/next-js-dynamic-api-routes-and-middlewares-3b3176e71e18?source=collection_archive---------9----------------------->

![](img/712be991f956c56038e4b96ac1f90c05.png)

Photo by [Ricky Kharawala](https://unsplash.com/@sweetmangostudios?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们可以轻松地创建服务器端呈现的 React 应用程序和静态站点。

在本文中，我们将通过 Next.js 了解动态 API 路由和中间件。

# 动态 API 路由

我们可以通过遵循通常的文件命名约定来创建动态 API 路由。

在`pages/api`文件夹中，我们可以创建一个文件名用括号括起来的文件。

例如，我们可以创建一个`pages/api/post/[pid].js`并编写:

```
export default (req, res) => {
  const {
    query: { pid },
  } = req
  res.end(`Post: ${pid}`)
}
```

然后我们可以去[http://localhost:3000/API/post/foo](http://localhost:3000/api/post/foo)进行请求。

结果我们会显示出`Post: foo`。

我们可以用通用的休息模式来设置路线。

例如，我们可以使用`GET api/posts/`来获取帖子列表。

而`GET api/posts/1`得到了一个单独的职位。

# 包罗万象的 API 路线

我们可以通过在文件名中使用`...`来创建一个 catch all API 路由。

例如，我们可以创建`pages/api/post/[...slugs].js`文件并编写:

```
export default (req, res) => {
  const {
    query: { slugs },
  } = req
  res.end(`Post: ${slugs.join(', ')}`)
}
```

然后当我们转到[http://localhost:3000/API/post/foo/bar](http://localhost:3000/api/post/foo/bar)时，我们得到:

```
Post: foo, bar
```

已退回。

# 可选的一揽子 API 路线

此外，我们可以创建无所不包的 API 路由，这些路由并不总是期望传入 URL 参数。

为此，我们用两个方括号将文件名括起来。

所以我们可以创建一个名为`pages/api/post/[[...slugs]].js`的文件，并添加:

```
export default (req, res) => {
  const {
    query: { slugs },
  } = req
  res.end(`Post: ${Array.isArray(slugs) && slugs.join(', ')}`)
}
```

我们有一个文件名用方括号括起来的文件。

在代码中，我们检查`slugs`是否是一个数组，然后返回连接在一起的数组条目。

# API 中间件

API roiutes 提供内置的中间件来解析传入的请求。

它们包括:

*   `req.cookies`解析 cookies
*   `req.query`解析查询字符串
*   `req.body`解析请求体。

# 自定义配置

每个 API 路由都可以导出一个`config`对象来更改默认配置。

例如，我们可以写:

```
export default (req, res) => {
  res.end(`Post: ${req.body}`)
}export const config = {
  api: {
    bodyParser: {
      sizeLimit: '1mb',
    },
  },
}
```

创建对请求体施加大小限制的 API 路由。

此外，我们可以使用以下命令禁用正文解析:

```
export default (req, res) => {
  res.end(`Post: ${req.body}`)
}export const config = {
  api: {
    bodyParser: false,
  },
}
```

我们将`bodyParser`设置为`false`来禁用主体解析器。

`externalResolver`属性是一个标志，它告诉服务器该路由正由外部解析器处理。

它会对未解决的请求禁用警告。

我们可以通过书写来使用它:

```
export default (req, res) => {
  res.end(`Post: ${req.body}`)
}export const config = {
  api: {
    externalResolver: true,
  },
}
```

# 结论

我们可以用 Next.js 添加动态路由和路由中间件。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**