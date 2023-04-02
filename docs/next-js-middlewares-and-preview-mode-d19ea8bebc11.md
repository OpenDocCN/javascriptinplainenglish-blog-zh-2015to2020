# Next.js —中间件和预览模式

> 原文：<https://javascript.plainenglish.io/next-js-middlewares-and-preview-mode-d19ea8bebc11?source=collection_archive---------11----------------------->

![](img/e4774ed5f78c9de9a6e7ed4a6ad78c18.png)

Photo by [Mitchel Lensink](https://unsplash.com/@lensinkmitchel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们可以轻松地创建服务器端呈现的 React 应用程序和静态站点。

在这篇文章中，我们将看看 route 中间件和 Next.js 的预览模式。

# 连接/快速中间件支持

我们可以使用 Connect 兼容的中间件。

例如，我们可以通过安装中间件并添加到我们的 Next.js 应用程序来添加中间件。

要安装它，我们运行:

```
npm i cors
```

或者

```
yarn add cors
```

然后，我们可以通过编写以下代码将其添加到我们的 API 路径中:

```
import Cors from 'cors'const cors = Cors({
  methods: ['GET', 'HEAD'],
})function runMiddleware(req, res, fn) {
  return new Promise((resolve, reject) => {
    fn(req, res, (result) => {
      if (result instanceof Error) {
        return reject(result)
      }
      return resolve(result)
    })
  })
}async function handler(req, res) {
  await runMiddleware(req, res, cors)
  res.json({ message: 'hello world' })
}export default handler
```

我们导入了`Cors`函数。

然后我们用它来创建`cors`中间件。

我们可以指定可用于路由的请求方法。

`runMiddleware`函数通过调用`fn`中间件函数来运行中间件，如果成功则解析到`result`。

否则，承诺被拒绝。

`handler`是运行中间件并返回响应的路由处理器。

# 响应助手

Next.js 附带了各种响应助手。

`res.status(code)`方法让我们设置响应的状态代码。

`code`是一个 HTTP 状态码。

方法让我们发送一个 JSON 响应。

`json`是有效的 JSON 对象。

`res.send(body)`让我们发送一个 HTTP 响应，其中`body`是一个字符串、对象或缓冲区。

`res.redirect`使用可选的`status`和`path`让我们重定向到特定的路径或 URL。

`status`的默认值为 307。

例如，我们可以这样称呼它们:

```
export default (req, res) => {
  res.status(200).json({ name: 'hello' })
}
```

它们可以用链子拴在一起。

# 预览模式

我们可以预览静态生成的路由。

Next.js 有一个预览模式的特性来解决这个问题。

它将在请求时而不是构建时呈现页面，并获取草稿内容而不是已发布的内容。

为了使用它，我们调用`res.setPreviewData`方法进行预览。

例如，我们可以写:

```
export default (req, res) => {
  res.setPreviewData({})
  res.end('Preview mode')
}
```

启用它。

# 安全地访问数据

我们可以通过从预览路径写入并发送到我们的页面来访问数据。

为此，我们通过创建`pages/api/preview.js`文件来创建预览 API 路径:

```
export default async (req, res) => {
  const response = await fetch(`https://yesno.wtf/api`)
  const data = await response.json();
  res.setPreviewData(data)
  res.redirect('/post')
}
```

我们从 API 获取数据并调用`setPreviewData`，这样我们就可以从页面 JavaScript 文件中的`getStaticProps`函数获取数据。

然后我们重定向到我们的文件，这样我们就可以在我们的页面上看到数据。

在`pages/post.js`中，我们写道:

```
function Post({ data }) {
  return (
    <div>{data && data.answer}</div>
  )
}export async function getStaticProps(context) {
  if (context.preview) {
    return {
      props: {
        data: context.previewData
      }
    }
  }
  return { props: { data: { answer: 'not preview' } } }
}export default Post
```

我们通过检查`context.preview`属性来检查预览模式是否打开。

如果是`true`，那么我们从`context.previewData`那里得到预览数据。

然后我们在我们的`Post`组件中显示`data`。

# 结论

我们可以用 Next.js 添加预览模式，这样我们就可以在它上线之前查看输出。

此外，我们可以在 Next.js 应用程序中使用 Connect 或 Express 中间件。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**