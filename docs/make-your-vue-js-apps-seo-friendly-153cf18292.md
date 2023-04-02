# 尽情享受吧。Js 应用程序搜索引擎优化友好

> 原文：<https://javascript.plainenglish.io/make-your-vue-js-apps-seo-friendly-153cf18292?source=collection_archive---------12----------------------->

## T2:我只需要几分钟！

![](img/ed11dde9f565ff59404a1f4f32fc0d11.png)

Photo by [Markus Winkler](https://unsplash.com/@markuswinkler?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是我最喜欢的框架之一，如果不是最喜欢的话。

这很容易理解，学习曲线一点也不陡峭，不像 React 和 Angular，它是一个重量约为 30kb 的轻型包。

然而，它仍然是一个单页应用程序(SPA)。尽管如果你使用 SPA，你仍然可以使用它来生成静态站点，但是你会失去 web 开发中一个重要的东西，即 SEO。

谷歌爬虫已经改进了多年，JavaScript 应用程序也是如此，然而，开发者仍然需要采取一些额外的步骤来完全符合 SEO，并使应用程序可以被有机地发现。

如果任何像爬虫或“有机发现”这样的术语听起来不熟悉，那么你应该检查我的搜索引擎优化(SEO)指南。

[](https://medium.com/javascript-in-plain-english/a-beginners-guide-to-seo-for-javascript-web-applications-c67d55728291) [## JavaScript Web 应用程序 SEO 初学者指南

### 在建立了多个网站后，以下是我对有机流量和 SEO 的了解

medium.com](https://medium.com/javascript-in-plain-english/a-beginners-guide-to-seo-for-javascript-web-applications-c67d55728291) 

在这篇文章中，我们将通过 [Vue-Meta](https://www.npmjs.com/package/vue-meta) 库让 Vue.js 应用变得更加 SEO 友好。

## 什么是 Vue-Meta？

Vue Meta 是一个 [Vue.js](https://vuejs.org/) 插件，允许你管理你的应用的元数据。您只需使用`metaInfo`属性将其作为组件数据的一部分导出。

它倾向于提供与 React 的 [React 头盔](https://www.npmjs.com/package/react-helmet)包相同的功能。

许多框架已经在使用这个包了。其中一些是:

1.  网格体
2.  Nuxt.js
3.  因子 JS
4.  里姆

## 装置

通过使用 NPM，您可以快速开始使用该软件包。

NPM 使得 JavaScript 开发者共享和重用代码变得容易。如果你还没有安装 NPM，可以查看[官方文档](https://www.npmjs.com/get-npm)开始安装。

一旦 NPM 启动并运行，运行下面的命令来安装 Vue-Meta 包。

```
npm install vue-meta --save
```

你也可以通过 CDN 安装包。更多信息请点击此处[。](https://www.npmjs.com/package/vue-meta#installation)

我们需要将包导入到主 JavaScript 文件中。

```
import Vue from "vue";
import App from "./App.vue";
import Meta from "vue-meta";Vue.use(VueMeta, {
*// optional pluginOptions* refreshOnceOnNavigation**:** true
})new Vue(
{
  render: h => h(App)
}
).$mount("#app");
```

## 定制插件并添加元数据

Vue 元包允许添加和动态修改几个元标签。

然而，为了简单起见，我们将首先看到如何添加标题标签。

```
<template>
  <div>Hello 2021!</div>
</template>

<script>
export default {
  name: 'Hello',
  metaInfo: {
    title: 'Goodbye 2020'
  }
}
</script>
```

如您所见，我们可以将一个对象传递到 Vue-Meta 库给我们的`metaInfo`属性中。

此外，我们可以设置默认标题。此标题将显示在没有任何标题的网站上。

在根组件中，添加标题和标题模板，以确保子组件遵循统一的模板，如下所示:

```
export default {
  name: 'App',
  metaInfo: {
    title: 'My Default Title',
    titleTemplate: '%s | November 2020'
  },

}
```

## 其他选项

除了标题元标签，我们还可以添加其他相关标签，以使我们的 Vue 应用程序更好地被索引，也有利于整体体验。

您可以设置一些最重要的标记，如描述和视口。

```
export default {
  name: 'App',
  metaInfo: {
    title: 'Default Title',

    meta: [
      { charset: 'utf-8' },
      { name: 'description', content: 'Thanksgiving website' },
      { name: 'viewport', content: 'width=device-width, initial-  scale=1' }
    ]
  },
  ...
}
```

然而这还不是全部。您甚至可以添加外部脚本。

```
export default {
....
  metaInfo() {
    return {
      title: "Goodbye 2020",
      meta: [
       ....
   script: [
        { src: 'your link here', async: true, body: true }
      ],
   link: [
        {
          rel: 'canonical',
          href: '....'
        }
   ]
    };
  }};
```

## VMID

是一个特殊的属性，Vue-Meta 允许我们使用它来控制组件树。

它基本上做的是，如果两个元数据有相同的`vmid`，那么子树将覆盖父树。这确保了它们不会合并，而是使用最后一个子元素的值。

```
{
  metaInfo: {
    meta: [
      { vmid: 'desc',
        name: 'description',
        content: 'This is great!' }
    ]
  }
}
```

## 结论

开发好的应用程序需要时间，最糟糕的事情之一就是没有足够的吸引力。

管理动态生成的 JavaScript 应用程序和网站中的 SEO 是相当棘手的，但是有几个软件包可以帮助这个过程变得稍微简单和直接。

其中一个包是 Vue-Meta for Vue.js Apps，它允许你添加元标签来提高整体 SEO 分数。

大多数 JavaScript 框架都有这样的包，但是，要想在 JavaScript 上获得最好的 SEO，明智的做法是使用服务器端呈现。