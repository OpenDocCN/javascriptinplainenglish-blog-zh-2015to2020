# 如何编写自定义的 React 挂钩

> 原文：<https://javascript.plainenglish.io/how-to-write-a-custom-react-hook-6a8315f351f6?source=collection_archive---------2----------------------->

## 关于如何将组件逻辑提取到可重用钩子中的一课。

React 钩子，在 [React v16.8](https://reactjs.org/blog/2019/02/06/react-v16.8.0.html) 中发布，改变了开发者编写代码的方式。默认情况下，React 让我们可以访问一组[强大的基础钩子](https://reactjs.org/docs/hooks-reference.html)，比如`useState`、`useEffect`、`useReducer`等等，但是我们也可以构建自己的定制钩子来抽象复杂的状态逻辑。

![](img/08d533300b9b5a24bce32e5bbb56a1a2.png)

Photo by [v2osk](https://unsplash.com/@v2osk?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 等等，什么是定制挂钩？

在编写 React 应用程序时，您肯定会发现自己在多个组件中使用相同的重复或冗余的状态逻辑。通过定制钩子，我们可以将这种逻辑提取到一个函数中，使我们的代码更干净，更具可重用性。

自定义钩子只是包含其他钩子的函数，并且包含一个公共的有状态逻辑，可以在多个组件中重用。这些功能的前缀是`use`。自定义钩子意味着更少的击键和代码。

在您编写自己的定制钩子之前，请记住开源社区已经发布了成千上万个钩子，所以很有可能有人已经编写了您需要的逻辑并在线发布了。

然而，这与我们无关，本文将关注如何编写定制钩子，而不是您是否应该编写它们。让我们直接进入第一个例子。

# 使用位置

让我们来看看这个计数器组件，它在本地存储中存储其当前的`count`值:

这里，我们使用`useState`和`useEffect`将本地状态同步到本地存储。现在，如果我们想在多个组件之间复制这个逻辑，该怎么办呢？我们将创建一个新的定制钩子来处理这个逻辑，而不是复制和粘贴代码。

`useLocalStorageState`接受两个参数，键和默认值。在第一次初始化时，我们从本地存储器获取`count`(如果它存在)并将其设置为 state，否则我们使用默认值。然后，我们使用`useEffect`钩子来保持我们的本地存储与本地状态同步，并将我们的`state`和`setState`函数作为数组返回。注意我们是如何使用`JSON.stringify`和`JSON.parse`字符串化和解析 localStorage 中的值的，这意味着我们也可以用这个钩子存储对象！

现在我们可以在任何我们想要的组件上重用这个`useLocalStorageState`钩子！

# 使用阵列

注意，我们并不总是必须从自定义钩子返回一个数组。在这个例子中，我们将构建一个定制的`useArray`钩子，它允许我们更容易地管理数组状态。

这个钩子非常直观。我们返回一个对象，该对象带有一组数组状态的修饰符，这使得我们可以轻松地操作数组。我们接受一个初始数组作为钩子参数，然后提供`add`、`clear`、`removeById`和`removeIndex`作为通常的`value`和`setValue`操作之外的附加函数。

让我们看看如何在组件中实现这个钩子:

看看我们是如何通过使用一个定制的钩子来大大减少我们在这个组件中需要的无关逻辑的数量的吧？相当不可思议。

# 我们必须以`use`开始我们的定制钩子吗？

严格来说不是，但实际上是。根据[反应文件](https://reactjs.org/docs/hooks-custom.html):*请执行。这个惯例非常重要。如果没有它，我们将无法自动检查是否违反了钩子* *的* [*规则，因为我们无法判断某个函数内部是否包含对钩子的调用。”*](https://reactjs.org/docs/hooks-rules.html)

# 自定义挂钩不共享状态

自定义钩子的每个实例都有自己的状态，所以不幸的是，默认情况下，您不能与自定义钩子共享状态。然而，如果你将一个定制钩子与一个全局状态库如 [Redux](https://redux.js.org/) 或[反冲](http://recoiljs.org/)配对，你可以构建与全局状态交互的定制钩子！

# 结论

React 定制钩子对于编写更干净、更易维护、更简洁的代码来说非常强大。我们看了几个很好的定制钩子的例子，`usLocalStorageState`和`useArray`，以及我们如何使用它们来降低代码复杂度和增加可重用性。有一些你用过或做过的很棒的定制反应钩吗？我很想看看他们，欢迎在下面评论！

## 保持联络

有很多内容，我很感谢你读我的。我是加州大学伯克利分校 MET 项目的本科生，也是一名年轻的企业家。我写软件开发、创业和失败(这是我非常擅长的)。你可以在这里[注册我的时事通讯](https://newsletter.cometcode.io/)或者在我的[网站](https://www.caelinsutch.com/)查看我正在做的事情。

请随时联系我，在 Linkedin[或 Twitter](https://www.linkedin.com/in/caelinsutch)[上联系我，我喜欢听到阅读我文章的人的声音:)](https://twitter.com/caelin_sutch)