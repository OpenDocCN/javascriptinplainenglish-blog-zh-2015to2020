# Nuxt 入门

> 原文：<https://javascript.plainenglish.io/getting-started-with-nuxt-4652bc83ddc6?source=collection_archive---------7----------------------->

## Nuxtjs 及其架构入门指南

![](img/afca586f6186b934132abced44edbb3b.png)

Photo by [Miguel Á. Padriñán](https://www.pexels.com/@padrinan?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/paper-boats-on-solid-surface-194094/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

Nuxtjs 是一个完全建立在 VueJS 之上的开源框架。

*"Nuxt 直观的 Vue 框架使用 NuxtJS 满怀信心地构建您的下一个 Vue.js 应用程序。一个开源框架，让 web 开发变得简单而强大。”*

Nuxtjs 捆绑了一些令人惊叹的技术，例如在服务器端渲染应用程序和单页面应用程序之间进行选择。

# **创建一个 Nuxt 应用**

创建一个新的 Nuxt 应用程序。打开您的开发者终端，根据您首选的包管理器使用下面的命令。

**NPM**

```
npm init nuxt-app getting-started-with-nuxt
```

该命令将创建一个名为 getting-started-with-nuxt 的 Nuxt 应用程序。你可以用你想要的标题来改变它。

**NPX**

```
npx create-nuxt-app getting-started-with-nuxt
```

该命令将创建一个名为 getting-started-with-nuxt 的 Nuxt 应用程序。你可以用你想要的标题来改变它。

**纱线**

```
yarn create nuxt-app getting-started-with-nuxt
```

该命令将创建一个名为 getting-started-with-nuxt 的 Nuxt 应用程序。你可以用你想要的标题来改变它。

这个命令将创建一个新的 Nuxt 应用程序。现在您需要将您的目录更改为新创建的 Nuxt 应用程序，并在您的终端上运行下面的命令。

**NPM**

```
npm run dev
```

**纱线**

```
yarn dev
```

该命令将传输我们的 Nuxt 应用程序文件，并将其提供给 **localhost:3000。**

Nuxt 还将在每次保存后重启并服务我们的应用程序，并在每次我们进行更改和保存时热重装我们的应用程序。

# **Nuxt 文件夹结构**

**资产:**这个目录包含您的未编译资产，比如 LESS、SASS 或 JavaScript。这包括媒体和图像等其他资产。

组件:组件目录包含了你的 Vue.js 组件。

**中间件:**这个目录包含了你的应用中间件。中间件允许您定义自定义函数，这些函数可以在呈现一个页面或一组页面之前运行。

这个目录包含了你的应用程序布局。

**页面:**该目录包含路由组件。它还包含我们的应用程序视图和路线。

**插件:**这个目录包含在挂载根 Vue.js 应用程序之前要运行的 JavaScript 插件

这个目录将包含你的静态文件。

**存储:**该目录将包含您的 Vuex 存储文件。

# Nuxt 的优势

Nuxt 还捆绑了许多令人惊叹的技术和优势。nuxt 的一些主要优点是。

**-通用/服务器端渲染。**

Nuxt 为您的应用程序提供了在通用渲染和服务器端渲染之间进行选择的定制。

**- SEO 优化。**

Nuxt 是为 SEO 有效性配置的，所以你不必担心你的应用程序，更像 WordPress。

- **通过 vue-meta 整合 meta 标签管理**

除了这些好处之外，Nuxt 还为您的应用程序集成了元标签。

**-简单页面路由(基于文件结构/页面的路由)。**

Nuxt 配置了 vue-router，因此您处理路由的时间更少。它提供了包含所有路由元件的页面目录。

**-选择包含 UI 框架。**

Nuxt 提供了包含你喜欢的 UI 框架的能力，比如 bootstrap、tailwind、Vuetify 和其他著名的 UI 框架。

**-选择包含后端框架。**

**-没有要维护的 Webpack 配置。**

**-自动代码分割、Babel transpile 和 Sass 集成。**

**-模块化架构可扩展。**

**-使用**“*nuxt 生成*”轻松构建应用程序。

# **构建生产就绪的 Nuxt 应用**

Nuxt 提供了一个简单的应用程序构建，使用命令“ *nuxt generate* ”来构建一个生产就绪的应用程序。它将创建一个 dist 文件夹，这是一个已经为部署做好准备的优化应用程序。

**NPM**

```
npm run generate
```

**纱线**

```
yarn generate
```

js 为我们提供了在服务器或静态部署之间的选择。

# 服务器部署

为了部署一个 SSR 应用程序，我们使用`target: server`，其中 server 是默认值。

## **结论**

Nuxt 提供了一个简单而强大的应用程序架构。

感谢您阅读这篇文章，如果您发现它有帮助，请不要犹豫与他人分享。

**资源和参考**

*   [Nuxt 网站](https://nuxtjs.org/)