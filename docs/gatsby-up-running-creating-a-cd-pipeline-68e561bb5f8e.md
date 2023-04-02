# Gatsby 启动和运行:创建 CD 管道

> 原文：<https://javascript.plainenglish.io/gatsby-up-running-creating-a-cd-pipeline-68e561bb5f8e?source=collection_archive---------9----------------------->

![](img/89f725f30873bdc8d78acef7f5bac608.png)

source: [https://docs.travis-ci.com/](https://docs.travis-ci.com/)

我的最后一次冒险是把我简陋的 Jekyll 网站迁移到盖茨比。我曾经通过在本地构建 Jekyll 站点并将生成的静态内容推送到 GitHub 上的`gh-pages`分支来手动部署我的 Jekyll 站点。有了新网站后，我决定是时候抛弃我的旧方式，建立一个自动管道来将我的网站部署到 GitHub 页面。

# 为什么选择 GitHub 页面？

我一直使用 GitHub 页面来托管我的博客，因为它是轻量级的，并且与 GitHub 无缝集成。它是简单的，免费的，并提供免费的好处，如强制 HTTPS 默认，也支持添加一个自定义域。

# 在本地部署 Gatsby 站点

在自动化部署我的新 Gatsby 站点之前，我学习了如何在本地部署它。为此，我在`package.json`的`scripts`标签中添加了一个条目，为我的 Gatsby 站点创建了一个生产就绪的构建。

```
"scripts": { 
  ...
  "build": "gatsby build"
}
```

`npm run build`构建站点并将生成的资产放在一个名为`public`的文件夹中。将这个`public`文件夹复制到任何 HTTP 服务器都会将我的站点部署到那个服务器上。

我使用 [http-server](https://www.npmjs.com/package/http-server) 启动了一个 HTTP 服务器，为`public`文件夹中的资产提供服务，这有效地在本地部署了我的站点。

```
➜  cd public
➜  http-serverStarting up http-server, serving ./
Available on:  
http://127.0.0.1:8082  
http://10.10.10.10:8082 
http://192.168.13.12:8082
```

# 将 Gatsby 站点手动部署到 Github 页面

## 设置 Github 页面

自从我在我的 Jekyll 站点上使用 GitHub 以来，我已经设置了 GitHub 页面，但是为了完整起见，我将在这里重复这些步骤。

我前往我的 GitHub 库的设置页面，向下滚动到`GitHub Pages`部分。

![](img/5caf346f7b3235f717622ca7a1eedde3.png)

Settings for a GitHub Repository

我不想让 Gatsby 构建过程生成的文件弄乱我的`master`分支，所以我选择了`gh-pages`分支作为我的 GitHub Pages 站点的源分支。我推送到这个分支的任何静态站点都会在`deborah-digges.github.io`发布。我还喜欢默认启用`Enforce HTTPS`的事实。所有的保安！

![](img/70b17694aa53e97b54a2709ff577cd5e.png)

GitHub Pages Configuration

## 手动部署

我通过在本地构建站点并仅将文件夹`public`推送到 GitHub 上的`gh-pages`分支，将站点手动部署到 GitHub 页面。

这需要一系列相当笨拙的命令。

这很难理解，而且很容易用`rm -r *`这样的命令意外地毁掉你的整个电脑。这是一件好事，gh-pages 包的存在拯救了我们自己。

我将依赖项添加到我的项目中，让它有所变化。

```
yarn add gh-pages
```

为了让`gh-pages`知道我的存储库位于哪里，我在`package.json`中添加了`homepage`属性。

```
{
  ... "homepage": "deborah-digges.github.io" ...
}
```

我给我的`package.json`添加了另一个脚本`deploy`，它执行`build`，然后将`public`文件夹推送到我在 Github 上的库的`gh-pages`分支。

```
"scripts": {
  "build": "gatsby build",
  "deploy": "npm run build && gh-pages -d public"
}
```

瞧啊。我能够使用`npm run deploy`脚本从我的计算机手动部署我的站点。那时，我的网站在 deborah-digges.github.io 运行

# ✨Continuously✨与 Travis CI 一起部署盖茨比站点

太好了！我部署了我的盖茨比站点！然而，我不想在周五下午从我的笔记本电脑上手动部署一个站点。

我决定[持续部署](https://www.atlassian.com/continuous-delivery/principles/continuous-integration-vs-delivery-vs-deployment)我的站点，这样每个新的提交都会自动部署到我的 GitHub Pages 站点。这很令人兴奋，但是我忽略了一个重要的步骤，那就是对我的网站进行自动化测试，以确保一个错误的提交不会导致我的整个博客瘫痪。然而，我决定在*过危险的生活，并为将来的博客帖子保留我的盖茨比网站的测试。*

## 报名

我用我的 GitHub 账户注册了 Travis CI 网站，并同意与 Travis 分享我的 GitHub 数据。

## 启用存储库

然后，我前往[存储库](https://travis-ci.org/account/repositories)页面，并为`deborah-digges.github.io`存储库启用构建。

![](img/2227f9a06f08b9fcd930aeb44a59730d.png)

Toggle for Enabling a Repository on Travis CI

## 添加 travis.yml 文件

我将一个`travis.yml`文件添加到我的存储库的根目录，告诉 Travis 在每次提交`master`时做什么。

.travis.yml

`script`运行我之前用来在本地部署站点的`yarn run deploy`步骤。它做了一些额外的步骤来给 Travis CI 正确的访问权限，以推送至我的 GitHub 存储库。

它告诉安装在 Travis CI 上的`git`客户端我是谁。

```
git config --global user.name $GH_USERNAME
git config --global user.email $GH_EMAIL
```

为了提供对我的 GitHub 存储库的脚本推送访问，我在远程 URL 中嵌入了我的 [Github 令牌](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token)环境变量。

```
git remote set-url origin "https://${GH_USERNAME}:${GH_TOKEN}@github.com/deborah-digges/
```

这些环境变量从何而来？

## 配置 Travis 环境变量

我转到我的存储库设置，让以下环境变量对我的脚本可用。

![](img/c248ddd114854d1c770f72c3d38e4be3.png)

Environment Variables in Travis CI

我尝试了几次才把它做好，但是我很自豪地说，我的站点现在在每次提交到我的库的`master`分支时都被持续地部署到 GitHub 页面上。

![](img/a5215769beec829369bc115c9b10e59c.png)

Latest Git Commits with Travis CI Enabled

这是一项繁重的工作，在我寻找更简单的解决方案的过程中，我将探索使用 GitHub 动作来持续部署我的站点。敬请关注更危险的生活！

*原载于*[*https://Deborah-digges . github . io*](https://deborah-digges.github.io/2020/09/24/gatsby-deploy/)*。*