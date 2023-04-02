# 使用和不使用 React 挂钩构建相同的应用程序，第 1 部分

> 原文：<https://javascript.plainenglish.io/building-the-same-application-with-and-without-react-hooks-part-1-4bdcf02c9ab5?source=collection_archive---------4----------------------->

![](img/28d54d9c6074fbebe545f1118854e973.png)

Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

随着 React 钩子的发布，很多人可能会想，钩子到底有什么好处，甚至什么是钩子？

# 什么是钩子

钩子是允许您从函数组件中“挂钩”React 状态和生命周期方法的函数。一个“钩子”的例子是`useState`。`useState`是一个钩子，它允许我们在一个功能组件中利用有状态行为。我们将在下一节介绍这个内置钩子的一个使用示例。

# 为什么是钩子

钩子允许我们把所有的组件做成功能组件。这并不意味着类组件不再工作，它们可以与函数组件和钩子共存。

因为钩子允许函数组件中的有状态逻辑，所以我们不再需要跨组件管理和绑定`this`的值。这不仅使组件代码更容易阅读，也更容易编写。

## 让我们来看一个以基于标准类的方式编写的简单组件:

## 现在让我们将它与用两个简单的内置钩子编写的相同组件进行比较:

## 让我们比较一下

第二个组件仍然设法使用有状态逻辑，它仍然在组件挂载时获取，但是也设法不在任何地方使用单个`this`。钩子的这种实现是最基本的，没有定制的钩子或 reducers，但是代码已经很容易理解和编写了。

我们可以更进一步，使用`useReducer`将我们的有状态逻辑放在一个 reducer 钩子中。

# 用户教育

我们可以为我们的有状态逻辑定义一个定制的缩减器，并配置以使逻辑可重用。

然后我们可以使用 react 中的`useReducer`钩子导入这个定制的 reducer 函数。

通过从我们创建的这个 reducer 函数中导入我们的有状态逻辑，如果需要的话，我们可以在其他组件中重用我们的逻辑。一旦我们将其他钩子如`useContext`合并到我们的应用程序中，这将会派上用场。

# 定制挂钩

我们可以更进一步，通过创建一个定制的钩子来使我们的代码更加可重用。我们就叫它`useSushi`。

我们现在不仅重定位了我们的调度，还重定位了所有将我们的调度调用到我们的定制钩子中的有状态方法。现在，我们的 slim 应用程序组件看起来像:

您可以从上面的片段中看到，我们的应用程序组件已经变得非常小，并且更容易阅读。在我们的代码中找不到任何`this`的实例，我们的逻辑现在是可重用的。然而，仍有一些我们可以改进的地方。

在这个例子中，我们仍然使用我们的分派作为我们获取的一部分。如果我们想创建一个同步获取函数，我们可以绕过这个问题。与此同时，我们仍然将有状态逻辑作为道具传递给子组件。有一个内置的钩子来帮助我们解决这个问题，叫做`useContext`。

这个应用程序的这些改进将是这篇博客文章下一部分的重点，敬请关注！

## 额外资源

# 额外资源

*   [React Docs 简介钩子](https://reactjs.org/docs/hooks-intro.html)
*   [今明两天做出反应，90%的清洁工人用钩子做出反应](https://www.youtube.com/watch?v=dpw9EHDh2bM)
*   [Kent C. Dodds GDG 盐湖 DevFest 2018:为什么 React Hooks](https://www.youtube.com/watch?v=zWsZcBiwgVE&feature=youtu.be&list=PLV5CVI1eNcJgNqzNwcs4UKrlJdhfDjshf)