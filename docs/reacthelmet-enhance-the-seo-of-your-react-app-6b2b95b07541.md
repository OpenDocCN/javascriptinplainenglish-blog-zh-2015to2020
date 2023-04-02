# ReactHelmet |增强 React 应用的搜索引擎优化

> 原文：<https://javascript.plainenglish.io/reacthelmet-enhance-the-seo-of-your-react-app-6b2b95b07541?source=collection_archive---------3----------------------->

![](img/13e83f9cd636bacd4e45a86ef3cbd1cb.png)

谁说 ReactApps 不能 **SEO** 友好？今天，我们将看一看**react-头盔**，这是一个由 **NFL** 创建的开源库(我不知道他们也对开源感兴趣)，它可以帮助你管理网页中的变化

它是一个**组件**，你可以把它放在你的**应用**的任何地方，组件的子组件是你想要更新的**头部标签**，改善你的应用*的 **SEO** (不要等着成为谷歌的第一个就这么做)。*

首先，安装依赖项，只需在 React 应用程序的根目录下运行其中一个命令(我也将使用 react-router-dom)

```
***react helmet:***
yarn add react-helmet
***or***
npm i react-helmet***if you want react router:*** yarn add react-router-dom
***or***
npm i react-router-dom
```

所以，让我们假设你的应用程序由两个页面`Todos.js`和`Config.js` 以及主文件`App.js`组成，如下所示:

现在我们要更新每个页面上的标题标签:

让我们在标题上添加一个动态计数器:

现在你可以用元标签优化你不同的应用程序屏幕，比如`description`、`robots`、`title`、`canonical`、`content-type`、`viewport`、`icon`、`keywords`，你可以在这里阅读更多。

你可以使用我制作的这个模板，将数据更新到你的应用程序的每个屏幕。

我希望这对你有用，有更多的方法来提高你的搜索引擎优化，但你可以从这里开始，谢谢。