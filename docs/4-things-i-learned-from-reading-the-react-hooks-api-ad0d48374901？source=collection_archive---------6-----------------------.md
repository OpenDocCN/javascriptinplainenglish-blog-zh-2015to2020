# 我从阅读 React Hooks API 中学到的 4 件事

> 原文：<https://javascript.plainenglish.io/4-things-i-learned-from-reading-the-react-hooks-api-ad0d48374901?source=collection_archive---------6----------------------->

## 让你的 React Hooks 知识更上一层楼

![](img/9d847311746913105c1aeb478aa88f90.png)

Photo by [Dave Phillips](https://unsplash.com/@teracomp?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我已经在一些项目中使用了 React 钩子，并且对基本钩子有了相当好的理解，比如 useState、useEffect 和 useContext。我希望将我的知识提升到一个新的水平，并在即将到来的项目中应用更高级的钩子，所以我决定通读一下[钩子 API 参考](https://reactjs.org/docs/hooks-reference.html)。

我对 React 的编程和使用还很陌生，但是我觉得 React 文档写得很好，很容易理解。下面是我通读 API 参考后学到的 4 件事。请注意，本文假设您对 React 有基本的了解。

# 首先，什么是 React 钩子？

React 挂钩仍然是新的，在 2019 年 2 月 React 16.8 发布时发布。钩子允许您在功能组件中使用状态和其他 React 特性。钩子是 JavaScript 函数，有两个特殊的规则:

1.  **只调用顶层的钩子** —钩子不应该在循环、条件或嵌套函数中调用。如果你需要运行一个条件检查，你可以把它放在钩子函数里面。
2.  **只从 React 函数中调用钩子** —钩子不应该从普通的 JavaScript 函数中调用。相反，从一个 React 功能组件调用它们或者创建一个自定义钩子。我们将在后面的文章中研究如何做到这一点。保持警惕！

# 1.清理使用效果挂钩

清理一个 [useEffect](https://reactjs.org/docs/hooks-reference.html#useeffect) 钩子与在一个基于类的组件中调用 componentWillUnmount 具有相同的功能。这个函数将在组件从 DOM 中移除之前被调用。我们可能使用它的一些例子是删除事件侦听器、使计时器无效、取消网络请求或清除在 componentDidMount 中创建的任何订阅，或者在我们的例子中是在 useEffect 运行时。

要清理 useEffect 挂钩，我们需要返回一个函数。该函数将在需要清理时运行。因为 useEffect 为每个渲染运行，所以在下次运行效果之前，清理功能也将清理以前渲染的效果。

```
useEffect(() => {
  // do something return () => {
    // clean up 
  };
}, []);
```

另外注意，useEffect 函数可以接受第二个参数，这是一个空数组，如上所示。传递到数组中的任何值都将决定钩子何时运行。如果您传递一个空数组作为第二个参数，那么钩子将只运行一次，在初始渲染时，

# 2.何时使用 useReducer

[useReducer](https://reactjs.org/docs/hooks-reference.html#usereducer) 是 useState 的替代。useReducer 通常会用在有复杂状态逻辑或者你的下一个状态依赖于你的当前状态的时候。useReducer 的逻辑和设置类似于 Redux 的工作方式，使用动作、Reducer、分派和状态。

对 useReducer 的更多了解让我思考 Hooks 的 useContext 和 useReducer 是否可以取代 React 应用中的 Redux。下面的文章帮助我更好地理解了这个话题。

**TL；博士**

*   将 useState 用于基本和简单/小型应用程序。
*   对于高级/中型应用程序，使用 useState + useReducer + useContext。
*   对复杂/大型应用程序使用 useState/useReducer + Redux。

[](https://www.robinwieruch.de/redux-vs-usereducer) [## React 的 useReducer Hook vs Redux - RWieruch

### 由于 React 钩子已经被释放，函数组件可以使用状态和副作用。有两个钩子是…

www.robinwieruch.de](https://www.robinwieruch.de/redux-vs-usereducer) 

# 3.什么是记忆化？

我不确定什么是记忆，所以在遇到[使用备忘录](https://reactjs.org/docs/hooks-reference.html#usememo)和[使用回调](https://reactjs.org/docs/hooks-reference.html#usecallback)挂钩后，它引导我了解更多。对于那些不熟悉它的人来说，[记忆化](https://en.wikipedia.org/wiki/Memoization)是一种性能优化技术，用于帮助加速计算机程序。它通过存储函数调用的结果并返回缓存的结果来实现这一点。

请记住，useMemo 和 useCallback 函数应该只用于昂贵的计算。对于简单的任务，它实际上会降低性能，并产生其他副作用。如果你想了解更多，这里有一篇我读过的有趣的文章。

[](https://kentcdodds.com/blog/usememo-and-usecallback) [## 何时使用备忘录和使用回调

### 性能优化总是有代价的，但并不总是有好处的。让我们谈谈成本和…

kentcdodds.com](https://kentcdodds.com/blog/usememo-and-usecallback) 

# 4.用 useRef 钩子管理焦点

[useRef](https://reactjs.org/docs/hooks-reference.html#useref) 挂钩有两个主要用途；访问 DOM 节点或跟踪可变变量。如果您熟悉基于类的组件，这类似于 createRef。这将派上用场的一个例子是管理焦点、文本选择或媒体播放。这可以用 useRef 钩子来完成。在下面的例子中，我们创建了一个对搜索输入的引用，并将焦点设置在按钮点击上。

```
function App() {
  const searchInput = useRef(null); const focusSearchInput = () => searchInput.current.focus(); return (
    <div>
      <input ref={searchInput} type="text" />
      <button onClick={focusSearchInput}>Focus Search Input</button>
    </div>
  );
}
```

感谢阅读！浏览文档总是学习和提高技能的好方法。如果您正在学习 React 或任何新的语言、框架或库，我建议您花些时间通读文档。总有新的东西你可以学习！

在我的下一篇文章中，我将看看如何用 React 构建定制钩子。

如果您刚刚开始学习 React，这里有一些我在旅程中发现有用的参考资料。祝你好运，编码快乐！

**官方 React 文档**——[教程:React 简介](https://reactjs.org/tutorial/tutorial.html)

**Udemy** — [现代与 Redux【2020 更新】](https://www.udemy.com/course/react-redux/)

**Youtube**——[完成 React 教程(带 Redux)](https://www.youtube.com/playlist?list=PL4cUxeGkcC9ij8CfkAY2RAGb-tmkNwQHG)