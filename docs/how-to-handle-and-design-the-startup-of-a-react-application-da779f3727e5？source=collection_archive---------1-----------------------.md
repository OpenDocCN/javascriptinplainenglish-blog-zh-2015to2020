# 如何处理和设计 React 应用程序的启动

> 原文：<https://javascript.plainenglish.io/how-to-handle-and-design-the-startup-of-a-react-application-da779f3727e5?source=collection_archive---------1----------------------->

## 这也适用于 React Native！

![](img/7a6f8b0473c46f461dde1004a0e7c842.png)

在构建基于 react 的应用程序时，一旦用户到达，您可能希望在显示界面之前做一些事情:

*   加载/预加载远程内容或资源。
*   确保用户身份验证。
*   初始化上下文。
*   无论如何…

这很常见！有时候异步完成这个任务没有问题，但是通常你想阻止用户界面，并且在完成之前显示一个闪屏。

一些应用程序所有者还希望在显示应用程序内容之前，在同一个闪屏上显示公司徽标几秒钟。

今天，我想根据我在各种基于移动/独立 PWA 的应用程序中的经验，与您分享一种处理此类需求的方法。

大部分是在我在 [Kaliop](https://medium.com/u/bc4d66204c48?source=post_page-----da779f3727e5--------------------------------) 的技术领导角色范围内实现的。

# 为什么我应该在应用程序启动时显示加载屏幕而不是静态屏幕？

**静态加载屏幕经常会让用户误以为应用程序在启动时冻结了**。这真的不是你想要的！这就是为什么建议展示装载机。

加载屏幕应该在视觉上有趣，贯穿品牌元素，并向用户提供足够的加载进度反馈。

![](img/0cceeae3f3555086096080b8012f6eaf.png)

8tracks application

# 如何显示一个加载动画，并在长时间强制进程没有加载时阻止 UI 显示？

## 我们可以使用加载组件和重定向？

很长一段时间以来，我一直在做一个特定的加载组件，它单独安装，并在加载完成后使用导航界面重定向，虽然这在 react native 和`react-navigation`上工作得很好，但对于 web 应用程序来说，它变得复杂了，因为入口点是多个的，并且取决于 URL。

这就是为什么我强烈推荐下面的方法，它适用于任何情况。

## 我们应该构建一个包装组件，并使用状态管理策略。

该模式将使用一个包装组件，当一些进程在后台加载时，该组件将阻止 UI 并显示另一个加载组件。这是你的动画和标志将被使用的地方。

我们将把这个实现分成三个独立的部分:

1.  逻辑发生的`Apploader`组件。
2.  流程提供者组件，通常是主要的应用程序组件，我们称之为 **AppStartupOrchestrator。**
3.  并且加载组件显示您的动画，为了清楚起见，在那篇文章中将不处理这一部分。

💡*下面所有的组件都是使用* `*typescript*` *编写的，这是一个由* ***微软*** *提供的 Javascript 超集，它允许静态类型。在 Javascript 中，这非常类似，所以可以随意使用这样的工具将以下代码片段转换为 JS ES2015 代码:*[*http://www.typescriptlang.org/play/*](http://www.typescriptlang.org/play/)*。*

*另外，在你的编辑器中随意复制代码片段来突出语法。*

# 我们的应用程序加载过程的实现

## 主加载程序组件(Apploader.tsx)

该组件将接受一系列强制进程，并且只要每个进程没有准备好，就会显示我们的加载器动画。

我们的应用程序组件是该视图的子视图，在我们的示例中，我们将看到 App router 是第一个子视图。

AppLoader.tsx

现在一切都在加载，同时，您可以看到，只要每个进程没有处于就绪状态，应用程序就会显示我们的加载器闪屏。一旦发生变化，组件的子组件将被填充，应用程序将运行，从这一刻开始，可以肯定地说，每个强制流程数据都已定义并可访问。

**📦奖励:最短持续时间拦截器**

上面的代码片段附带了一个小功能，它将确保加载程序至少显示 XXXX 毫秒，有时你的应用程序加载太快，你会错过你可能想看到的加载状态。

## 流程编排器及其加载挂钩(**appstartuporchestrator . tsx+**usefavoritecountry . tsx**)**

对于加载任务，我建议使用一些钩子来返回某种空值和一个最终值，一旦动作加载被处理，这个最终值就不为空。

**🔗装载钩示例**

正如你在下面的例子中看到的，`favoriteCountry`将返回是否是`CountrySelectionStatus.NotChosen`、`CountrySelectionStatus.Loading`或者一个代表国家代码的字符串。

我们有 3 种状态可以用来定义我们的装载状态。

useFavoriteCountry.tsx

在这个例子中，我们使用了`to`库中的一个函数 [**await-to-js**](https://www.npmjs.com/package/await-to-js) 用来以一种更简单的方式处理来自 Promise 的错误。

**🎼管弦乐队(App.tsx)**

在我们的应用程序根级组件中，我们可以有这样的内容:

App.tsx

如您所见，我们正在使用上面刚刚创建的钩子，只要状态为正在加载，就绪就是`false`，就将其添加到加载进程列表中，但是一旦发生这种变化，`ready`就是`true`，这将触发显示我们的`AppLoader`子进程。此外，我们在生产中增加了 2 秒的最短持续时间，以防我们最喜欢的国家流程比这少。

## 🚀如何提高装载任务的性能？

考虑离线支持和加载性能优化的缓存策略。依靠缓存优先策略可以显著缩短应用程序加载最新数据的时间，同时从网络中收集新数据。

# 更进一步:

上面的模型非常简单但是有效，你可以通过添加可选的进程来在你的加载屏幕上显示一些指示器，你也可以处理错误消息显示，甚至是一个完成进度条。

想深入动画设计？让我们在 react 官方[博客查看一个不错的样本:https://Facebook . github . io/react-native/blog/2018/01/18/implementing-twitters-app-loading-animation-in-react-native](https://facebook.github.io/react-native/blog/2018/01/18/implementing-twitters-app-loading-animation-in-react-native)

![](img/2ccfa64d5a52af1e862347967c6fbaa3.png)

Twitter loading animation in react-native

[**🇫🇷STOP！你是法国人吗🥖？您也可以访问 ici 网站，接收法国的私人通讯🙂**](https://codingspark.io)