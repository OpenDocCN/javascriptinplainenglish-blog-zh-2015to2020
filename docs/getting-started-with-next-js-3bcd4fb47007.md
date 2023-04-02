# Next.js 入门

> 原文：<https://javascript.plainenglish.io/getting-started-with-next-js-3bcd4fb47007?source=collection_archive---------13----------------------->

![](img/6e6a452b7461e80c78ab7c9020c00267.png)

Photo by [Alex Blăjan](https://unsplash.com/@alexb?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们可以轻松地创建服务器端呈现的 React 应用程序和静态站点。

在本文中，我们将了解如何开始使用 Next.js。

# 入门指南

我们可以用提供的命令行程序创建 Next.js 项目。

要创建项目，我们只需运行:

```
npx create-next-app
```

或者:

```
yarn create next-app
```

来创建我们的项目。

然后，为了启动开发服务器，我们运行:

```
npm run dev
```

然后我们去 [http://localhost:3000/](http://localhost:3000/) 和我们的 app。

现在我们可以在`pages`文件夹中创建一个组件来添加我们的页面。

我们进入`pages`文件夹，创建一个`hello.js`文件并添加:

```
function Hello() {
  return <div>hello world</div>
}export default Hello
```

敬它。

我们必须记住导出组件，这样它就会被渲染。

然后我们可以去[http://localhost:3000/hello](http://localhost:3000/hello)看看我们的页面。

Next.js 通过文件名自动路由，所以我们不必担心这个问题。

# 带有动态路线的页面

如果我们想创建带有动态路由的页面，我们只需将它们放在文件夹结构中，URL 将遵循相同的结构。

例如，我们可以创建一个`posts`文件夹并创建我们的文件。

我们在`pages/posts`中创建`1.js`并写道:

```
function Hello() {
  return <div>hello 1</div>
}export default Hello
```

同样，我们在同一个文件夹中创建`2.js`,并编写:

```
function Hello() {
  return <div>hello 2</div>
}export default Hello
```

那么当我们转到 http://localhost:3000/posts/1 和[http://localhost:3000/posts/2、](http://localhost:3000/posts/2,)的时候就会分别看到`hello 1`和`hello 2`。

# 预渲染

默认情况下，Next.js 会预先呈现每个页面。

每个页面的 HTML 都是预先创建的。

这比在客户端渲染更有利于性能和 SEO。

这些页面只附带它需要加载的 JavaScript，以便它加载得更快。

有两种预渲染形式。

一个是静态生成，一个是服务器端渲染。

静态生成意味着 HTML 在构建时生成，并在每个请求中重用。

因为它重用页面，这将是呈现页面的最快方式，所以这是推荐的选项。

另一个选项是服务器端呈现，它在每个请求中呈现 HTML。

我们可以在两种类型的渲染中进行选择。

# 数据静态生成

我们可以用数据静态生成页面。

为此，我们可以写:

`pages/yesno.js`

```
function YesNo({ data }) {
  return <p>{data.answer}</p>
}export async function getStaticProps() {
  const res = await fetch('https://yesno.wtf/api')
  const data = await res.json()
  return {
    props: {
      data,
    },
  }
}export default YesNo
```

我们用`YesNo`组件创建了一个新的组件文件。

`getStaticProps`函数从 API 异步获取数据。

然后我们通过返回一个在`props`属性中有`data`的对象来返回解析值。

然后我们可以从`YesNo`组件中的道具获得数据。

在 JSX，我们展示了数据。

# 结论

我们可以使用 Next.js 创建页面，它可以很容易地显示静态内容或来自 API 的内容。

喜欢这篇文章吗？如果是这样，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**