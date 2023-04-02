# 如何使用 useReducer React 挂钩

> 原文：<https://javascript.plainenglish.io/how-to-use-the-usereducer-react-hook-722c2d511a3b?source=collection_archive---------18----------------------->

当 React 挂钩在 [React v16](https://reactjs.org/blog/2019/02/06/react-v16.8.0.html) 中引入时，开发人员利用这个机会修改了[功能组件](https://reactjs.org/docs/components-and-props.html)中的 React 状态。一大堆钩子被创造出来，比如`useEffect`、`useState`和[其他](https://reactjs.org/docs/hooks-intro.html)。今天，我们将深入了解`[useReducer](https://reactjs.org/docs/hooks-reference.html#usereducer)`，看看我们如何使用它来编写更好的 React 代码。

![](img/d8de207308af5d7eec3f49d20769d821.png)

Photo by [Caspar Camille Rubin](https://unsplash.com/@casparrubin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了简单起见，我们将看一个非常简单的 React 组件，以及我们如何实现`useReducer`来跟踪状态。

在其核心，`useReducer`是一个管理组件状态的钩子，类似于`useState`，但是更强大。

不同的是，在`useState`中，你调用一个 updater 函数来修改状态:

```
const [state, setState] = useState(0)setState(p => p++)
```

这是实现本地状态管理的一个超级简单而强大的方法。然而，一旦我们开始处理更复杂的状态，我们就开始遇到一些问题。这些`setState`函数调用开始变得非常庞大和笨拙。这就是`useReducer`的用武之地。

不是使用函数来修改状态，而是通过传入消息来改变状态。

如果你熟悉 [Redux](https://redux.js.org/) ，`useReducer`也有非常相似的行为。“缩减器”是一个纯粹的函数，它基于前一个状态和一些分派的动作来计算下一个状态:

```
(currentState, action) => newState
```

“纯函数”是一种接受输入并返回输出而不修改输入的函数(更多信息请阅读此处的[和](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-pure-function-d1c076bec976))。这意味着缩减器返回一个全新的状态，而不仅仅是先前状态的修改形式。

减压器有几个关键规则。减速器:

1.  永远不要改变他们的参数(不修改当前状态)
2.  从不产生副作用(没有外部函数调用或 API 请求)
3.  不要调用非纯函数，这些函数根据输入以外的任何东西改变输出(即不要使用`Date.now()`或任何变化的值)

通过遵循这些规则，你将总是能写出易于测试和使用的 reducers，这将在调试或重构代码时帮助你。

Reducers 帮助我们集中状态管理，允许组件发送消息来修改状态和改变组件中更复杂的状态。

让我们来看一个非常简单的反例，看看我们在现实生活中如何实现 reducer:

这段代码有几个关键部分。首先是减速器，这是一个接受一个`state`和一个`action`的函数。您可以看到，我们使用了一个 switch 语句，根据正在使用的操作返回不同的状态。

在组件中，我们可以看到如何调用`dispatch`来调用状态动作，在本例中是递增或递减计数。

虽然这是一个很好的基本例子来看看它是如何工作的，但是一旦我们有了更复杂的状态，可能是一系列嵌套的对象，并且每个动作每次只修改该状态的一小部分，那么`useReducer`的真正威力就会显现出来。

另一个优势是`useReducer`允许我们在应用程序中重用 reducers，你可以定义一个单一的状态行为，并在应用程序的多个地方复制它，这是一个防止重复代码的好方法。

# 结论

这是很多信息，所以让我们回顾一下我们所学的内容。

*   `useReducer`是一个 React 钩子，通过使用“actions”和“reducers”来修改局部状态
*   我们使用一个纯函数，它接受一个先前的状态和一个动作(或一个键)，我们称之为“缩减器”
*   `useReducer`最适合在处理复杂状态时使用

我希望这篇文章能帮助你更好地理解如何使用`useReducer`来编写更干净的 React 代码！

## 保持联络

有很多内容，我很感谢你读我的。我是加州大学伯克利分校的一名学生，也是一名年轻的企业家，我写关于软件开发以及我经营和发展公司的经历。你可以在这里注册我的简讯或者在我的[网站](https://www.caelinsutch.com/)查看我正在做的事情。请随时联系我，在 Linkedin 或 Twitter 上联系我，我喜欢听到阅读我作品的人的声音:)