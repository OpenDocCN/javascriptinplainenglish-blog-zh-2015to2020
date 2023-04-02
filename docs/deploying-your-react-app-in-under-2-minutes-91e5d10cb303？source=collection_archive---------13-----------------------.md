# 在两分钟内部署您的 React 应用程序

> 原文：<https://javascript.plainenglish.io/deploying-your-react-app-in-under-2-minutes-91e5d10cb303?source=collection_archive---------13----------------------->

## React 应用程序的超快速部署

![](img/5621bb6783f848ebf976ca1a52d76e5a.png)

Photo by [Victor Rodriguez](https://unsplash.com/@vimarovi?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果你是一个热衷于构建 React 应用程序的开发人员，那么 GitHub 页面可能是你托管这些应用程序的最佳选择。你所要做的就是遵循一些简单的步骤，你就可以直接从你的存储库中托管尽可能多的应用程序。听起来很刺激？念开了！

GitHub Pages 是一个静态站点托管服务，它直接从 GitHub**上的存储库中获取文件，并发布一个网站。如果需要，我们可以选择通过构建过程运行文件。以下是部署 React 应用程序需要遵循的步骤:**

# 1.将您的项目存储在 GitHub 中

确保将项目文件存储在 GitHub 的存储库中。它可以是私有或公共存储库。

# 2.作为开发依赖项安装 GitHub 页面

下一步是将 GitHub Pages 作为项目的开发依赖项进行安装。为此，请运行:

```
npm install gh-pages --save-dev
```

# 3.在`package.json`文件中定义主页属性

在 react 应用程序的`package.json`文件中，使用给定的语法添加 homepage 属性:

```
"homepage": "http://**<github_user_name>**.github.io/**<github_repo_name>**"
```

确保用您的项目详细信息替换用户名和 repo 名称。

# 4.向`package.json`文件添加附加脚本

现在我们需要在`package.json`文件中添加部署脚本。在现有的脚本属性中，添加一个`predeploy`属性和一个`deploy`属性，每个属性的值如下所示:

```
"scripts": {
  // other scripts
  "predeploy": "npm run build",
  "deploy": "gh-pages -d build"
}
```

`predeploy`脚本构建最终代码，而`deploy`脚本部署代码。

# 5.部署您的项目

现在您需要运行`deploy`脚本来部署项目。

```
npm run deploy
```

# 结论

React 应用的部署非常简单，不到两分钟即可完成。我希望你觉得这很有用。我已经链接了我的回购，发布了 Github 页面供参考。感谢阅读！

[](https://github.com/harshaktg/reactdeploydemo) [## harshaktg/reactdeploydemo

### 这个项目是用 Create React App 引导的。在项目目录中，您可以运行:在…中运行应用程序

github.com](https://github.com/harshaktg/reactdeploydemo)  [## React 应用

### 使用 create-react-app 创建的网站

harshaktg.github.io](https://harshaktg.github.io/reactdeploydemo/)