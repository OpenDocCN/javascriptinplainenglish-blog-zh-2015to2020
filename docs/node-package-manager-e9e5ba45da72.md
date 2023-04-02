# 节点和 npm 入门

> 原文：<https://javascript.plainenglish.io/node-package-manager-e9e5ba45da72?source=collection_archive---------4----------------------->

多年来，周围有很多开发商。他们每个人都对 JavaScript 开发的许多部分做出了贡献。但是重复代码怎么办？如果许多开发人员一遍又一遍地编写代码，就会有许多重复的代码。为了避免这个问题，他们发明了一种叫做节点包管理器的东西。

![](img/6b76121c7b1190e88d5be2d1d9017d0d.png)

我们应该听说过 JavaScript 包。一个包是一个文件或一组文件，其中充满了现有的、*可重用的*代码。它们被设计成共享的，允许许多 web 开发人员在他们自己的项目中使用相同的代码。Npm(节点程序包管理器)有助于将这些程序包组织起来。

世界各地都有开发人员，他们用完美的解决方案解决了许多问题。当我们试图解决类似的问题时，当优秀的工作解决方案已经存在时，我们不需要重新发明整个解决方案。我们可以重用其他开发人员编写的代码，这些代码以包的形式提供。npm 就是用于这一目的的工具。现有代码的价值令人难以置信。

# 如何设置

如果您已经在计算机中安装了 Node.js，npm 应该已经安装在您的系统中了。如果您想确认您的计算机中有 npm，请输入以下命令。

```
node -v
```

如果您已经有一个版本，它应该会出现在您的终端中。您也可以使用下面的命令进行双重检查。

```
npm -v
```

如果没有出现版本号，您可以使用[节点版本管理器](https://nodesource.com/blog/installing-node-js-tutorial-using-nvm-on-mac-os-x-and-ubuntu/) (nvm)进行安装。您拥有的 npm 版本也可以使用以下命令更新到最新版本。

```
npm install --global npm
//npm install -g npm 
//the above line will also work;just a short form
```

顾名思义，npm 是一个包管理器，npm 通过命令行与您的 JavaScript 项目目录一起工作，允许您安装预先存在的代码包。你可以安装和使用不同大小的包，从最小的包如 *isNumber* 到巨大的库和框架如 *React* 和 *Express* 。

# package.json

`package.json`文件将告诉我们运行 JavaScript 应用程序所需的所有包，并列出每个包的名称。如果您在一个名为`package.json`的目录中输入以下命令，它将安装运行该 JavaScript 应用程序所需的所有包。

```
npm install
```

`package.json`文件是在 GitHub 等网站上共享 JS 代码库的关键部分。我们不需要在每个项目中包含所有依赖项的代码，我们只需要包含一个小文件，列出 npm 需要为项目获得什么。该文件通常还包含有关项目的信息，如名称、版本、作者和许可证。`package.json`文件是用 JSON 编写的，所以就像 JavaScript 中的对象一样，它总是用花括号括起来，并且包含键和值。

因为 npm 依赖于一个`package.json`文件，所以它有一个内置命令来*构建* `package.json`文件。运行以下命令将启动一系列提示，询问文件中包含的具体内容。最后，它将创建一个文件或编辑一个现有的`package.json`文件。

```
npm init
```

如果您正在从头开始构建一个项目，这将非常有用。到目前为止，对于像我这样的初学者来说，npm 是最有用的工具。希望我已经尽可能简短地给出了我对 npm 的理解。编码快乐！