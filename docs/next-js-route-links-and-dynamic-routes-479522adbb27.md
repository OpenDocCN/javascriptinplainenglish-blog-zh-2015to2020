# Next.js 路径链接和动态路径

> 原文：<https://javascript.plainenglish.io/next-js-route-links-and-dynamic-routes-479522adbb27?source=collection_archive---------2----------------------->

![](img/f2d5059e1e218ed5a78ec6a499c6b5e9.png)

Photo by [JJ Ying](https://unsplash.com/@jjying?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们可以轻松地创建服务器端呈现的 React 应用程序和静态站点。

在本文中，我们将看看使用 Next.js 的路由。

# 动态路由链接

我们可以为动态路由添加链接。

为此，我们写道:

`pages/foo.js`

```
import Links from './links';function Foo() {
  return <div>
    <Links />
    <p>foo</p>
  </div>
}export default Foo
```

`pages/blog/[slug].js`

```
import { useRouter } from 'next/router'
import Links from '../links';const Post = () => {
  const router = useRouter()
  const { slug } = router.query return <div>
    <Links />
    <p>Post: {slug}</p>
  </div>
}export default Post
```

`pages/links.js`

```
import Link from 'next/link'const posts = [
  { slug: 'hello-world', title: 'hello world', id: 1 },
  { slug: 'good-news', title: 'good news', id: 2 },
  { slug: 'great-news', title: 'great news', id: 3 },
]function Links() {
  return (
    <ul>
      <li>
        <Link href="/foo">
          <a>foo</a>
        </Link>
      </li>
      {posts.map((post) => (
        <li key={post.id}>
          <Link href="/blog/[slug]" as={`/blog/${post.slug}`}>
            <a>{post.title}</a>
          </Link>
        </li>
      ))}
    </ul>
  )
}export default Links
```

我们有一个`[slug].js`文件，它显示了我们传入的 slug。

`router.query`对象中有 URL 参数。

它可以有和我们传入的一样多的 URL 参数。

`useRouter`钩子有路线数据。

我们需要括号来表示该文件用于可以接受 URL 参数的动态路由。

`links.js`文件包含链接。

我们使用`Link`组件来添加链接。

`href`具有路由的 URL 模式。

并且`as`具有到 fo 的路由的实际路径。

此外，我们有一个到`foo`页面的静态链接。

因此，当我们单击链接时，我们将转到映射到路线的页面。

当我们点击最后 3 个链接时，我们应该会看到这个小块。

# 捕捉所有路线

我们可以通过在文件名前添加`...`来添加无遗漏路线。

例如，我们可以写:

`pages/blog/[...slug].js:`

```
import { useRouter } from 'next/router'
import Links from '../links';const Post = () => {
  const router = useRouter()
  const { slug } = router.query return <div>
    <Links />
    <p>Post: {slug.join(', ')}</p>
  </div>
}export default Post
```

`pages/links.js`

```
import Link from 'next/link'const posts = [
  { slug: 'hello-world/1', title: 'hello world', id: 1 },
  { slug: 'good-news/2', title: 'good news', id: 2 },
  { slug: 'great-news/3', title: 'great news', id: 3 },
]function Links() {
  return (
    <ul>
      <li>
        <Link href="/foo">
          <a>foo</a>
        </Link>
      </li>
      {posts.map((post) => (
        <li key={post.id}>
          <Link href="/blog/[slug]" as={`/blog/${post.slug}`}>
            <a>{post.title}</a>
          </Link>
        </li>
      ))}
    </ul>
  )
}export default Links
```

我们为每个`slug`添加了一个额外的 URL 参数。

它们在`[...slug].js`中被渲染成数组。

因此，我们调用`join`将数组转换成逗号分隔的字符串。

# 可选的全包路线

我们可以通过在文件名周围添加额外的括号来使全包路由可选。

例如，我们可以写:

`pages/blog/[[...slug]].js`

```
import { useRouter } from 'next/router'
import Links from '../links';const Post = () => {
  const router = useRouter()
  const { slug } = router.query return <div>
    <Links />
    <p>Post: {slug && slug.join(', ')}</p>
  </div>
}export default Post
```

我们在文件名周围加了一组括号。

现在我们可以转到`/blog`路径，我们将看到在`Post:`之后没有任何内容的同一个页面。

# 结论

我们可以用 Next.js 添加动态路由链接。

我们所要做的就是遵循文件名约定，使用带有`Link`组件的`href`和`as`道具。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**