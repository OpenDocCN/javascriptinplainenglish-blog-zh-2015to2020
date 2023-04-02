# 是的，它是 npx，而不是 NPM——差异解释

> 原文：<https://javascript.plainenglish.io/yes-its-npx-not-npm-the-difference-explained-58cbb202ec33?source=collection_archive---------0----------------------->

![](img/dfafd5495a706804f025d41f377a2b5b.png)

最近当我开始学习反应时，我看到很多人(包括我自己)在看到反应时感到困惑，而不是非常熟悉的反应。

我们中的一些人觉得这很奇怪，但没有多想，其他人认为这是一个打印错误，甚至通过运行`npm`而不是`npx`来*修复它。*

*当我不止一次看到某事发生时，我认为它值得解释。这就是为什么这篇文章，写给所有和我有同样误解的人:*

> *不是错别字，是 npx，不是 npm！😃*

# *NPM*

*我们可能已经知道， [**npm**](https://www.npmjs.com/) 是 *Node.js* 的包管理器，其目标是自动化**依赖**和**包管理**。*

*这意味着我们可以在`package.json`文件中为一个项目指定我们所有的依赖关系*(包)*，每当有人需要为它安装依赖关系时，他们只需运行`npm install`，瞧！*

*它还提供了**版本控制**，也就是说，它可以指定我们的项目所依赖的**版本**，因此我们可以在很大程度上防止更新破坏我们的项目或使用我们的首选版本。*

# *NPX*

*另一方面，**[**npx**](https://www.npmjs.com/package/npx)是**一个用于执行节点包**的工具，从 *npm5.2* 版本开始，它与 *npm* 捆绑在一起。***

*****npx** 的作用如下:***

```
*****1.** By default, it first checks if the *package* that is to be executed exists in the path (i.e. in our project);**2.** If it does, it executes it;**3.** Else, means that the package has not been installed, so npx installs its latest version and then executes it;***
```

***如上所述，这种行为是 **npx** 的默认行为，但是它有*标志*可以用来防止这种行为。
举例来说，如果我们运行`npx some-package *--no-install*`意味着我们告诉 **npx** 它应该**只**尝试**执行**名为
`some-package`的包，**不安装**如果它以前没有被安装过的话。***

> ***更多关于 **npx** 旗帜[这里](https://www.npmjs.com/package/npx)。***

# ***例子***

***假设我们有一个名为`**my-package**`的包，我们想执行它。***

***嗯，如果没有 **npx** ，要执行一个包，我们必须从它的本地路径运行它，就像这样:***

```
***./node_modules/.bin/my-package***
```

***或者在`package.json`文件的*脚本部分*中将其定义为一个单独的脚本，如下所示:***

```
***{
  "name": "something",
  "version": "1.0.0",
  "scripts": {
    "my-package": "./node_modules/.bin/my-package"
  }
}***
```

***然后用`**npm run my-package**`运行它。***

***现在，有了 **npx，**我们只需运行`**npx my-package**`就能轻松做到。***

# ***结论***

***因此，为了强调这一点，`npm !== npx`我希望这篇简短的文章有助于澄清这个误解。***

***如果你有任何问题，欢迎在评论中提问！***

***继续编码，做令人敬畏的事情，保持快乐！😊***