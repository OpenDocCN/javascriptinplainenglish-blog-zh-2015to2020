# 角度部署到 GitHub 页面

> 原文：<https://javascript.plainenglish.io/angular-7-deploy-to-github-pages-images-assets-7704f3b2005c?source=collection_archive---------1----------------------->

![](img/db1ab03086bd8fdcf0b00d943ba5c03d.png)

你可能听说过 GitHub 有一个惊人的功能，可以免费将你的网站部署到他们的域中。

你需要什么？

1.  GitHub 帐户或组织。
2.  代码库。

这里我们就来说说使用 ***angular*** 和***github******pages***部署单页 app。

**设置**:

1.  你有一个有角度的项目。
2.  从 npm 安装 angular-cli-ghpages 包，如下所示

```
'npm install -g angular-cli-ghpages'
```

3.您可以像这样设置 npm 脚本，或者直接在终端中运行:

```
"build-and-deploy-gh-pages": "ng build --prod --base-href ./ && npx ngh --dir dist/[reponame]"
```

哪里*。/* 可以替换为:

```
'[https://[username].github.io/[reponame]/](https://[username].github.io/[repo]/)'
```

事实上，在上面的链接中你会找到你的 github 页面。

您将被要求输入您的 github 用户名和密码以部署到 gihub 页面。该脚本将自动创建 gh-pages 分支，并设置您的 repo 进行在线发布。

下一步是运行该脚本，如下所示:

```
'npm run build-and-deploy-gh-pages'
```

为了正确编译图像，请在组件中使用如下路径:

```
'./assets/images/image.png'
```

**如果不遵循这种方法，您可能会面临图像加载问题！**

作为证明，你可以查看这个[库](https://github.com/ngQuad/website)和它的 github-pages [链接](https://ngquad.github.io/website/)。

如果你有疑问和问题，随时联系我:D。