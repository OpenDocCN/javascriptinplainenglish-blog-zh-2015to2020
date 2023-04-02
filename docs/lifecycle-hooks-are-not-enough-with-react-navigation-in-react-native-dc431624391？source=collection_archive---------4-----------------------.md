# 生命周期挂钩对于 React Native 中的 React 导航是不够的

> 原文：<https://javascript.plainenglish.io/lifecycle-hooks-are-not-enough-with-react-navigation-in-react-native-dc431624391?source=collection_archive---------4----------------------->

## 也使用 React 导航库提供的这些事件和 API

![](img/a9176e3e7e0924523c7ce6420d438aa5.png)

React Navigation-Photo by [Dan Chung](https://unsplash.com/@dannayyyboi?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/navigation?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

React 生命周期挂钩帮助我们检测呈现 UI 的各个阶段。在 web 中，我们经常使用`useEffect` hook 在渲染、改变指定变量和卸载时执行动作。然而，在 React Native 中使用 React 导航识别屏幕转换时，这还不够。

我前段时间就面临这样的情况。我正在做的项目有一个`Profile`屏幕和一个`Settings`屏幕。可以在`Settings`屏幕上更新个人资料详情。然而，当从`Settings`屏幕导航回`Profile`屏幕时，我仍然看到旧的过时信息，即使它已被设置为在渲染时获取配置文件数据。

发生这种情况是因为 React 导航处理 UI(屏幕)呈现的方式与 React 处理 web 上呈现的方式有很大不同。下面是解释。

*   当我们第一次导航到 Profile 屏幕时，它被推入导航堆栈。
*   然后我们导航到设置屏幕，导致设置屏幕也推入堆栈。由于配置文件屏幕没有从堆栈中弹出，所以它仍然是挂载的。
*   更新个人资料信息后，我们按“后退”按钮再次导航到个人资料屏幕。由于配置文件屏幕一直保持安装状态，为了导航到配置文件，设置被弹出堆栈，因此被卸载。
*   但是，当导航到配置文件屏幕时，不会调用任何挂载方法，因为它已经被挂载了。因此，配置文件屏幕不执行任何数据提取，仍然向我们显示旧信息。

因此，我们需要使用替代品来避免这种行为。

有了 [React Navigation](https://reactnavigation.org/) 提供的解决方案，克服这种困惑并不困难。

## 聆听聚焦和模糊事件

React Navigation 为我们提供了两个事件，`focus`和`blur`来分别检测进入一个新的屏幕和离开一个屏幕。使用这些方法，您可以成功地调用您的`useEffect`中的业务逻辑。

现在我们正在收听`useEffect`中的`Focus`事件。每次聚焦配置文件屏幕时，即配置文件屏幕可见时，会发生 onFocus 事件，并获取更新的配置文件数据。

当卸载配置文件屏幕时，我们应该停止监听事件。这就是为什么我们通过在`useEffect`中返回事件监听器来移除事件监听器。

我们也可以在屏幕失去焦点时触发一个功能。您可以通过监听`Blur`事件来做到这一点，就像我们在上面的代码中监听`Focus`事件一样。

我们可以用 React 导航的`useFocusEffect`钩子做同样的事情，如下所示。

## 检查屏幕是否聚焦

React 导航还提供了`useIsFocused`钩子来检查屏幕是否被聚焦。

在本文中，我们讨论了如何使用焦点和模糊事件侦听器来避免 React Native 中组件/屏幕呈现的一些混淆问题，使用 React 导航。

感谢阅读…

**资源:**

 [## 反应导航

### 在上一节中，我们使用了具有两个屏幕(主页和详细信息)的堆栈导航器，并学习了如何使用…

reactnavigation.org](https://reactnavigation.org/docs/navigation-lifecycle) 

# **简明英语笔记**

你知道我们推出了一个 YouTube 频道吗？我们制作的每个视频都旨在教给你一些新的东西。点击此处 查看我们，并确保订阅该频道😎