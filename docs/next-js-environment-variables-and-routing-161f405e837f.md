# Next.js 环境变量和路由

> 原文：<https://javascript.plainenglish.io/next-js-environment-variables-and-routing-161f405e837f?source=collection_archive---------9----------------------->

![](img/111c109c8f75b170bde561c9723c9e16.png)

Photo by [Matt Duncan](https://unsplash.com/@foxxmd?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们可以轻松地创建服务器端呈现的 React 应用程序和静态站点。

在本文中，我们将了解如何向浏览器和路由公开环境变量。

# 向浏览器公开环境变量

如果我们给变量加上前缀`NEXT_PUBLIC_`,就可以向浏览器公开我们的环境变量。

例如，我们可以写:

```
NEXT_PUBLIC_API_KEY=...
```

然后我们可以通过写来使用它:

```
function Photos({ data }) {
  return <p>{JSON.stringify(data)}</p>
}export async function getServerSideProps() {
  const headers = new Headers();
  headers.append('Authorization', process.env.NEXT_PUBLIC_API_KEY);
  const res = await fetch(`https://api.pexels.com/v1/search?query=people`, {
    method: 'GET',
    headers,
    mode: 'cors',
    cache: 'default',
  })
  const data = await res.json()
  return { props: { data } }
}export default Photos
```

我们从`process.env`属性中访问环境变量。

# 默认环境变量

我们可以在`.env`、`.env.development`和`.env.production`文件中添加默认的环境变量。

`.env.local`文件将覆盖这些文件中的值。

并且`.env*.local`文件应该被添加到`.gitignore`中，因为这是我们应该存储秘密的地方。

# Vercel 上的环境变量

我们还可以在 [Vercel](https://vercel.com/) 的环境变量部分存储环境变量。

我们还是用`.env`、`.env.development`和`.env.production`来存储默认值。

这些值可以通过运行以下命令拉入`.env.local`:

```
vercel env pull .env.local
```

# 测试环境变量

同样，我们可以使用`test`环境来存储任何测试环境的环境变量。

这对于设置运行自动化测试的环境很有用。

如果`NODE_ENV`设置为`test`，将使用测试值。

当应用程序在`test`环境中运行时，不会加载`.env.local`。

# 支持的浏览器和功能

Next.js 支持 IE11 和所有现代浏览器，如 Chrome、Firefox 和 Edge。

Next.js 为 IE11 兼容性注入 polyfills。

所以，我们不用担心 IE11 上的功能缺失。

服务器端聚合填充也被注入。

这样，我们可以在服务器端环境中使用`fetch`,而无需添加我们自己的 polyfills。

Next.js 确保了现代 JavaScript 特性，如:

*   异步/等待(ES2017)
*   对象静止/扩散属性(ES2018)
*   动态导入()(ES2020)
*   可选链接(ES2020)
*   无效合并(ES2020)
*   类字段和静态属性

一切都是现成的。

# 按指定路线发送

使用 Next.js 自动完成路由

`pages`文件夹的文件夹结构决定了所定义的路线。

例如，如果我们将`page/index.js`映射到`/`。

并且`pages/posts/index.js`被映射到`/posts`。

我们可以嵌套我们的文件，它们将被映射到带有文件路径的 URL。

# 动态路段

我们用括号将文件名括起来，使它们具有动态性。

例如，如果我们有`pages/blog/[slug].js`，那么它被映射到`/blog/:slug` URL。

如果我们有`pages/post/[…all].js`，那么在`/post`之后我们可以有任意数量的 URL 参数。

比如会搭配`/post/2020/id/title`。

# 页面之间的链接

我们可以用`Link`组件在页面之间添加链接。

例如，我们可以写:

`pages/links.js`

```
import Link from 'next/link'function Links() {
  return (
    <ul>
      <li>
        <Link href="/foo">
          <a>foo</a>
        </Link>
      </li>
      <li>
        <Link href="/bar">
          <a>bar</a>
        </Link>
      </li>
    </ul>
  )
}export default Links
```

`pages/foo.js`

```
import Links from './links';function Foo() {
  return <div>
    <Links />
    <p>foo</p>
  </div>
}export default Foo
```

`pages/bar.js`

```
import Links from './links';function Bar() {
  return <div>
    <Links />
    <p>bar</p>
  </div>
}
export default Bar
```

使用`Link`组件添加`Links`组件，以将链接添加到页面。

我们使用`Foo`和`Bar`组件中的`Links`来添加路线。

现在我们可以点击它们在路线之间跳转。

# 结论

我们可以通过 Next.js 在浏览器环境中使用环境变量。

此外，我们可以使用`Link`组件向页面添加链接。

路由是按照一些惯例完成的。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**