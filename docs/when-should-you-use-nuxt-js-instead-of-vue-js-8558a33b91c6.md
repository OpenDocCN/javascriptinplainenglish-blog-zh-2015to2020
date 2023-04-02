# 什么时候应该用 Nuxt.js 而不是 Vue.js？

> 原文：<https://javascript.plainenglish.io/when-should-you-use-nuxt-js-instead-of-vue-js-8558a33b91c6?source=collection_archive---------0----------------------->

## 什么时候用 Nuxt，什么时候用 Vue

![](img/c160a1ec882d7687768ecac512a1e6fd.png)

***Vue.js*** 以构建用户界面和单页应用而闻名。它促进了“高度解耦”,这允许开发者容易地创建用户界面和快速原型。 ***Nuxt.js*** 是一个构建在 Vue 之上的框架，目的是让开发变得简单而强大。它侧重于开发人员的体验。

## Nuxt.js 提供了哪些 Vue 没有的东西？

Nuxt.js 使开发人员能够构建*服务器端呈现的*应用程序，其中 Node.js 服务器将基于您的 Vue 组件向客户端交付 HTML(而不是在客户端运行 JavaScript)。这将允许比使用 Vue 建立的传统温泉更好的 SEO。Nuxt 的另一大优势是能够为您的应用程序创建一个静态生成的网站。这使得用户无需服务器就能发布他们的应用程序(比如 GitHub Pages)。然而，如果你愿意的话，你仍然可以用 Nuxt 建造一个 SPA 来利用它优于 vanilla Vue 的优势。

## 什么时候用 Nuxt？

如果需要**服务器端渲染**或者**静态站点生成**就使用 Nuxt。

使用 Nuxt，您可以在`nuxt.config.js`文件的 head 属性中为您的应用程序定义默认的`<meta>`标签。这允许你添加一个默认的标题和描述标签，如果 SEO 很重要的话，Nuxt 是一个完美的选择。您还可以通过在视图的`<script>`标签中添加 head 属性来定义单个页面的标题和描述。这对于非 SPA 的应用程序非常有用。

使用 Nuxt 的另一个原因是，如果您不想手动配置应用程序的路由。默认情况下，Nuxt 会根据文件夹结构自动生成路由。例如，如果您为像`about/index.vue`这样的 about 页面创建一个视图，Nuxt 将自动为它创建路线`/about`。如果您创建另一个视图`user/_id/index.vue`，动态路线`/user/{id}`将根据用户的 id 为用户创建。

如果您需要认证(比如认证路由)，Nuxt 通过其中间件功能使之变得容易。您可以定义在呈现页面或布局之前运行的自定义函数。为此，首先在`/middleware`文件夹中定义一个自定义函数。要使用该函数，请在页面的`middleware`属性中指定它的名称。

Nuxt 是高度可配置的，以满足您的需求。安装和开始使用 Nuxt 也很简单:

```
npm install nuxt
npm init nuxt-app <project-name>
```

## 什么时候用 Vue？

如果你需要一个不复杂的**单页应用**就使用 Vue。就这么简单。如果 SEO 不重要，或者手动配置路线很重要，那么 vanilla Vue 是更好的选择。仅仅为了利用 SPA 的自动路由生成而使用 Nuxt 是不值得的。

如果你需要 TypeScript 支持，那么坚持使用 Vue。最近发布的 Vue 3 是用编写的，支持 TypeScript。就目前情况来看，NuxtJS 对类型脚本的支持很差。

## 我应该从 vanilla Vue 迁移到 Nuxt 吗？

不，不是真的。如果您的应用程序已经有了很大的代码库，那么迁移到使用 Nuxt 是不值得的。如果您的应用程序还不太成熟，那么如果您真的认为 Nuxt 提供的好处会有显著的优势。将 Nuxt 与 Vue 结合使用仍然可以达到同样的效果。

## 结论

最终，选择将取决于你试图建立什么。每种选择都有其优点。一般来说，如果你需要 SSR/SSG 或者 SEO 很重要，就用 Nuxt。如果你正在建立一个 SPA 或者需要 TypeScript 支持，请坚持使用 Vue。