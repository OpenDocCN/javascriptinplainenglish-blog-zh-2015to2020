# 🚀将任何应用程序部署到 GitHub 页面

> 原文：<https://javascript.plainenglish.io/deploying-any-app-to-github-pages-1e8e946bf890?source=collection_archive---------1----------------------->

![](img/4aae73d52e8088a07c5f7cf1f25ded88.png)

Photo by [SpaceX](https://unsplash.com/@spacex?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/nasa?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

[GitHub Pages](https://pages.github.com/) 是你的网站持有者&你的项目。您可以直接从 GitHub repo 托管您的代码。本文将帮助您如何在`master`分支中管理您的应用程序，并轻松地在`gh-pages`分支中部署代码。

可以选择 [React](https://reactjs.org/) 、 [Vue](https://vuejs.org/) 、 [Gatsby](http://gatsbyjs.com/) 、 [Next](https://nextjs.org/) 、 [Nuxt](https://nuxtjs.org/) 、 [Gridsome](https://gridsome.org/) 等任意前端框架，在 master 分支中构建 app，使用`npm run build`命令构建代码，主机直接使用`gh-pages`分支。

将你的应用程序放到 GitHub Pages 的最快方法是使用一个包——[GH-Pages](https://github.com/tschaub/gh-pages)。

[](https://github.com/tschaub/gh-pages) [## tschaub/GH-页数

### 将文件发布到 GitHub 上的 gh-pages 分支(或任何其他地方的任何其他分支)。npm 安装 gh-pages —保存-开发此…

github.com](https://github.com/tschaub/gh-pages) 

```
npm i gh-pages -D
```

或者您可以全局安装软件包:

```
npm i gh-pages -g
```

将这个简单的脚本添加到您的 **package.json** :

```
{
  "scripts": {
    "deploy": "npm run build && gh-pages -d dist"
  }
}
```

**注意**:假设构建文件夹是`dist`。

当您运行`npm run deploy`时，构建文件夹的所有内容都将被推送到您的存储库的 gh-pages 分支。

# 如果您正在 GitHub 中创建用户页面

用你的用户名创建一个库，比如`username.github.io`，创建一个名为`code`的分支，或者你可以给分支起任何名字。在这个分支中构建应用程序，在部署应用程序时，使用`gh-pages`命令将构建文件夹的内容推送到 gh-pages 分支

***注意*** *:在这种情况下，你需要将你的构建目录推送到* `*master*` *分支，使用下面的命令*

```
{
  "scripts": {
     "deploy": "npm run build && gh-pages -d dist -b master",
  }
}
```

运行`npm run deploy`之后，你应该在`[http://username.github.io](http://username.github.io)`看到你的网站。

运行 **gh-pages —帮助**列出 gh-pages 包的所有支持选项。

# gh 页的有用 npm 脚本

如果您的应用程序源代码位于私有存储库中，请创建一个名为 about 的公共存储库，源代码将位于私有存储库中，而从构建中生成的静态内容将进入公共存储库

```
{
  "scripts": {
    "deploy": "npm run build && gh-pages -d dist --repo <url>",
  }
}
```

部署到另一个分支[不是 gh-pages]:

```
{
  "scripts": {
    "deploy": "gridsome build && gh-pages -d dist -b master",
  }
}
```

要在将更改推送到分支时包含点文件:

```
{
  "scripts": {
    "deploy": "npm run build && gh-pages -d dist -t"
  }
}
```

要在发布变更时更改提交消息:

```
{
  "scripts": {
    "deploy": "npm run build && gh-pages -d dist -m Build v1"
  }
}
```

## 简单英语的 JavaScript

你知道我们有三份出版物和一个 YouTube 频道吗？在[T3【plain English . io找到一切的链接！](https://plainenglish.io/)