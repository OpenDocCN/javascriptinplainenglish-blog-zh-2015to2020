# 解释了 Node.js 应用程序中不同类型的依赖关系

> 原文：<https://javascript.plainenglish.io/what-the-dependency-types-of-dependencies-in-a-node-js-application-explained-904a5424fbd3?source=collection_archive---------1----------------------->

![](img/e084d1740aa98bd545c49f11ee8710a8.png)

Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在创建或使用 node js 项目的过程中，每个开发人员偶尔都会遇到这样的短语:

```
**npm i package_name
npm i package_name --save-dev
npm i package_name -g**
```

现在还有其他的旗帜，比如:

I 是 install 的简写

```
**npm i package_name --save-optional
npm i package_name --no-save**
```

这些旗帜是什么意思？这些命令如何塑造我们的项目？

这些问题的答案就在 **package.json** 文件中。当我们创建一个节点 js 项目时，我们在我们的项目文件夹中使用命令: *npm init* ，这导致某些问题，并最终创建 **package.json** 文件，或者如果我们下载一个节点 js 项目，我们总是在项目的文件夹中找到这个文件。

现在这个文件包含了很多关于项目的信息，比如项目的名称、版本、入口点(main)、脚本、作者等等。但是关键的是，它包含了关于**项目的依赖**的信息，如果有的话。当用户发布他/她的项目时，项目接收者运行的第一个命令是: *npm install* 或者一些调用这个命令的脚本。这是建立项目的一部分。这是节点包管理器(npm)查看 **package.json** 文件，并安装 **package.json** 文件中列出的所有依赖项。但是依赖关系的类型不止一种。

*   **依赖性(生产依赖性)**
*   **devDependencies(开发依赖关系)**
*   **选择性依赖**
*   **对等依赖**
*   **捆绑依赖**

# 生产依赖性

