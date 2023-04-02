# Next.js — CSS-in-JS、快速刷新和环境变量

> 原文：<https://javascript.plainenglish.io/next-js-css-in-js-fast-refresh-and-environment-variables-94f505612eeb?source=collection_archive---------2----------------------->

![](img/9e3529c584a9be2a1c96be119ec6badf.png)

Photo by [John O'Nolan](https://unsplash.com/@johnonolan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们可以轻松地创建服务器端呈现的 React 应用程序和静态站点。

在本文中，我们将了解如何使用 Next.js 设计页面样式、使用快速刷新以及添加环境变量。

# 更少的手写笔支持

Next.js 支持 Less 和 Stylus 进行造型。

为了增加对它的支持，我们安装了 [@zeit/next-less](https://github.com/vercel/next-plugins/tree/master/packages/next-less) 和 [@zeit/next-stylus](https://github.com/vercel/next-plugins/tree/master/packages/next-stylus) 插件。

# CSS-in-JS

我们还可以使用任何 CSS-in-JS 解决方案来设计 Next.js 应用程序。

例如，我们可以添加内嵌样式:

```
function Hello() {
  return <p style={{ color: 'blue' }}>hello world</p>
}export default Hello
```

我们还可以使用 styled-jsx 将 CSS 放在 JavaScript 文件中。

Next.js 自带的。

例如，我们可以写:

```
function Hello() {
  return <div>
    <style jsx>{`
        p {
          color: blue;
        }
        div {
          background: red;
        }
        @media (max-width: 600px) {
          div {
            background: blue;
          }
        }
      `}</style>
    <style global jsx>{`
        body {
          background: lightgray;
        }
      `}</style>
    <p>hello world</p>
  </div>
}export default Hello
```

我们添加了包含局部和全局样式的样式标签。

全球风格有`global`道具，而本地风格没有。

# 快速刷新

快速刷新是 Next.js 的一个特性，它使开发变得更加容易。

它为我们提供了对 React 组件所做编辑的即时反馈。

在下一个 9.4 或更高版本中，默认情况下启用该功能。

此功能使编辑可见，而不会丢失组件状态。

如果我们编辑一个组件，它将重新加载组件。

如果我们编辑了不是组件但被他们使用的东西，那么两个文件都将被刷新。

如果文件是由刷新树之外的文件导入的，那么快速刷新将进行完全重新加载。

当遇到任何运行时错误时，我们会看到一个显示错误的覆盖图。

如果我们在应用程序中添加了错误边界，那么他们会在下一次编辑时重试渲染。

如果有钩子，那么当快速刷新发生时，它们的状态将被更新。

# 静态文件服务

Next.js 应用程序可以有静态文件。

我们只需将它们放在`public`文件夹中。

例如，我们可以写:

```
function Hello() {
  return <div>
    <img src="/kitten.jpg" alt="kitten" />
  </div>
}export default Hello
```

鉴于`kitten.jpg`在公共文件夹中。

我们会看到展示的小猫图片。

我们不应该更改`public`文件夹的名称，因为它是唯一可以用于服务静态资产的文件夹。

# 环境变量

我们可以从一个`.env.local`文件中加载环境变量。

例如，我们可以通过编写以下内容来创建一个`.env.local`文件:

```
API_KEY=...
```

然后我们可以创建一个使用`API_KEY`环境变量的页面:

```
function Photos({ data }) {
  return <p>{JSON.stringify(data)}</p>
}export async function getServerSideProps() {
  const headers = new Headers();
  headers.append('Authorization', process.env.API_KEY);
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

我们用`process.env`对象访问我们的环境变量。

这些环境变量仅在 Node.js 环境中可用。

# 结论

Next.js 9.4 或更高版本带有快速刷新特性，使开发更加方便。

此外，Next.js 附带了 CSS-in-JS 支持。

我们也可以在应用程序中使用环境变量。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**