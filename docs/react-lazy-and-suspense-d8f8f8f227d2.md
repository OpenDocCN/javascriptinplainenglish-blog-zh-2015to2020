# 反应慵懒而悬疑

> 原文：<https://javascript.plainenglish.io/react-lazy-and-suspense-d8f8f8f227d2?source=collection_archive---------14----------------------->

用 JavaScript 框架开发应用程序是令人愉快的。但是，有许多组件需要考虑。比如**性能。**

![](img/2d84d91d677ceffa148baaed1bbb503e.png)

对于面向对象的语言来说，这比 JavaScript 框架要简单一些。因为语言本身可以帮助开发者。反应是不同的，我们必须考虑性能。想象一下，我们有一个 web 应用程序，它由 100 个组件组成。

我们将如何去考虑性能？用**懒**！

正如你可以看到上面的例子。有三个组件，分别是*欢迎车*、*用户车*和*车*。他们中的两个在用 react.lazy，那么有什么区别呢？汽车和用户会在我们需要的时候装载。但是对于欢迎来说就不一样了。它将立即加载。懒惰帮助我们提高代码性能。

## 悬疑呢？

悬念是从 v16.6 开始的实验性变化。它只需要一个参数。也就是**回退。**通过暂停，我们可以控制组件负载。直到组件完全加载。它将显示我们在回退中定义的消息。

我们可以在一个悬疑列表中收集所有围绕悬疑的组件。它协调组件的负载状态。

你可以在官方网站上看到暂停的详细信息

[](https://reactjs.org/docs/react-api.html#suspense) [## 反应顶级 API -反应

### React 是 React 库的入口点。如果从一个标签加载 React，这些顶级 API 可以在…

reactjs.org](https://reactjs.org/docs/react-api.html#suspense) 

感谢您的阅读！