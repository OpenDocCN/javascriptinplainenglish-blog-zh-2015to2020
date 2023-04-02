# 创建您的第一个 React 网站的分步指南|创建-React-应用程序

> 原文：<https://javascript.plainenglish.io/the-easiest-way-to-create-your-first-react-app-5b2acb6dd518?source=collection_archive---------10----------------------->

让我们创建完全没有构建配置的 React 应用程序。它只需要不到 30 秒的时间，并且您只需要在终端上运行一个命令就可以开始了。

![](img/e883185942701c9997300377efeada7f.png)

Photo by [Gia Oris](https://unsplash.com/@giabyte?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

然而，在开始之前，让我向那些新的创建-反应-应用程序的观众说一声。

# 什么是 create-react-app？

1.  它基本上是一个由脸书的开发者开发的工具。
2.  当我们开始构建 React 应用时，它给了我们一个巨大的开端。
3.  它将我们从耗时的设置和配置中解救出来。
4.  您只需运行一个命令，创建 React 应用程序，设置启动 React 项目所需的工具。

# 背后的哲学

## 1)一个依赖项

如果没有它，您将不得不自己处理/安装数十个依赖项，并且每当新的 react 版本出现时，您还必须管理所有这些依赖项。比方说，现在您使用的是 react 版本 10，您必须升级到版本 12。你必须检查每一个依赖项，看看它们是否与最新的 React 版本兼容，然后你必须升级它们。不仅如此，您还必须留意每个依赖项的主要发布版本所带来的任何重大变化。

那是很大的工作量！

> 脸书的这项倡议(创建-反应-应用)背后的基本理念是只有一个依赖。

## 2)不需要配置

它使用了 Webpack、Babel、ESlint 和许多其他令人惊叹的项目，但它为您提供了一个基于它们的策划体验，并且不需要任何配置。

> *已经为您处理了相当多的开发和生产版本的配置，因此您可以专注于编写代码。*

## 3)没有锁定

此外，没有锁定。如果你是一个有经验的程序员，你想自定义设置。为此，您可以随时运行**弹出**命令。只需一个命令，所有的配置和构建依赖项都将直接转移到您的项目中，这样您就可以从中断的地方重新开始。

# 创建我们的第一个 react 应用

已经够了！让我们从官方支持的只使用一个命令创建单页面 react 应用程序的方法开始。

## 该命令是:

```
npx create-react-app PROJECT_NAME
```

作为旁注 **npx** 自带 **Npm 5.2** 或更高版本。所以，如果你有一个旧版本的 Npm，我强烈建议你浏览一下[文档](https://reactjs.org/docs/create-a-new-react-app.html)，看看旧版本的说明。

就是这样，这个过程大概需要 30 或者 35 秒。最后，我们有史以来第一个 react 应用程序已经准备好接受服务和测试！

运行应用程序的命令:

```
yarn start (or npm start)
```

如果你访问 localhost:3000，我们的 react 应用程序就在这里，启动并运行，你已经正式创建了你的第一个 react 应用程序= >

# 文件夹结构一览

让我们浏览一下文件夹结构。这是一个非常精简的文件夹结构，只有几个文件(我们需要的文件)。

create-react-app 库已经处理了很多事情。例如，如果您看到 **package.json 文件**，这个文件相当精简。它基本上只有大约七个依赖项，如果你真的看到一个没有 create-react-app 的真实应用程序，它有一个相当大的 package.json 文件，这对新开发人员来说可能是一个威胁。

很简单，对吧？

# 使用模板(typescript)

您可以选择添加一个模板，如下所示:

```
npx create-react-app PROJECT_NAME --template TEMPLATE_NAME
```

假设我想启动一个 react 应用程序，但这次我希望它是一个 TypeScript 应用程序。好吧，给你:

```
npx create-react-app PROJECT_NAME --template typescript
```

与之前非常相似的体验，但这次模板是 TypeScript，所有文件都是 TypeScript 格式。

有许多模板可供选择。点击此[链接](https://create-react-app.dev/docs/getting-started/#selecting-a-template)了解更多信息。

# 弹出您的应用程序

这个库的最后一个功能是**弹出**你的应用程序。

```
npm run eject
```

这是单向操作。当心，你不能回去！

所以在跑步之前，你真的需要理解它到底意味着什么。如果您对构建工具或 create-react-app library 做出的配置选择不满意，您可以随时使用此命令弹出。

它将从项目中移除单个生成依赖项。基本上，它会直接在 package.json 文件中显示所有的依赖项。因此，您现在可以自己处理依赖性了。

但是，您不必使用弹出功能。create react 应用程序为我们提供的精选功能最适合中小型部署，您不应该觉得有义务使用该功能。

然而，如果你有一个大而复杂的应用程序，并且你真的知道你在做什么，那么你就应该弄乱它。

我希望这篇文章能帮助您理解最新的 NodeJS 版本。

感谢阅读。如果你有任何问题，请随时回复。

# 资源

1.  [https://create-react-app.dev/docs/getting-started](https://create-react-app.dev/docs/getting-started)
2.  [https://github . com/Facebook/create-react-app/blob/master/changelog . MD](https://github.com/facebook/create-react-app/blob/master/CHANGELOG.md)

*原载于*[*https://www.theimmigrantprogrammers.com*](https://www.theimmigrantprogrammers.com/p/the-easiest-way-to-create-your-first?r=ch20b&utm_campaign=post&utm_medium=web&utm_source=copy)*。*