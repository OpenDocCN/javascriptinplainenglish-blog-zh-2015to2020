# React.useMemo 以及何时应该使用它

> 原文：<https://javascript.plainenglish.io/react-usememo-and-when-you-should-use-it-e69a106bbb02?source=collection_archive---------1----------------------->

![](img/93bfda55bc1de63f1d8039c566eaa815.png)

随着应用程序的扩展，性能问题变得越来越明显。尽管 React 经过了很好的优化，开箱即用，但了解它提供的工具以使您的代码更快是很重要的。其中一种工具是`React.useMemo`钩子和它的助手`React.useCallback`。

# useMemo 解决什么问题？

`useMemo`是一个记住函数输出的 React 钩子。就是这样。useMemo 接受两个参数:一个函数和一个依赖列表。`useMemo`会调用函数并返回其返回值。然后，每次你再次调用`useMemo`时，它会首先检查是否有任何依赖关系发生了变化。如果不是，它将返回缓存的返回值，而不是调用函数。如果它们已经改变，`useMemo`将再次调用提供的函数，并重复该过程。

这应该让您想起了`useEffect`钩子:`useMemo`和`useEffect`都接受依赖列表。唯一的区别是`useEffect`是针对副作用的(因此得名)，而 useMemo 中的函数应该是纯净的，没有副作用。

# 你应该什么时候使用它？

首先，重要的是要注意你的代码不能依赖于`useMemo`。换句话说，您应该能够用直接的函数调用替换`useMemo`调用，而不改变应用程序的任何行为，除了性能。最简单的方法是先写没有`useMemo`的代码，然后根据需要添加。

要了解`useMemo`以及何时应该使用它，请看这个例子。首先，看看这段没有`useMemo`的代码:

这个小程序可以让你输入你的名字和号码。然后它会问候你并显示斐波纳契数列的数字。如果您运行它，您会注意到如果我们更改名称或数字,`NameDisplay`组件和`FibDisplay`组件都将重新呈现(并运行昂贵的计算)。这是不可接受的，解决方法如下:

首先，注意`FibDisplay`中`useMemo`的使用。我们将昂贵的计算封装在一个函数中，该函数仅在`length`改变时运行。组件仍将重新呈现，但除非需要，否则昂贵的计算不会运行。

其次，注意到`NameDisplay`组件用`React.memo`包装。`React.memo`是记忆整个构件的一种方法。只有当道具改变时，它才会重新渲染，从而彻底解决我们的问题。

# 但是不要过度使用它

虽然优化性能是一项崇高的追求，但您应该始终考虑这样做的含义和副作用。在 React.useMemo 的情况下，有几个:

1.  **开销**。挂钩本身引入了新的复杂逻辑，并且它可能引入比它解决的更多的性能问题。除非这是一个非常昂贵的计算，否则不要使用 useMemo，或者，如果您不确定，您可以对两种方法进行基准测试并做出明智的决定。
2.  没有担保。根据[反应文件](https://reactjs.org/docs/hooks-reference.html#usememo)，你可能永远不会依赖`useMemo`上的内部机制。换句话说，虽然`useMemo`应该只在依赖关系改变时被调用，但这是没有保证的。如果`useMemo`在每次渲染时都调用你的回调函数，你的应用程序一定会运行得很好(虽然可能有点慢)。

考虑到 React.memo，所有这些问题同样适用。但是还有一点:`React.memo`应该只适用于[纯组件](https://medium.com/better-programming/working-with-react-pure-components-166ded26ae48)。另一个问题与 Redux/Context 和钩子有关。在钩子出现之前，Redux 选择器通过 props 传递存储值，而`React.memo`会捕获它们。然而，如果你使用的是`useSelector/useContext`，当这些改变时`React.memo`不会重新呈现你的组件。由于这些复杂性，我建议不要使用`React.memo`，因为`useMemo`在大多数情况下应该足够了。

# 奖励:使用回调

我注意到很多人对`useCallback`感到困惑。`useCallback`与 useMemo 非常相似，只是功能不同。其实`useCallback(fn, deps)`相当于`useMemo(() => fn, deps)`。`useCallback`在使用 lof 回调函数时，除非依赖关系改变，否则不重新声明它们，这样可以加速应用程序。这是我们之前使用`useCallback`的例子:

感谢您的阅读，希望您喜欢。请在评论中告诉我你对`useMemo`的看法，并查看我的其他文章:

[](https://medium.com/javascript-in-plain-english/10-javascript-interview-questions-for-2020-697b40de9480) [## 2020 年 10 个 JavaScript 面试问题

### 随着对 JS 开发人员需求的增长，你必须做好准备。这里有 10 个问题可以帮助你…

medium.com](https://medium.com/javascript-in-plain-english/10-javascript-interview-questions-for-2020-697b40de9480) [](https://medium.com/javascript-in-plain-english/7-really-good-reasons-not-to-use-typescript-166af597c466) [## 不使用 TypeScript 的 7 个非常好的理由

### 有很多理由使用 TypeScript，但我会给你 7 个不使用的理由。

medium.com](https://medium.com/javascript-in-plain-english/7-really-good-reasons-not-to-use-typescript-166af597c466) [](https://medium.com/javascript-in-plain-english/to-infinity-and-beyond-with-javascript-proxy-api-8d4f7a26c8dc) [## 使用 JavaScript 代理 API 无限超越

### 代理 API 是一个高级的概念，但是如果你想掌握 JavaScript，代理 API 是你绝对需要的…

medium.com](https://medium.com/javascript-in-plain-english/to-infinity-and-beyond-with-javascript-proxy-api-8d4f7a26c8dc) [![](img/446049aa060bbaea15a64e1a907b1030.png)](http://eepurl.com/gYiA29)