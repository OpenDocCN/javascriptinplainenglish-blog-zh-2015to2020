# 为您的项目创建样板模板

> 原文：<https://javascript.plainenglish.io/creating-boilerplate-templates-for-your-projects-215bd9365053?source=collection_archive---------1----------------------->

![](img/24ab74f01ba00e92792ed57b75bd7b54.png)

Photo by [Martin W. Kirst](https://unsplash.com/@nitram509?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/coding?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

有多少次你开始一个新项目，然后花几分钟时间安装依赖项，为你的设置写配置文件。如果有一种更好的方法来生成样板文件并立即开始编码会怎么样。在今天的文章中，我将向您展示如何创建您自己的 cli 来设置您的项目，并且您将准备好开箱即用地开始编码。

我们需要做的第一件事是初始化一个项目。让我们打开我们最喜欢的终端，为我们的项目创建一个新目录。然后我们用 npm 初始化它。

```
mkdir cli-project && cd create-express-app
npm init -y
```

对于我们的 CLI，我们需要一些包:

*   *arg*
*   *粉笔*
*   放电合成法（Electric Synthetic Method 的缩写）
*   execa
*   调查者
*   列表
*   国家汽车停车场
*   pkg-安装

```
npm i -S arg chalk esm execa inquirer listr ncp pkg-install
```

一旦我们安装了软件包，让我们设置我们的目录结构:

```
mkdir {bin,src,templates}
```

我们将从 bin 目录开始，让我们创建我们的文件，`create-express-app`。注意没有文件扩展名，这是有意的。现在让我们设置我们的文件:

```
#!/usr/bin/env node require = require('esm')(module /*,options*/);
require('../src/cli').cli(process.argv);
```

第 1 行告诉 shell 用 node 运行这个脚本。我们使用 esm，因此我们可以使用 ES6 导入语法。第三行调用 cli。

我们需要在 package.json 中添加一个键，告诉我们的脚本在哪里:

```
"bin": {
  "@npmusername/create-express-app": "bin/create-express-app",
  "create-express-app": "bin/create-express-app"
},
"publishConfig": {
  "access": "public"
}
```

在我们的 src 目录中，我们将创建我们的文件。我已经决定将文件分成几个关注的区域，即文件复制、git、初始化 npm、包和实用程序。让我们从`utils.js`文件开始:

我们有两个助手函数，一个用于包，另一个包装我们的子任务。

我们的下一个文件是我们的`pkgjson.js`文件。我们在这里处理 NPM、Git 的初始化，并复制模板文件。它还根据您是否需要 typescript 来处理`package.json`中的 scripts 键。

我们列表中的下一个文件是`packages.js`文件。它列出了项目的所有依赖项。我将包存储为键，每个键是包的数组。我有两个独立的变量，一个用于开发依赖，一个用于依赖。我可以用 Object.keys 方法遍历这些键，并创建一个名为`pkgList`的新变量。在`packageListGenerator`助手功能的帮助下，我能够从包中创建任务。然后在 TaskListGenerator 的帮助下，我可以制作父列表。您还会注意到第三个参数是一个布尔值，它告诉 Listr 任务是否要运行。当你运行这个的时候，它会问你是否要安装 TypeScript，如果你选择 no，它不会运行任何基于`options.typescript`变量的带有 TypeScript 的任务。

既然我们已经复制了文件，也准备好了安装包，我们需要把它和一个`main.js`文件放在一起。

该文件根据您的选择和目标目录设置模板。之后，它调用我们为复制文件和安装包而创建的两个函数。在它运行之后，它将在您将它传递给`create-express-app`命令时指定的目录中创建您的项目。这是最后一个`cli.js`文件，当你输入 cli 命令时，它会加载这个文件。

CLI 可以接受两个参数，一个是创建的目录的项目名，另一个是启用 typescript 的`--typescript`或`-ts`命令。如果您不提供任何命令，CLI 将提示您输入问题和默认答案，默认答案是 Yarn、api 和 TypeScript 的 N。您可以根据自己的喜好进行定制。

有了这些之后，我们需要创建模板文件。这个项目的模板文件应该位于 templates 目录中，有两个目录，一个用于`typescript`，一个用于`javascript`。根据您传入的内容，将是它复制的目录。对于 JavaScript，目录中只有一个文件，一个简单的 express api:

我们的 typescript 文件位于它的 src 目录中，基本上是相同的，只是使用了 es6 语法。

当然，这不是一个很好的服务器，因为它是用于演示的目的。但是您可以用它来创建任何您想要的样板模板。如果你不想把它发布到 NPM 注册中心，你可以使用命令`npm link`把它链接到你的本地机器上并在本地使用它。

github 库可以在这里找到:[https://github.com/dswhitely1/create-express-app](https://github.com/dswhitely1/create-express-app)

如果你想看 React 版本，你可以在这里找到:[https://github.com/dswhitely1/create-react-project](https://github.com/dswhitely1/create-react-project)

# **简明英语团队的说明**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页[**plain English . io**](https://plainenglish.io/)找到所有这些内容——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**