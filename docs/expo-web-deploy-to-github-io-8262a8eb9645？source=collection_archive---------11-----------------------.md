# Expo 和 Github 的免费网站托管和 CD

> 原文：<https://javascript.plainenglish.io/expo-web-deploy-to-github-io-8262a8eb9645?source=collection_archive---------11----------------------->

## 使用 Expo Web 通过连续交付脚本部署到 GitHub.io

![](img/0c06e787dd13043efcbe34a7b9f6edf1.png)

Your phone, your code, and your localhost:19006 site — all that’s left is deployment! And coffee. That mug is totally empty. Photo by [Christopher Gower](https://unsplash.com/@cgower?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/web-development?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

您有一个 Expo.io 应用程序，并准备好部署您的 web 版本。我已经在 GitHub.io 上免费部署，并遇到了几个 *100%可避免的*问题——从那以后，用一个命令部署到生产就一帆风顺了。

在本文结束时，您将知道如何:

*   在 github.io 上部署您的网站(免费)
*   将一个(非免费的)自定义域连接到您的网站(并通过修改您的构建脚本来保持连接)
*   编写一个脚本来测试(如果有)、构建，然后将应用部署到生产环境中(本质上是 CI/CD)。

# 部署

在 github.io 上部署任何 GitHub pages 静态站点都很简单。首先，在项目中安装`gh-pages`作为 devDependency。

在构建您的网站(运行:`expo web:build`)之后，将您的资产部署到 GitHub repo 中的 gh-pages 分支，并使用以下命令进行推送:

```
gh-pages -d web-build
```

顺便说一下，这个实用程序比仅仅将静态站点部署到 GitHub 更有帮助。你可以用它推*任何东西*到*任何分支*，推*任何远程* (GitLab，BitBucket 等。—请注意，其他服务不会将这些文件作为网站静态提供)。当我出于公司原因不想在公共 NPM 上发布时，我用它将库输出推送到分支——是的，您可以将 git repo+分支指定为 NPM 依赖项。对于一些 CI/CD+Git 问题，`gh-pages`可能是您工具箱中的一个有用工具。要了解更多信息，请运行`gh-pages --help`。

从技术上来说，此时你已经部署在 GitHub.io 上了。然而，你可能想要连接一个自定义域名到你的站点上(比如[jamesfulford.com](http://jamesfulford.com))。

# 自定义域指令

您需要的大多数说明都来自 GitHub Pages 文档，但是其中也有一些 gotchya。如果你想要一个子域(“www。或者是“简历”)，请按照说明配置子域。如果没有，向下滚动以遵循配置“apex”域的说明。

 [## 管理 GitHub Pages 站点的自定义域

### 您可以设置或更新某些 DNS 记录和您的存储库设置，以指向您的 GitHub 的默认域…

help.github.com](https://help.github.com/en/github/working-with-github-pages/managing-a-custom-domain-for-your-github-pages-site) 

基本上，apex 域的步骤是:

*   从注册商处获得域名(godaddy.com，AWS Route 53 等。)
*   在回购的“设置”标签中，粘贴您的自定义域名。
*   将域名的 A 记录指向 GitHub.io 文档提供的 IP 地址。

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

*   在您的构建输出中，指定一个包含域名的 CNAME 文件。**参见下一节关于修改构建脚本的内容。**
*   (后来)启用 HTTPS。根据我的经验，选中复选框并不总是“有效”，所以确保以后再次选中它。

# 在每次构建时生成 CNAME 文件

在进行构建时，您需要自动包含一个 CNAME 文件。(否则，您的下一次部署将清除以前的 CNAME 文件，您的站点将会崩溃。)

在部署到 gh-pages 分支之前，构建一个脚本，将您的 CNAME 文件添加到构建输出中。请随意调整我的快速脚本以满足您的需求:

Don’t forget to change the “yourwebsitehere.com” bit!

将`"web:build": "bash build.sh"`添加到 package.json 脚本中。

# 一键部署脚本

一旦建立了构建脚本和定制域，您就可以运行一个命令来部署到生产环境中。我把它添加到我的 package.json 脚本中，然后知道我的应用程序的 web 版本在 GitHub 上免费测试和部署，晚上就可以睡个好觉了。

```
"web:deploy": "yarn ci && yarn web:e2e && yarn web:build && gh-pages -d web-build",
```