# 建立你的盖茨比网站的 6 个技巧

> 原文：<https://javascript.plainenglish.io/6-tips-on-building-your-gatsbyjs-website-64203212aa4?source=collection_archive---------4----------------------->

![](img/891dde5ec7ccc1248bd1299c470ccacb.png)

到目前为止，我已经建立了 2 个个人网站。第一个是 4 年前我如何自学网络开发的基础知识。这是一个简单的[自举](https://getbootstrap.com/)网站，远远谈不上像样。我也有一个独立的 [Ghost](https://ghost.org/) 博客网站，但当我试图定制它时，它变得一团糟。在多年巩固我的技能后，我去寻找一个更好的替代方案，它快速、可定制且易于维护。于是，我找到了[盖茨比](https://www.gatsbyjs.com/)，建立了我的第二个[个人网站](https://sugarfabby.com)。整个经历真的很棒。

在学习如何使用盖茨比的过程中，我参考了来自[韦斯·博斯](https://wesbos.com/)和[肯特·c·多兹的](https://kentcdodds.com/)盖茨比网站。看到他们如何将盖茨比发挥到极致，真是令人惊讶。

如果你想开始或改进你的盖茨比项目，这里有一些我认为有用的建议。

# 1.带 React 头盔的 SEO

您可以构建一个可重用的 SEO 组件，并用不同页面中的 props 覆盖默认元数据。我有一个默认的搜索引擎优化组件，它有基本的 HTML 标题，脸书 OpenGraph 和 Twitter 元数据，代表一般的页面。然后，我会在我的博客帖子页面中用 Markdown frontmatter 数据覆盖它们，这样它们就可以被单独索引和共享。

# 2.在 gatsby-browser 和 gatsby-ssr 中包装您的布局

如果您有一个希望应用于所有页面的通用页面设计，您可以创建一个布局组件并将其添加到`gatsby-browser`和`gatsby-ssr`文件中，如下所示:

```
exports.wrapPageElement = ({ element, props }) => (
   <Layout {...props}>{element}</Layout>
);
```

使用`wrapPageElement` API，Gatsby 会在构建页面时自动应用布局。同样值得一提的是，布局将保持挂载状态。我在玩动画的时候遇到了卸载状态的问题，这可以通过使用这个方法来解决。如果你的内容是静态的，你自己把布局应用到每个页面上也是完全没问题的。

# 3.挖掘盖茨比·阿皮斯的超能力

我花了相当长的时间才理解盖茨比·阿皮斯的力量。最常见的用例是自动化一些构建过程。例如，我可以在`createPages` API 中查询所有的 markdown 文件，并调用`createPage`动作来构建带有路径、模板的页面，并添加我想要的任何上下文数据。这就是我如何产生我所有的博客帖子。本质上，你可以有不同的页面模板来构建你的播客、研讨会、书评或任何你想要的页面。

`onCreateNode`也是另一个有用的 API，它增加了额外的 GraphQL 节点字段。例如，我添加了一个`editLink`字段，它是 GitHub 中的文章编辑链接，我可以在我的博客文章模板页面中查询它。

您可以在这里探索所有的 API[并为自己找到一个用例。](https://www.gatsbyjs.com/docs/api-reference/)

# 4.使用 Gatsby 图像插件

一开始，我只是简单地用`img`标签来标记我所有的图片。就响应性而言，这不是一个好的解决方案。对我来说，创建不同大小的图像并使用`picture`标签也很耗时。

这就是`gatsby-image`的用武之地。它完成上述所有过程，甚至提供延迟加载。您还可以在模糊和 SVG 描摹效果之间进行选择，以增加渐进式加载用户体验。它与提供图像的不同 CMS 插件一起工作。

# 5.您可能不需要集成 CMS

这显然取决于你的网站的性质。如果有其他人在管理内容，集成像 Contentful，Wordpress 或 Drupal 这样的 CMS 将是最好的。

如果你只是简单的建立和管理你自己的网站，你不需要 CMS。使用像`gatsby-source-filesystem`这样的社区插件，从本地文件中获取资源比你想象的要简单。例如，我在本地用 MDX 写博客。发布和编辑都是通过将提交推送到我的存储库来完成的。这样你就可以完全控制内容，而不需要依赖其他服务。

# 6.如果您在 Netlify 上托管，请激活增量构建

将 Gatsby 网站部署到 Netlify 可能会花费大量时间，因为它需要一个接一个地生成所有页面。如果有很多图像，构建时间会更长。

我建议按照这个[指南](https://www.netlify.com/blog/2020/04/23/enable-gatsby-incremental-builds-on-netlify/)在 Netlify 中设置增量构建。它使用 Gatsby 缓存插件来帮助在构建之间保持缓存，并且只重建已经更改的文件。这极大地改善了网站的构建时间。

**感谢阅读！**

请随意查看我网站上的回购，看看这些技巧在起作用，希望你会发现它们很有用。如果你有任何问题，我很乐意帮忙。

[](https://github.com/fabianlee1211/sugarfabby.com) [## fabianlee1211/sugarfabby.com

### 我的个人作品集和博客网站 disrupt GitHub 是超过 5000 万开发者的家园，他们一起工作来托管…

github.com](https://github.com/fabianlee1211/sugarfabby.com)