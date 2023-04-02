# 解释了 5 个最常用的 Gatsby.js 插件

> 原文：<https://javascript.plainenglish.io/5-of-the-most-used-plugin-with-gatsby-js-explained-311d2eb854b3?source=collection_archive---------4----------------------->

## 盖茨比-图像，盖茨比-源文件系统，盖茨比-插件-反应-头盔，盖茨比-插件-夏普&盖茨比-插件-清单解释

你是新来的盖茨比外挂？你可能会想，什么是盖茨比插件？所以，这是一段可重用的代码，它可以节省您的时间！

> *Gatsby 插件是实现 Gatsby APIs 的 Node.js 包。对于更大、更复杂的站点，插件允许您将站点定制模块化到特定于站点的插件中。*
> 
> 出自[《盖茨比》doc](https://www.gatsbyjs.org/docs/plugins)

下载量超过 100 万的五个 Gatsby.js 插件是:

*   盖茨比形象
*   盖茨比源文件系统
*   盖茨比-插件-反应-头盔
*   盖茨比-插件-夏普
*   盖茨比插件清单

我们试着简单解释一下吧！

![](img/ed20ca31e73352c84f2925a73de880bf.png)

No, that’s not me (sadly) … Photo by [Frank Vessia](https://unsplash.com/@frankvex?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/explaining?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 🖼️一世:盖茨比形象

> 是一个 React 组件，专门用于与 Gatsby 的 GraphQL 查询无缝协作。它将 [Gatsby 的原生图像处理](https://image-processing.gatsbyjs.org/)能力与高级图像加载技术相结合，轻松全面地为您的站点优化图像加载。`gatsby-image`使用 [gatsby-plugin-sharp](https://www.gatsbyjs.org/packages/gatsby-plugin-sharp/) 为其图像转换提供动力。
> 
> 摘自[盖茨比 doc](https://www.gatsbyjs.org/packages/gatsby-image/)

简单地说，Gatsby-image 是一个插件，可以让你渲染你的图像。当我开始使用 Gatsby.js 时，它是我最纠结的插件之一，但现在，我对它的工作印象深刻。它不只是渲染你的图像，因为它:

> -将大图像调整到设计所需的大小。
> 
> -生成多个较小的图像，这样智能手机和平板电脑就不会下载桌面大小的图像。
> 
> -去除所有不必要的元数据，优化 JPEG 和 PNG 压缩。
> 
> -有效地延迟加载图像，以加速初始页面加载并节省带宽。
> 
> -使用“模糊”技术或“[跟踪占位符](https://github.com/gatsbyjs/gatsby/issues/2435)”SVG 来显示图像加载时的预览。
> 
> -保持图片的位置，这样当图片加载时你的页面不会跳转。
> 
> 摘自[盖茨比 doc](https://www.gatsbyjs.org/packages/gatsby-image/)

理解盖茨比形象的主要概念很重要，那就是**固定与流动。对于一个固定的图像，你将有一个确定的尺寸(宽度和高度)。你的形象将无法伸展。和流体的不一样。在流体的一个，你将有一个最大的宽度和高度，它将被设计成伸展，以适应容器。**

作为一个初学者，这个插件最让我困惑的一点就是片段的使用。在所有的教程和初学者中，我看到这样的例子:

```
{
  imageSharp {
    fluid(maxWidth: 200) {
      ...GatsbyImageSharpFluid
    }
  }
}
```

但是当我打开我的 GraphiQL 时，我看不到`GatsbyImageSharpFluid` 或`GatsbyImageSharpFixed`。那是一个片段，GraphiQL 不支持。

> GraphQL 包含了一个叫做“查询片段”的概念。顾名思义，它们是可以在多个查询中使用的查询的一部分。
> 
> 摘自[盖茨比 doc](https://www.gatsbyjs.org/packages/gatsby-image/)

在 GraphiQL 中，它相当于:

```
{
  imageSharp {
    fluid(maxWidth: 200) {
       base64
       aspectRatio
       src
       srcSet
       sizes
    }
  }
}
```

您应该注意到 Gatsby.js 有两个 per 依赖项:`gatsby-plugin-sharp`和`gatsby-transformer-sharp`。

Gatsby-image 有大量属性，您可以轻松添加。我鼓励你去看看它们！

## 🕵️‍♂️走得更远:

[](https://www.gatsbyjs.org/packages/gatsby-image/?=) [## 盖茨比形象

### 快速、优化的图像，无需工作。gatsby-image 是一个 React 组件，专门设计用于与…

www.gatsbyjs.org](https://www.gatsbyjs.org/packages/gatsby-image/?=) [](https://www.orangejellyfish.com/blog/a-comprehensive-guide-to-images-in-gatsby/) [## 《盖茨比》中的形象综合指南

### 正如 Kyle Gill 在他广受欢迎的“Gatsby.js 使图像优化变得容易”一文中所指出的，确保图像用在…

www.orangejellyfish.com](https://www.orangejellyfish.com/blog/a-comprehensive-guide-to-images-in-gatsby/) [](https://dev.to/alexabruck/understanding-gatsby-image-part-1-graphql-generated-files-markup-2ehd) [## 理解 gatsby-image(第 1 部分):graphql、生成的文件和标记

### 这是涵盖 Gatsby 插件 gatsby-image 的三部分系列的第 1 部分:第 1 部分:Graphql，生成的文件&…

开发到](https://dev.to/alexabruck/understanding-gatsby-image-part-1-graphql-generated-files-markup-2ehd) 

# 🗂️二世:盖茨比源文件系统

所以，让我们来谈谈 Gatsby-source-filesystem 插件。

> 一个 Gatsby 源代码插件，用于将数据从本地文件系统导入 Gatsby 应用程序。
> 
> 该插件从文件中创建`File`节点。
> 
> 摘自[盖茨比 doc](https://www.gatsbyjs.org/packages/gatsby-source-filesystem/?=)

所以基本上，如果你想在 Gatsby 中添加一个文件，比如一张图片或者一个 markdown，你将需要这个插件。换句话说，通过这个插件，我们告诉 GraphQL 在哪里查询我们的资产。

现在最难理解的是。每当您在一个新文件夹中有一个文件时，它都需要配置一个 Gatsby-source-filesystem。您可以配置插件的多个实例，例如:

```
// In your gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        name: `pages`,
        path: `${__dirname}/src/pages/`,
      },
    },
  ],
}
```

例如，在/static/assets 中有图像，在/content/blog 中有 markdown 文件。您需要在数组插件中放置两个配置对象。

## 🕵️‍♂️走得更远:

[](https://www.gatsbyjs.org/packages/gatsby-source-filesystem/?=) [## 盖茨比源文件系统

### 一个 Gatsby 源代码插件，用于将数据从本地文件系统导入 Gatsby 应用程序。该插件创建了…

www.gatsbyjs.org](https://www.gatsbyjs.org/packages/gatsby-source-filesystem/?=) [](https://www.freecodecamp.org/news/how-to-source-content-with-gatsby-js-c220dde97e7/) [## 如何使用 Gatsby.js 获取内容

### 作者 Dimitri Ivashchuk 如何使用 Gatsby.js 获取内容

www.freecodecamp.org](https://www.freecodecamp.org/news/how-to-source-content-with-gatsby-js-c220dde97e7/)  [## 【已解决】gatsby 问题:如何基于 gatsby-source-filesystem name 进行查询？

### @tsiq-swyx 我正在使用类似这样的东西将集合字段从文件传递到 markdown

救生员代码](https://lifesaver.codes/answer/question-how-do-i-query-based-on-gatsby-source-filesystem-name) 

# ⛑️三世:盖茨比插件反应头盔

> React 头盔是一个组件，让你控制你的文件头使用他们的 React 组件。
> 
> 使用这个插件，你可以在组件中添加属性，例如标题、元属性等。将被添加到 Gatsby 构建的静态 HTML 页面中。
> 
> 来自[盖茨比的文档](https://www.gatsbyjs.org/packages/gatsby-plugin-react-helmet/?=)

每个 HTML 页面都有两个主要部分:头部和主体。head 部分包含了所有的文档信息，比如它的标题，关键字，…使用 Gatsby-plugin-react-helmet 插件允许你访问你的文档的头，并改变它的值。这对于 SEO 来说非常有用。

## 🕵️‍♂️走得更远:

[](https://www.gatsbyjs.org/packages/gatsby-plugin-react-helmet/?=) [## 盖茨比-插件-反应-头盔

### 为使用 React 头盔添加的服务器渲染数据提供插件支持。反应头盔是一个组件，让你…

www.gatsbyjs.org](https://www.gatsbyjs.org/packages/gatsby-plugin-react-helmet/?=) [](https://medium.com/yellowcode/how-to-do-meta-tags-in-gatsbyjs-45245dc68ab9) [## 如何在 GatsbyJS 中做 Meta 标签

### 如何用一个新的 GastbyJS 网站设置你所有的 meta 标签

medium.com](https://medium.com/yellowcode/how-to-do-meta-tags-in-gatsbyjs-45245dc68ab9) 

# 🪒四世:盖茨比外挂夏普

你已经知道这个了，因为它是盖茨比形象的对等依赖。这意味着如果没有 gatsby-plugin-sharp，你就不能使用 gatsby-image。

> 它旨在为处理常见的 web 图像格式提供出色的现成设置。
> 
> 对于 JPEGs，它生成默认质量等级为 50 的渐进图像；
> 
> 对于 PNGs，它使用 [pngquant](https://github.com/pornel/pngquant) 来压缩图像。默认情况下，它使用[50–75]的质量设置。
> 
> 摘自[盖茨比 doc](https://www.gatsbyjs.org/packages/gatsby-plugin-sharp/?=)

这个插件允许你使用 [sharp node.js 包](https://www.npmjs.com/package/sharp)，这是一个图像优化库。所有那些在盖茨比形象中施展魔法的花哨的尺寸调整实际上都是夏普做的:

> 这种高速 Node.js 模块的典型用例是将常见格式的大图像转换为较小的、网页友好的不同尺寸的 JPEG、PNG 和 WebP 图像。
> 
> 从[锋利的 doc](https://sharp.pixelplumbing.com/)

这是一个重要的包裹。我们网站性能的公平份额取决于您的图像大小。这个插件将允许你进行优化。

## 🕵️‍♂️更进一步

[](https://www.gatsbyjs.org/packages/gatsby-plugin-sharp/?=) [## 盖茨比-插件-夏普

### 展示基于 Sharp 图像处理库构建的几个图像处理函数。这是一个低级的帮手…

www.gatsbyjs.org](https://www.gatsbyjs.org/packages/gatsby-plugin-sharp/?=)  [## sharp -高性能 Node.js 图像处理

### 将常用格式的大图像调整为不同尺寸的较小的、网页友好的 JPEG、PNG 和 WebP 图像

sharp.pixelplumbing.com](https://sharp.pixelplumbing.com/) 

# 📚最后一个&第五个:盖茨比插件清单

说句公道话，我从来没有做过 PWA(即使我真的想做)。所以，我和你同时发现了这个插件。

先从开头说起:**什么是 PWA？**

> 它们是常规网站，利用现代浏览器功能，通过类似应用程序的功能和优势来增强网络体验。
> 
> 摘自[盖茨比 doc](https://www.gatsbyjs.org/docs/add-a-manifest-file/)

这是一个普通的网页，但是如果你愿意，你可以在你的手机菜单上添加一个快捷方式。PWA 有 3 个技术要求:

*   安全环境(HTTPS)
*   服务人员
*   清单文件

顾名思义，gatsby-plugin-manifest 为您提供了 PWA 的清单文件。但是，清单文件是什么？

> 一个 [JSON](https://developer.mozilla.org/en-US/docs/Glossary/JSON) 文件，控制你的应用程序如何呈现给用户，并确保渐进式网络应用程序可被发现。它描述了应用程序的名称、开始 URL、图标以及将网站转换为类似应用程序的格式所需的所有其他细节。
> 
> 从 [MDN 文档](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps)

典型的清单包括以下内容:

*   应用程序名称
*   图标
*   网址

您可以通过插件配置来进行配置:

```
{
  plugins: [
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: "GatsbyJS",
        start_url: "/",
        icon: "src/images/icon.png", 
        // This path is relative to the root of the site.
      },
    },
  ]
}
```

PWA 不仅有这三种主要配置，还有大量选项。你可以在这里了解更多关于这个[的信息。](https://web.dev/add-manifest/#display)

> 这个插件提供了除清单配置之外的几个特性，使您的生活更加轻松，它们是:
> 
> -自动图标生成—从单一来源生成多种图标尺寸
> 
> [-图标支持](https://www.w3.org/2005/10/howto-favicon)
> 
> -传统图标支持(iOS) [1](https://www.gatsbyjs.org/packages/gatsby-plugin-manifest/?=#fn-1)
> 
> [-缓存破坏](https://www.keycdn.com/support/what-is-cache-busting)
> 
> -本地化—为基于路径的本地化提供唯一的清单( [Gatsby 示例](https://github.com/gatsbyjs/gatsby/tree/master/examples/using-i18n))
> 
> 来自[盖茨比的文档](https://www.gatsbyjs.org/packages/gatsby-plugin-manifest/?=)

## 🕵️‍♂️走得更远:

[](https://www.gatsbyjs.org/packages/gatsby-plugin-manifest/?=) [## 盖茨比插件清单

### 此插件支持的 web 应用清单(PWA 规范的一部分)允许用户将您的站点添加到他们的主页…

www.gatsbyjs.org](https://www.gatsbyjs.org/packages/gatsby-plugin-manifest/?=) [](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps) [## 渐进式网络应用(PWAs)

### 渐进式 web 应用程序是使用新兴 Web 浏览器 API 和功能以及传统渐进式…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps) [](https://web.dev/progressive-web-apps/) [## 渐进式网络应用

### 加入我们的 web.dev LIVE，从 6 月 30 日到 7 月 2 日的数字活动，学习现代网络技术。更多信息请访问…

网络开发](https://web.dev/progressive-web-apps/) 

👏如果你喜欢这篇文章，请随时让我关注更多类似的内容

📚还有，如果你想了解更多，可以看看那些文章:

[](https://medium.com/@mariequittelier/jamstack-how-to-do-a-contact-form-step-by-step-with-gatsby-js-netlify-and-mailgun-52d26432a5c4) [## 如何用 Gatsby.js，Netlify，Mailgun 一步步做一个联系表单

### 在这篇文章中，我将一步一步地向你展示如何用 Gatsby.js，Netlify 函数一步一步地做一个联系人表单…

medium.com](https://medium.com/@mariequittelier/jamstack-how-to-do-a-contact-form-step-by-step-with-gatsby-js-netlify-and-mailgun-52d26432a5c4) [](https://medium.com/@mariequittelier/how-to-connect-your-gatsby-js-landing-page-to-google-analytics-and-deploy-to-netlify-step-by-step-8352467583df) [## 如何将您的 Gatsby.js 登录页面连接到 Google Analytics 并逐步部署到 Netlify

### 使用谷歌分析很重要，因为它可以让你跟踪你的网站的使用情况。让我们来学习如何使用…进行设置

medium.com](https://medium.com/@mariequittelier/how-to-connect-your-gatsby-js-landing-page-to-google-analytics-and-deploy-to-netlify-step-by-step-8352467583df) [](https://medium.com/@mariequittelier/how-to-create-a-newsletter-with-mailchimp-gatsby-js-netlify-d48778d5c774) [## 如何用 Mailchimp，Gatsby.js & Netlify 创建时事通讯

### 在填写了联系表格并连接了 Google Analytics 之后，我想继续我的系列文章，讲述如何…

medium.com](https://medium.com/@mariequittelier/how-to-create-a-newsletter-with-mailchimp-gatsby-js-netlify-d48778d5c774)