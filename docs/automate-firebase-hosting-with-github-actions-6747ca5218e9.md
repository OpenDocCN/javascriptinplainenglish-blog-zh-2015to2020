# 使用 Github 动作自动化 Firebase 托管

> 原文：<https://javascript.plainenglish.io/automate-firebase-hosting-with-github-actions-6747ca5218e9?source=collection_archive---------8----------------------->

![](img/8c64a51cb07cad3c6c218d5b69d1f9b5.png)

> 更新 01/21/21:使用“firebase init”命令，您现在可以通过 CLI 从 Github 自动设置部署，您不必遵循本教程的任何内容。先试试那个。它应该为合并设置一个操作，为 PR 设置一个操作，为每个 PR 创建预览站点。

这是一个快速设置 Github 动作来部署一个站点到 Firebase 主机的教程。这将包括用 Vue、React、Gatsby、Next.js 静态生成器或任何其他客户端站点构建的站点。几分钟后你就可以设置好了，这样每次你把代码推给 master，它就会自动被 Github 构建并部署到 Firebase。我假设您已经在 Github 上编写了代码，并设置了 firebase 项目。如果您的项目在根目录中没有一个`firebase.json`文件，或者如果该文件没有托管部分，您可能需要运行`firebase init`。还要确保您有一个带有您的项目 ID 的`.firebaserc`文件，否则部署功能将无法工作。如果您运行`firebase init`，它应该已经为您创建好了。

这里有一个关于`firebase.json`文件应该是什么样子的例子:

`site-name`应该替换为您想要部署到的站点的名称，而`dist`应该是您的站点构建到的目录。通常不是 dist 就是 public。

接下来，您需要在路径`.github/workflows/`下创建一个名为`main.yml`的文件。该文件应该包含以下内容:

这里您可能需要更改的是对`dist`的任何引用，更改为您的构建脚本输出最终代码的任何内容，并且`npm run build`行可以更改为您的构建命令。还要确保将`site-name`更改为您在最后一步中在`firebase.json`文件中使用的内容。注意:您可以很容易地用 yarn 替换 npm，yarn 目前在构建环境中是全局可用的。

这个文件基本上运行两个独立的任务:一个从源代码构建站点文件，另一个获取工件并将其部署到 Firebase 主机。

这里需要做的最后一件事是将您的秘密 Firebase 令牌添加到 Github，这样它就有权限为您部署站点。要获得这个令牌，在您的终端中运行`firebase login:ci`并复制生成的代码。您将把该代码粘贴到秘密页面上 Github 项目的设置中。创建一个名为`FIREBASE_TOKEN`的新密码，并粘贴您从终端获得的代码。

现在，您可以提交这两个文件并推送到 Github，Github 动作会处理剩下的事情。推送之后，你应该可以在 Github 项目的 actions 标签中看到进度。

*最初发表于*[*【https://codegregg.com】*](https://codegregg.com/blog/firebaseHostingGithubActions/)*。*

## **简明英语团队的笔记**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页[**plain English . io**](https://plainenglish.io/)找到所有这些内容——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**