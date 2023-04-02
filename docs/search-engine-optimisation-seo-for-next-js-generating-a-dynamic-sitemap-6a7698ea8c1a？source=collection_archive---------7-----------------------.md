# Next.js 的 SEO:生成动态站点地图

> 原文：<https://javascript.plainenglish.io/search-engine-optimisation-seo-for-next-js-generating-a-dynamic-sitemap-6a7698ea8c1a?source=collection_archive---------7----------------------->

![](img/bbe5b65e6e76c145406a7410011f6e6c.png)

Photo by [Merakist](https://unsplash.com/@merakist?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/seo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 什么是网站地图，为什么它对 SEO 有益？

Next.js 因其搜索引擎优化友好性和易于设置而在网络社区中广受欢迎。虽然 Next.js 提供了很多有益的特性，但是你可以为你的网站生成站点地图，并提交给各种搜索引擎。等等，什么是我们的网站地图？

网站地图是一个文件，它包含了你的网站中的页面、视频和其他资产文件的详细信息，以及它们之间的对应关系。网站地图有助于搜索引擎，如谷歌和雅虎，更好地理解你的网页之间的关系，从而使它们能够被更恰当地索引。

这是一个三页的网站示例文件的样子:

```
<?xml version="1.0" encoding="UTF-8"?>
**<urlset** **>
  <url>**
    **<loc>**http://www.example.net/**</loc>**
    **<lastmod>**2009-09-22**</lastmod>**
  **</url>
**  **<url>**
    **<loc>**http://www.example.net/about**</loc>**
    **<lastmod>**2009-09-22**</lastmod>**
  **</url>**
  **<url>**
    **<loc>**http://www.example.net/blog**</loc>**
    **<lastmod>**2009-09-22**</lastmod>**
  **</url>**
**</urlset>**
```

如果你有一个小网站，或者你的应用程序中的页面链接良好，你可能不需要网站地图。如果您的网页不经常更改，您甚至可以编写自己的静态 xml 文件。谷歌有一个[综合指南](https://support.google.com/webmasters/answer/156184?hl=en#:~:text=A%20sitemap%20is%20a%20file,more%20intelligently%20crawl%20your%20site.&text=A%20sitemap%20image%20entry%20can,matter%2C%20type%2C%20and%20license.)，详细说明了你个人用例的最佳前进方式。

如果你决定生成一个网站地图，并且你的网站是基于 Next.js 的，那么请继续阅读！

## 步骤 1:设置和项目结构

如果您还没有 Next.js 项目设置，您可以按照官方 Next.js [教程](https://nextjs.org/learn/basics/create-nextjs-app)上的说明设置一个启动器。

大多数 Next.js 网站在`pages`目录中有静态和动态生成的页面。为了读取这些页面名，我们将安装`globby`——一个可以遍历文件系统并返回路径名的 NPM 包。

```
yarn add --dev globby
```

这是我们的站点地图生成脚本唯一需要的外部依赖。

创建站点地图有两个部分。让我们来看一个标准的 Next.js 项目结构。

```
|- pages/
    |- about.js                 # about page
    |- blog/                    # blog folder
        |- index.js             # default blog 
        |- [id].js.             # dynamically generated blog posts|- components/
        |- MainComponent.jsx    # Main component
        |- OtherComponents.jsx  # Other component
```

首先，我们的脚本需要遍历文件系统并找到静态文件的名称(在本例中，是 about.js 和两个 index.js 文件)。其次，我们需要添加所有的动态页面([id])。js)到我们的网站地图。在这种情况下，遍历文件系统是行不通的，因为我们必须知道页面的所有 URL。我们将在步骤 3 中解决这个问题。首先，我们为所有静态页面生成一个站点地图。

## 步骤 2:编写生成站点地图的脚本

一旦我们有了基本的设置，我们现在可以写一个小脚本来生成一个站点地图。我已经把我的写在`utils/sitemap.js`里了，但是你可以随意选择一个文件夹！下面的脚本将把我们所有的静态文件转换成路径名，然后把它们转储到一个 xml 文件中，这个文件可以在我们的公共目录中被访问。

我在要点上添加了一些评论，进一步解释了正在发生的事情。

简而言之，我们使用`globby`从我们的目录中获取路径名(忽略我们的 api 路由和其他我们不想包含在站点地图中的页面)。然后，我们对这些进行映射，并为其中的每一个生成一个`<url><loc></url>`形式的条目。简单！

## 步骤 3:更新 next.config.js 以在构建时运行脚本

既然我们已经编写了初始脚本，我们需要更新 next.config.js 文件中的 webpack 配置，以便在构建时生成站点地图。确保根据您的项目结构更新您的文件路径！现在当你运行`yarn dev`或`npm run dev`时，你应该会在你的`public/`文件夹中得到一个`sitemap.xml`文件。

```
module.exports = {
  webpack: (config, { isServer }) => {
  if (isServer) {
    require('./src/utils/sitemap.js')
  }
  return config
  }
};
```

**步骤 4:处理动态页面(或通过外部 api 生成的页面)**

Next.js 的关键特性之一是能够生成动态页面。我们在原始脚本中没有考虑这些动态页面，但是添加这些页面非常简单。根据用于生成动态页面 URL 的 slugs 的来源，实现会有所不同。如果您的页面是从外部 api 或 CMS(例如，Contentful)中获取的，那么您必须先获取这些页面的[id]参数，然后才能为它们生成站点地图。

您可以将它们存储在数据库中，直接从 CMS 获取它们，或者在 json 文件中有一个页面名称列表——但是想法是一样的，查询这个列表并映射名称以在 sitemap 文件中形成一个`<loc>`实例。轻松点。

对于这个例子，我们将使用一个外部 api。在 https://jsonplaceholder.typicode.com/有一个很好的模拟数据 api。我们将查询假帖子，并使用这些帖子中的 id 来生成我们的页面。

最终的代码应该看起来像这样，并附有解释。

简而言之，我们从外部来源获取了一个样本帖子列表，并映射它们以生成它们各自的 URL 路径。现在，如果你运行`npm run dev`或`yarn dev`，你应该得到一个新的文件`public/sitemap.xml`，它将保存你的站点地图的内容。

一旦您部署了您的网站，您的站点地图将在`https://YOUR_URL/sitemap.xml`变得可访问，并且您可以使用您的唯一链接将您的站点地图提交给各种搜索引擎(例如，在 Google 的搜索控制台中)。

这就是所有的人！我们已经为静态编写和动态生成的页面创建了一个站点地图！现在，无论您何时添加新页面，无论是静态编写的还是动态生成的，并且触发了新的构建，您的站点地图都会自动更新，搜索引擎将能够有效地抓取它们。编码快乐！