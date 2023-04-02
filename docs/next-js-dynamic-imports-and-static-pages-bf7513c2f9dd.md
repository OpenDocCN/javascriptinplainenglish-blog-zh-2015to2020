# Next.js —动态导入和静态页面

> 原文：<https://javascript.plainenglish.io/next-js-dynamic-imports-and-static-pages-bf7513c2f9dd?source=collection_archive---------9----------------------->

![](img/0f6bc58464a3e294be3de428d534c92f.png)

Photo by [Yogendra Singh](https://unsplash.com/@yogendras31?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们可以轻松地创建服务器端呈现的 React 应用程序和静态站点。

在本文中，我们将了解动态导入，并使用 Next.js 呈现静态页面。

# 动态导入

ES2020 本身支持动态导入。

我们可以在 Next.js 中使用它来动态导入组件。

它也适用于服务器端渲染。

这让我们可以将代码分割成易于管理的块。

例如，我们可以写:

`components/name.js`

```
function Name() {
  return (
    <span>world</span>
  )
}export default Name
```

`pages/hello.js`

```
import dynamic from 'next/dynamic'const NameComponent = dynamic(() => import('../components/name'))function Hello() {
  return (
    <div>hello <NameComponent /></div>
  )
}export default Hello
```

我们导入了`dynamic`函数，并传入了一个调用`import`来导入组件的回调函数。

然后我们可以在代码中使用`NameComponent`。

现在我们应该看到:

```
hello world
```

已显示。

# 具有命名导出的动态组件

我们可以创建带有命名导出的动态组件。

为此，我们可以写:

`components/name.js`

```
export function Name({ }) {
  return (
    <span>world</span>
  )
}
```

`pages/hello.js`

```
import dynamic from 'next/dynamic'const NameComponent = dynamic(() => import('../components/Name').then((mod) => mod.Name)
)function Hello() {
  return (
    <div>hello <NameComponent /></div>
  )
}export default Hello
```

在回调中，我们用`then`回调来调用`import`以从模块中返回`Name`组件。

# 带有自定义加载组件

我们还可以添加一个组件，在组件加载时显示该组件。

例如，我们可以写:

`components/name.js`

```
function Name() {
  return (
    <span>world</span>
  )
}
export default Name
```

`pages/hello.js`

```
import dynamic from 'next/dynamic'const NameComponent = dynamic(() => import('../components/Name'), { loading: () => <p>loading</p> })function Hello() {
  return (
    <div>hello <NameComponent /></div>
  )
}export default Hello
```

我们在代码中有一个带有`loading`属性的对象，它的值是一个组件。

# 禁用服务器端呈现

通过将一个对象传递到属性设置为`false`的`dynamic`函数中，可以禁用服务器端呈现。

例如，我们可以写:

`components/name.js`

```
function Name() {
  return (
    <span>world</span>
  )
}
export default Name
```

`pages/hello.js`

```
import dynamic from 'next/dynamic'const NameComponent = dynamic(() => import('../components/Name'), { ssr: false }
)function Hello() {
  return (
    <div>hello <NameComponent /></div>
  )
}export default Hello
```

# 静态 HTML 导出

我们可以用`next export`命令将我们的应用程序导出到静态 HTML。

它的工作原理是将所有页面预先呈现为 HTML。

页面可以导出一个`getStaticPaths`属性，返回我们想要预呈现的所有路径。

`next export`应在我们的 app 中没有动态数据变化时使用。

为了使用它，我们运行:

```
next build && next export
```

`next build`开发应用程序，`next export`为我们提供 HTML 页面。

我们也可以将这两个命令添加到我们的`package.json`文件中:

```
"scripts": {
  "build": "next build && next export"
}
```

并运行`npm run build`来运行它。

`getInitialProps`不能在任何页面上与`getStaticProps`或`getStaticPaths`一起使用。

# 结论

我们可以用 Next.js 动态导入组件。

此外，我们可以将 Next.js 应用程序构建为静态 HTML 文件。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**