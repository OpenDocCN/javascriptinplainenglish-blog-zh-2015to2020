# Next.js —客户端导航和 API 路线

> 原文：<https://javascript.plainenglish.io/next-js-client-side-navigation-and-api-routes-e535eabff1f9?source=collection_archive---------4----------------------->

![](img/c9afbd46cb121ba7cbadcc20619aedb9.png)

Photo by [Sebastian Herrmann](https://unsplash.com/@officestock?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们可以轻松地创建服务器端呈现的 React 应用程序和静态站点。

在本文中，我们将看看使用 Next.js 的路由。

# 客户端导航

除了在服务器端进行路由，我们还可以使用 Next.js 进行客户端导航。

例如，我们可以写:

`pages/links.js`

```
import { useRouter } from 'next/router'function Links() {
  const router = useRouter() return (
    <ul>
      <li>
        <span onClick={() => router.push('/foo')}>foo</span>
      </li>
      <li>
        <span onClick={() => router.push('/bar')}>bar</span>
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

我们有跨度为的`pages/links.js`文件，它在点击时运行`router.push`方法。

`router`对象来自 Next.js 的`useRouter`钩子。

# 浅层布线

浅层路由让我们无需再次运行类似`getServerSideProps`、`getStaticProps`和`getInitialProps`的数据获取方法就可以更改 URL。

我们通过`router`对象获得更新后的`pathname`和`query`。

例如，我们可以写:

`pages/count.js`

```
import { useEffect } from 'react'
import { useRouter } from 'next/router'function Count() {
  const router = useRouter() useEffect(() => {
    router.push('/count?count=10', undefined, { shallow: true })
  }, []) useEffect(() => {
  }, [router.query.count]) return <p>{router.query.count}</p>
}export default Count
```

添加一个带有`useEffect`钩子的`Count`组件，该钩子调用`router.push`向 URL 添加一个查询字符串。

`shallow: true`表示我们启用浅层路由。

我们应该看到显示 10，因为当我们转到[http://localhost:3000/count](http://localhost:3000/count)时，从查询字符串中可以看到`router.query.count`是 10。

浅层路由仅适用于相同页面的 URL 更改。

所以如果我们有:

```
router.push('/?count=10', '/about?count=10', { shallow: true })
```

然后从零开始加载`about`页面。

# API 路线

我们可以使用 API routes 用 Next.js 构建一个 API。

为此，我们可以将 JavaScrtipt 文件放在`pages/api`文件夹中。

然后里面的文件将被映射到`/api/*`的网址。

例如，我们可以写:

`pages/api/hello.js`

```
export default (req, res) => {
  res.statusCode = 200
  res.json({ name: 'hello world' })
}
```

`req`有请求数据，而`res`是一个我们可以用来创建响应的对象。

我们用`res.statusCode`设置响应状态代码，并用`res.json`方法创建我们的 JSON 响应。

现在，当我们用任何 http 动词向[HTTP://localhost:3000/API/hello](http://localhost:3000/api/hello)发出请求时，我们会看到:

```
{
  "name": "hello world"
}
```

已退回。

为了处理不同的 HTTP 方法，在 API 路由中，我们可以使用`req.method`属性。

例如，我们可以写:

```
export default (req, res) => {
  res.statusCode = 200
  res.json({ name: 'hello world', method: req.method })
}
```

现在，当我们向[http://localhost:3000/API/hello](http://localhost:3000/api/hello)发出 PUT 请求时，我们得到:

```
{
    "name": "hello world",
    "method": "PUT"
}
```

已退回。

API 路由不指定 CORS 头，所以我们只能在请求来自与 API 路由相同的来源时发出这些请求。

# 结论

我们可以用 Next.js 做 API 路由。

使用`useRouter`钩子可以从客户端进行导航。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**