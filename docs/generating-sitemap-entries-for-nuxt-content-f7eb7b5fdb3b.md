# 为 Nuxt 内容生成站点地图条目

> 原文：<https://javascript.plainenglish.io/generating-sitemap-entries-for-nuxt-content-f7eb7b5fdb3b?source=collection_archive---------1----------------------->

## 为 Nuxt 站点生成站点地图时，不会自动包含内容模块中的文件。让我们看看如何解决这个问题！

![](img/d98b95f903f7c793366e1912685a4540.png)

在启动我的新网站([https://jackwhiting.co.uk](https://jackwhiting.co.uk))时，我选择使用 [Nuxt Content](https://content.nuxtjs.org/) 来管理我的博客帖子、作品和任何其他内容。在生成站点地图的时候，我注意到从`@nuxt/content`创建的任何内容都没有被添加到我的站点地图中，我需要修复这个问题。在这篇文章中，我们将看看如何解决这个问题，并让您的所有条目列出来。

# 设置

在我们做任何事情之前，我们需要确保我们已经安装了`[@nuxtjs/sitemap](https://www.npmjs.com/package/@nuxtjs/sitemap)`模块。在终端中打开您的项目并运行以下代码。

```
yarn add @nuxtjs/sitemap
```

在你的`nuxt.config.js`中将网站地图添加到你的模块中。

```
export default {
  modules: ['@nuxt/content', '@nuxtjs/sitemap']
}
```

这应该总是在你已经包括的任何其他模块之后，以确保所有的路由都被捕获。

接下来，为站点地图配置添加一些默认值——添加一个`hostname`并设置一个空函数，我们稍后将使用该函数来获取和返回我们内容的路径。

```
export default {
  sitemap: {
    hostname: process.env.BASE_URL || 'YOURSITEURL',
    routes: async () => {}
  }
}
```

关于站点地图选项的完整列表，你可以查看[自述文件](https://www.npmjs.com/package/@nuxtjs/sitemap)。

# 添加您的路线

您设置内容结构的方式可能是独特的，您可能使用独特的 URL，您可能有多个文件夹或只想包含一个文件夹。这些都可能改变你定义路线的方式，所以我在下面提供了几个例子，让你可以开始设置和运行。

# 从一个目录添加路由

```
routes: async () => {
  const { $content } = require('@nuxt/content')

  const posts = await $content('posts')
    .only(['path'])
    .fetch()

  return posts.map((p) => p.path)
}
```

# 从多个目录添加路由

```
routes: async () => {
  const { $content } = require('@nuxt/content')

  const posts = await $content('posts')
    .only(['path'])
    .fetch()
  const works = await $content('works')
    .only(['path'])
    .fetch()

  *// Map and concatenate the routes and return the array.*
  return []
    .concat(...works.map((w) => w.path))
    .concat(...posts.map((p) => p.path))
}
```

# 用更多信息扩展路线

有时你可能想在你的站点地图上添加`lastmod`、`priority`或其他细节——我们可以通过环绕它们并添加额外的属性来扩展我们包含的路线。

```
routes: async () => {
  const { $content } = require('@nuxt/content')

  const posts = await $content('posts').fetch()
  const works = await $content('works').fetch()

  *// Setup an empty array we will push to.*
  const routes = []

  *// Add an entry for the item including lastmod and priorty*
  works.forEach((w) =>
    routes.push({
      url: w.path,
      priority: 0.8,
      lastmod: w.updatedAt
    })
  )

  posts.forEach((p) =>
    routes.push({
      url: p.path,
      priority: 0.8,
      lastmod: p.updatedAt
    })
  )

  *// return all routes*
  return routes
}
```

# 测试一切

将您的路线添加到`nuxt.config.js`文件后，您可以在终端中运行`yarn dev`。一旦开始编译，在浏览器中访问 http://localhost:3000/sitemap . XML，您应该会看到列出的所有内容条目。

如果你不想每次运行`yarn dev`时都编译`sitemap.xml`，你可以将函数包装在一个条件中。

```
routes: async () => {
  if (process.env.NODE_ENV !== 'production') return

  *// rest of the function here*
}
```

希望这篇文章对你有帮助。如果您发现内容有任何问题，请随时在 Twitter ( [@jackabox](https://twitter.com/jackabox) )上联系我。

*原载于 2020 年 8 月 20 日*[*【https://jackwhiting.co.uk】*](https://jackwhiting.co.uk/posts/generating-sitemap-entries-for-nuxt-content/)*。*