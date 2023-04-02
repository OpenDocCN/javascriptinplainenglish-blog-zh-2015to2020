# 使用 useHistory 挂钩导航 React 应用程序

> 原文：<https://javascript.plainenglish.io/navigating-your-react-app-with-the-usehistory-hook-c7c465bfc6f6?source=collection_archive---------2----------------------->

![](img/08436d3fbb347d7030e754f262393cb7.png)

Photo by [Mick Haupt](https://unsplash.com/@rocinante_11?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/navigation?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在我们开始之前，请记住，只有当您使用 React 16.8(2019 年 2 月发布)或更高版本，并试图在组件内部实现这一点时，`useHistory`钩子才会起作用。如果不是这样，请看我的另一篇文章，[使用“历史”从组件外部导航 React 应用程序](https://medium.com/@bcantello/using-history-to-navigate-your-react-app-from-outside-a-component-40ea74ba4402)。

如果你在 React 中做过任何编程，你可能对使用`<Switch>`、`<Route>`和`<Link>`的基本路由很熟悉。为此，我们从`react-router-dom`导入`BrowserRouter`、`HashRouter`、`MemoryRouter`或`NativeRouter`，然后在`index.js`中将`<App />`组件包装在我们选择的路由器中。这看起来会像这样:

现在，我们可以在应用程序中的任何地方自由使用上述标记。然而，您可能不知道的是，每当我们使用上面列出的任何路由器时，React 也会创建一个`history`对象供我们使用，并在默认的 props 中传递该对象。例如，假设没有向登录组件传递任何属性。登录仍然可以接受默认属性，并以这种方式利用历史对象:

我们显然希望在其中加入一些额外的逻辑，但关键是我们可以通过`props`访问历史对象，即使没有向`Login`传递任何属性，并使用`history.push`将新条目推送到历史堆栈中。基于上面的例子，这将把用户路由到与`/home` url 相关联的组件。那很简单！

> *但是如果我们真的将道具传递给* `*Login*` *，从而覆盖了包含历史对象的默认道具，会发生什么呢？或者尝试从组件外部访问历史对象呢？*

在这些情况下，上述方法不起作用。如果你试图在组件**中使用来自**的历史，并且你正在使用 React 16.8 或更高版本**，那么你可以使用`useHistory`钩子！我将在本文中介绍如何做到这一点。但是，如果您试图从组件外部使用历史，或者您正在使用 React 的旧版本，您将需要创建自己的历史对象。关于如何做到这一点的文章可以在[这里](https://medium.com/@bcantello/using-history-to-navigate-your-react-app-from-outside-a-component-40ea74ba4402)找到。**

React Router 附带了`useHistory`钩子，它允许我们访问路由器的状态，以便从组件内部进行导航。钩子必须在组件内部使用。这就是他们的工作方式。只要您符合这个标准，您就可以导入`useHistory`钩子并使用它来导航，如下例所示。

就是这样！在这个例子中，我们使用一个文字按钮将`/home`推入历史堆栈。你不必使用按钮或者让用户直接与你的应用程序交互。也许您有一个登录页面，并希望在预定的时间后自动转到主页。将`setTimeout`与历史结合起来！

历史对象中通常包含许多其他属性。在本例中，我们使用了`history.push`，但是您也可以使用:

*   `length` -(数字)历史堆栈中条目的数量
*   `action` -(字符串)当前动作(`PUSH`、`REPLACE`或`POP`)
*   `push(path, [state])` -(函数)将新条目推入历史堆栈
*   `replace(path, [state])` -(函数)替换历史堆栈中的当前条目
*   `go(n)` -(函数)通过`n`条目移动历史堆栈中的指针
*   `goBack()` -(功能)相当于`go(-1)`
*   `goForward()` -(功能)相当于`go(1)`
*   `block(prompt)` -(功能)阻止导航

这个列表并不详尽，所以我鼓励你自己更深入地挖掘一下`history`！如果这种方法不适合您，请确保您使用的是 React 16.8 或更高版本，并且在组件内部。如果没有，请看我的另一篇关于使用“历史”从组件外部导航 React 应用的文章。