**生产依赖关系**是正常的依赖关系，是运行项目所必需的。这些是在项目代码中使用的包。例如，如果我的应用程序使用 mongoDB，那么在我的应用程序中可能有一个 [***mongodb 节点包***](https://www.npmjs.com/package/mongodb) 的依赖项。这些依赖项在 **package.json** 文件中的“依赖项”下指定。和开发人员可以使用

```
npm i package_name
// or
npm i package_name --save-prod
```

来安装它们。如果安装了 **package.json** 文件中“dependencies”项下的所有依赖项，应用程序将无法正常工作。

# 开发依赖性

接下来是**开发依赖**，这些是在开发时需要的依赖，但不负责应用程序的工作，也就是说，即使我们跳过这些依赖，我们的应用程序也会正常工作。其中最常见的就是[***nodemon***](https://www.npmjs.com/package/nodemon)*这其中*

> *是一个帮助开发基于 node.js 的应用程序的工具，它在检测到目录中的文件更改时自动重新启动 node 应用程序。*

***Nodemon** 在开发应用程序时可能会有所帮助，因为当我们进行一些更改时，它使我们不必一次又一次地启动我们的应用程序，但当我们部署我们的应用程序时，它通常是无用的，因为用户/部署环境必须启动我们的应用程序一次，因为当唯一的目的是使用应用程序而不是修改它时，不必进行任何更改。开发人员可以使用下面的命令将软件包作为开发依赖项进行安装。*

```
*npm i package_name --save-dev*
```

*通过发出这个命令，一旦安装了这个包， **package.json** 文件将被修改，这个包及其版本将被保存在关键字“devDependencies”下。*

# *可选依赖项*

***可选依赖关系**是那些顾名思义是可选的依赖关系，即在为应用程序/项目安装依赖关系时，这些依赖关系不会导致安装失败，因为 **npm** 将忽略这些依赖关系，如果这些依赖关系失败或未找到。无论有没有这些依赖项，应用程序都会运行得很好。*

*现在为了让事情更清楚，考虑一下这个场景，假设您正在制作一个节点脚本，它接受一些输入并在控制台上记录多个自定义输出。控制台日志颜色相同，为了区分日志，您使用 [***粉笔***](https://www.npmjs.com/package/chalk) 作为帮助您设置日志样式的依赖项，但是当您分发此脚本/应用程序时，脚本/应用程序的接收者/用户可能也使用**粉笔**来设置日志样式，这不是必需的，但也是可能的。这将使 chalk 成为这种特殊情况下的可选依赖项。这种依赖性不应该中断脚本/应用程序的工作，因为它的目的是控制台日志，而不是样式。当使用可选的依赖项时，要记住的一件事是，处理依赖项的缺乏仍然是你的程序的责任。如果您需要此任务的进一步帮助，请阅读此处的[](http://npm.github.io/using-pkgs-docs/package-json/types/optionaldependencies.html)*。要使依赖项可选，开发人员可以使用以下命令:**

```
**npm i package_name --save-optional**
```

**这将导致您的包出现在 **package.json** 文件中的“optionalDependencies”键下**

# **对等依赖**

**当你为一个主机工具或包开发一个插件/包的时候，你会用到这些。这意味着您希望插件/包的用户安装这些依赖项，而不一定在您的插件/包中使用这些依赖项。例如，如果我正在为一个名为“汽车版本 3.5.0”的虚拟主机工具或库制作一个名为“电动汽车版本 1.2”的虚拟插件/包，那么我将在我的 **package.json** 文件中的“peerDependencies”键下指定版本为 3.5.0 的包“汽车”。用户应该在安装/使用主机工具或库的插件之前安装这些主机依赖项。记住从插件目录运行 *npm 安装*不会安装这些包。**

# **捆绑的依赖关系**

**这些是在发布我们的包时捆绑的包的数组。如果您希望将某些或所有依赖项捆绑在一个文件中，那么您可以在您的 **package.json** 文件中的关键字“bundledDependencies”下指定这些依赖项。要实际创建一个包含**bundle dependencies**中提到的所有依赖项的 tarball，您可以发出命令: *npm pack* ，这将创建一个**。tgz** 文件名类似:*name _ of _ the _ project-version _ of _ the _ project . tgz*。在这之后，用户可以简单地发出命令*NPM install**name _ of _ the _ project-version _ of _ the _ project . tgz*来使用单个文件安装项目捆绑依赖项。当我们想要保留我们的依赖项并使用单个文件使它们可用时，就使用这种方法。**

# **—奖金—**

**当开发人员在 node js 应用程序上工作时，他们有时会使用以下命令:**

```
**npm i package_name -g**
```

**此命令用于将节点 js 包安装为全局依赖项，这意味着此包是全局安装的，并且只能在使用开发人员机器的任何应用程序/项目中使用，这些是经常使用的包，从未被任何特定的应用程序/项目使用。因此，在安装这些模块时，它们不会依赖于 **package.json** 文件，事实上，这些模块不会导致对 **package.json** 文件的任何修改。这些通常是为开发人员提供某些实用程序的包，这些实用程序在开发中经常使用，但在生产中几乎从不需要。一些例子是[**create-react-app**](https://www.npmjs.com/package/create-react-app)包，它用于创建 react app 的 react app 框架，而无需为初始设置进行构建配置，像 **nodemon** 这样的包也可以全局安装。**

**有时，您可能还会遇到以下命令:**

```
**npm i package-name --no-save**
```

**这样做的目的是安装一个在项目中使用的包，而不是作为依赖项。这些包是*一次性的*，应用程序不依赖于这些包，它们也不会像全局包安装一样列在 **package.json** 文件中的任何依赖项下。**

**但是主要的区别是，全局包可以从任何地方(任何项目)通过 node 访问，安装了-no-save 标志的包只能从发出命令的特定项目访问。**

# **Package . JSON vs Package-lock . JSON**

**这两个文件是 node js 应用程序中依赖关系管理的核心。**

**Package.json 文件用于提供关于项目的更多信息，package-lock.json 提供关于依赖项的更多信息。Package.json 文件包含关于所有种类的主要依赖关系的信息，例如如果我的应用程序使用 [***express***](https://www.npmjs.com/package/express) 作为 *devDependencies* 它将在 package.json 文件中列出，但是 *express* 本身具有 **30** 依赖关系和**18**dev 依赖关系，这些信息包括版本、完整性等。将在 package-lock.json 文件中列出。因此，当发出 *npm install* 命令时 **npm** 利用这些文件创建一个完整的功能构建并相应地安装依赖项。**

**这就是所有的人。我可能漏掉了一些要点，但大致就是这样。我只是想确保我在这里提到的包，如 nodemon、chalk、express、mongodb 等。在这篇文章中，我并不是在推广它们，我提到它们只是因为我广泛地使用它们，它们让我的生活变得轻松了一些。如果你觉得这篇文章有帮助，请留下掌声。**

**欢迎在其他社交平台上联系我，比如[](https://www.instagram.com/ab_si_/)**[**LinkedIn**](https://www.linkedin.com/in/abhishek-singh-2939a0aa/)[**脸书**](https://www.facebook.com/proabe)**我一直都想进行一场智慧的对话，祝你周末愉快，编码愉快😉。******