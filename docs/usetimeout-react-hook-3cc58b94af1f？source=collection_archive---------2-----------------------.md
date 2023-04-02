# useTimeout() React 挂钩

> 原文：<https://javascript.plainenglish.io/usetimeout-react-hook-3cc58b94af1f?source=collection_archive---------2----------------------->

![](img/abb644c4162e37dc6d7e9ba59f52d669.png)

Running out of time! Photo by [Brad Neathery](https://unsplash.com/@bradneathery?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/time?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在 React 中处理时间效果很难。例如，如果 React 组件生命周期使用不当，构建倒计时组件就非常容易出错([，但是如果你有一个使用时间钩子](https://medium.com/javascript-in-plain-english/usetime-react-hook-f57979338de)，这就很容易了！).

将延迟回调集成到 React 生命周期中也存在同样的困难。我们将在 React 钩子中包装 setTimeout。在本文中，我们将讨论:

*   在 React 钩子中处理回调
*   效果清理
*   取消效果

# 代码优先！

钩子、Jest 测试和一个示例组件可以在 GitHub repo:

[](https://github.com/jamesfulford/testing-react-hooks/tree/master/src/hooks/useTimeout) [## jamesfulford/测试-反应-挂钩

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/jamesfulford/testing-react-hooks/tree/master/src/hooks/useTimeout) 

提供一个回调和一个超时值(以毫秒为单位)，钩子将启动一个`setTimeout`并返回一个可以取消超时的函数。

有趣的事实是，如果你想知道:在一个已经被清除的 ID 上调用 clearTimeout 是不可行的。

# 挂钩依赖和效果清理

使用这个钩子最棘手的部分是在指定回调的时候。如果在重新渲染期间，传递给`useTimeout`的回调被重新定义(即使实现是相同的)，那么效果清理被触发。这将导致超时被取消，新的超时将被排队。这可能不是您想要的行为。

为了控制重新渲染重排行为，您应该[使用 React 的 useCallback](https://reactjs.org/docs/hooks-reference.html#usecallback) 并在那里管理依赖关系。当回调的依赖关系改变时，回调将改变并触发超时清理/重新渲染。

说到正确管理 Rect 钩子依赖，我建议使用 [exhaustive-deps eslint 规则](https://github.com/facebook/react/issues/14920)。

# 示例用法

我用这个钩子创建了一个快速堆栈来展示一个示例组件。[https://stackblitz.com/edit/react-use-timeout?file=InterruptingCow.jsx](https://stackblitz.com/edit/react-use-timeout?file=InterruptingCow.jsx)

该组件展示了钩子的重新呈现行为，使用和不使用`useCallback`方法。它还展示了取消回调。请务必打开控制台并阅读评论！

此外，请查看 GitHub repo 中的单元测试:它们非常清楚地说明了钩子是如何工作的。

[](https://github.com/jamesfulford/testing-react-hooks/blob/master/src/hooks/useTimeout/index.test.ts) [## jamesfulford/测试-反应-挂钩

### 您现在不能执行该操作。您使用另一个选项卡或窗口登录。您在另一个选项卡上注销，或者…

github.com](https://github.com/jamesfulford/testing-react-hooks/blob/master/src/hooks/useTimeout/index.test.ts) 

# 相关文章

有兴趣了解更多关于 React Hooks 的信息吗？请真实地阅读您最近的文章:

[](https://medium.com/javascript-in-plain-english/usetime-react-hook-f57979338de) [## 使用时间()反应挂钩

### 反应中的倒数很复杂/很难。让我们用钩子。

medium.com](https://medium.com/javascript-in-plain-english/usetime-react-hook-f57979338de)