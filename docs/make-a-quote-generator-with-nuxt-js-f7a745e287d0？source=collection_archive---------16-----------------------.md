# 使用 Nuxt.js 创建报价生成器

> 原文：<https://javascript.plainenglish.io/make-a-quote-generator-with-nuxt-js-f7a745e287d0?source=collection_archive---------16----------------------->

## 如何用 Nuxt.js 编写报价生成器的分步指南

![](img/fb13a1aa8189a6fa665c8f9326d41998.png)

Photo by [Christopher Gower](https://unsplash.com/@cgower?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在本文中，我们将使用 Nuxtjs 制作一个简单的随机报价生成器。

如果您不熟悉 Nuxtjs，请阅读下面的文章，它将帮助您快速入门。

[](https://medium.com/javascript-in-plain-english/getting-started-with-nuxt-4652bc83ddc6) [## Nuxt 入门

### Nuxtjs 及其架构入门指南

medium.com](https://medium.com/javascript-in-plain-english/getting-started-with-nuxt-4652bc83ddc6) 

首先，我们要安装一个新的 Nuxtjs 项目

## **Npm**

```
npm init nuxt-app quote-generator
```

## **纱**

```
yarn create nuxt-app quote-generator
```

安装完成后，打开您的开发人员终端并运行下面的命令。

```
npm run dev 
```

这个命令将我们的应用程序服务到浏览器中，并在我们进行更改时重新热加载它。

**给发电机组件报价**

导航到组件的目录并创建一个文件名为 ***Quotes.vue.***

我们现在将在这里编写报价生成器组件。

我们还需要安装 axios 来发出 **API** 请求。要安装 axios，请运行下面的命令。

```
npm install axios -D
```

该命令将把 axios 包安装到我们的应用程序中

现在让我们编写报价生成器。

我们将使用免费的 API 来获取报价[https://type.fit/api/quotes](https://type.fit/api/quotes)

现在，在我们的 Quotes.vue 组件中，我们将包含下面的代码

```
<template><div class="quotes"><div><h1>"{{ quotes.text }}"</h1><p>{{ quotes.author }}</p>*<!-- prevent default loading of the page -->*<button type="submit" @click.prevent="getQuotes">Get Quotes</button></div></div></template>
```

在按钮上，您可以看到我们附加了一个单击事件，它将侦听按钮上的任何单击事件。我们还用. prevent 指令将它链接起来。

这将防止点击按钮时页面冒泡和重新加载。

不，我们需要添加功能，将返回一个从 API 随机报价。

在 Inside out 组件中，我们将有一个名为 ***getQuotes*** 的方法，它将在每次点击按钮时发出一个 API 请求。

```
<script>*import* axios *from* "axios";*export* *default* {name: "Quotes",data() {*return* {quotes: {},};},methods: {async getQuotes() {const res = *await* axios("https://type.fit/api/quotes");const data = *await* res.data;*//   get a random quote from the returned data*let randQuote = data[Math.floor(Math.random() * data.length)];*//   store the info to the data instance*this.quotes = randQuote;},},};</script>
```

首先，我们将导入我们安装的 axios 包，并使用它来发出 API 请求。

在我们的数据中，我们有一个 quotes 对象，它将存储来自 API 的报价。

在我们的方法中，我们有一个****get quotes***函数，它将进行异步调用并返回一个随机报价。*

*我们还通过在 API 调用中添加 async await 来使请求异步。*

## ***获取随机报价***

```
**//   get a random quote from the returned data*let randQuote = data[Math.floor(Math.random() * data.length)];*//   store the info to the data instance*this.quotes = randQuote;*
```

*将报价组件导入 index.vue 页面*

*现在，由于我们的 Quotes.vue 是一个单独的组件，我们需要将其导入到我们的页面(index.vue)*

*在我们的 index.vue 中，我们导入报价组件，如下所示。*

```
**// lazy loading of the quotes component*const Quotes = () => import("@/components/Quotes");*export* *default* {components: {Quotes,},};*
```

*我们正在缓慢加载报价组件。要了解更多关于延迟加载组件的信息，请查看下面的文章。*

*[](https://medium.com/javascript-in-plain-english/lazy-loading-components-in-vue-js-478c9f300b71) [## 在 Vue.js 中延迟加载组件

### 通过组件的延迟加载提高 Vue.js 应用程序的性能。

medium.com](https://medium.com/javascript-in-plain-english/lazy-loading-components-in-vue-js-478c9f300b71) 

我还准备了一个简单的视频，向你展示我们如何做这个简单的项目。

你可以在下面查看。

## **结论**

我们已经看到了如何在 Nuxtjs 中制作一个简单的报价生成器。

我们还学习了如何:

*   使用 axios 进行 API 调用
*   组件的延迟加载

感谢您通读这篇文章，如果您觉得它有帮助，请不要犹豫，分享出来。*