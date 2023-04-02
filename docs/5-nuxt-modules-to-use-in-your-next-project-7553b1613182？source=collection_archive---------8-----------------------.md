# 在您的下一个项目中使用的 5 个 Nuxt 模块

> 原文：<https://javascript.plainenglish.io/5-nuxt-modules-to-use-in-your-next-project-7553b1613182?source=collection_archive---------8----------------------->

![](img/6291ffdf9ad3285e0027478026a14b64.png)

# 介绍

Nuxt 团队和社区最近发布了一个扩展的 [Nuxt 模块浏览器](https://modules.nuxtjs.org/)，它允许你通过流行度、类型和 Github 星级来过滤 Nuxt 模块。

直到我看到这个浏览器，我才知道 Nuxt 模块生态系统已经变得如此强大。我能够找到一些我在这个项目和其他项目中使用过的真正有用的模块。

这个列表可能相当令人生畏(以一种令人敬畏的方式)，所以我想我应该把它归结为五个对我有巨大帮助的。我推荐的绝不仅仅是这些 ***和*** ，只有五个我认为很棒的！

TL:DR:给我看看模块！

1.  [nuxt/content](https://content.nuxtjs.org/)
2.  [nuxt/顺风](https://tailwindcss.nuxtjs.org/)
3.  [nuxt/彩色模式](https://color-mode.nuxtjs.org/)
4.  [nuxt/cloudinary](https://cloudinary.nuxtjs.org/)
5.  [nuxt/feed](https://www.npmjs.com/package/@nuxtjs/feed/)

# [1。Nuxt/Content](https://content.nuxtjs.org/)

当建立这个网站时，我知道我需要找到一个无头 CMS 来管理我的内容。我想在 Markdown 中创作我的内容，插入 Vue 组件，并能够提交给版本控制。Nuxt 内容有 ***所有这些*** ，还有更多！

我发现的一些最好的特性是:

*   使用 YAML 前沿物质将任何变量注入文章的能力
*   要使用的自动注射`createdAt`、`updatedAt`和`toc`(目录)变量
*   “双击”直接在页面上编辑，并立即看到反映的更改
*   能够将 Vue 组件直接插入到您的降价中

本模块还有更多内容，但我怎么推荐都不为过！

# [2。nuxt/顺风](https://tailwindcss.nuxtjs.org/)

如果你从事前端 web 开发，你可能听说过 [TailwindCSS](https://tailwindcss.com/) 。如果你不熟悉的话，这是一个“实用第一”的 CSS 框架，有大量的定制和周到的默认设置。如果你像我一样，你知道一旦你尝试了，你就不能回头。

这个模块可以非常容易地让你的 Nuxt 项目启动并运行 Tailwind，并允许你直接在你的应用中引用 Tailwind 配置。它与**黑暗模式**和我们列表中的下一个模块配合得非常好…

# [3。nuxt/彩色模式](https://color-mode.nuxtjs.org/)

这个模块使得在用户第一次访问时检测用户偏好的配色方案，或者切换并保存他们的选择以供后续页面访问变得非常容易。它和 **Nuxt/Tailwind** 配合的也很好。

您可以直接在您的模板或组件中读取`$colorMode.preference`，并根据他们当前的偏好呈现不同的内容，更改他们的颜色模式偏好就像调用这样的`toggle`函数一样简单:

```
<template>
  <button @click="toggleColorMode">Toggle Color Mode</button>
</template>

<script>
export default {
  methods: {
    toggleColorMode() {
      this.$colorMode.preference = this.$colorMode.value == "light" ? "dark" : "light";
    }
  }
}
</script>
```

# [4。Nuxt/Cloudinary](https://cloudinary.nuxtjs.org/)

提高网站性能的最简单的方法之一是通过图像优化。Nuxt/Cloudinary 让这个 ***变得微不足道*** 。

[Cloundinary](https://cloudinary.com/) 是一个管理图像和视频内容的解决方案(有非常慷慨的免费计划)。该服务提供了通过强大的转换动态优化图像的能力，甚至能够向图像添加文本覆盖。

[玛雅·沙文](https://twitter.com/MayaShavin)维护着这个神奇的模块，感觉几乎像是魔术。该模块将一个 **Cloudinary 实例** ($cloudinary)注入到您的 Nuxt 项目中，您可以使用它来做一些真正强大的事情。

您可以获取存储在 Cloudinary 实例中的图像或视频，并执行转换

```
const url = this.$cloudinary.image.url('sample', { crop: 'scale', width: 200 })
```

您甚至可以获取远程图像并执行完全相同的转换

```
const url = this.$cloudinary.image
              .fetchRemote(
                'https://icatcare.org/app/uploads/2018/07/Thinking-of-getting-a-cat.png',
                { crop: 'scale', width: 200 })
```

该模块还提供了一些现成的 Vue 组件，你可以在你的模板中的任何地方使用它们。这真的很神奇！

# [5。Nuxt/feed](https://www.npmjs.com/package/@nuxtjs/feed)

这个模块帮助你从你的网站内容中生成一个 RSS，Atom 或者 JSON feed！

你可能在想，人们还在使用 RSS 提要吗？Chris Coyier(CSS Tricks&CodePen 的创始人)就知道！

> 谁会因为你的个人博客有 RSS 订阅而去阅读它呢？我要看你的个人博客，因为它有 RSS 订阅。[](https://t.co/mtcyKhEVet)
> 
> **-克里斯·科伊尔(@ Chris Coyier)*[*2020 年 1 月 7 日*](https://twitter.com/chriscoyier/status/1214606808125341696?ref_src=twsrc%5Etfw)*

*对我来说，这很棒的主要原因是，如果你决定交叉发布到像 [Dev.to](https://dev.to/) 这样的平台，你实际上可以提供你的 RSS 提要 URL，让你的所有内容成为草稿，你可以立即发布。众所周知，最初发布在您网站上的交叉发布内容可以为您的 SEO 排名创造奇迹(前提是您为您的内容定义了一个合适的规范 URL)。*

*另外，如果您正在使用 Nuxt 内容，您可以使用官方文档中描述的一些方法[生成一个提要](https://content.nuxtjs.org/integrations)！*

# *包扎*

*就像我之前说的，有 ***吨*** 的模块很惊艳，这只是我真正喜欢的五个。*

*在这篇博客写作的时候，目前有 146 个 Nuxt 模块可以在[modules.nuxtjs.org](https://modules.nuxtjs.org/)上使用(还有更多模块需要合并到网站中)。去看看，找到一些听起来有趣或有帮助的东西。*

*尽情探索 Nuxt 模块的精彩世界吧！*

*感谢阅读。*

**原载于 2020 年 10 月 29 日*[*https://David parks . dev*](https://davidparks.dev/blog/5-nuxt-modules-to-use-in-your-next-project/)*。**