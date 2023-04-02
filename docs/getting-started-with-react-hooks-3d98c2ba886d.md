# React 挂钩入门

> 原文：<https://javascript.plainenglish.io/getting-started-with-react-hooks-3d98c2ba886d?source=collection_archive---------4----------------------->

## React 中使用钩子的终极指南

![](img/45540cf1c9346cef0905100487cb8ba9.png)

React hooks 是在 *v16.8* 中引入的伟大特性，React hooks 增加了**在 React 功能组件**中管理状态和与生命周期事件交互的能力。它有助于更简单地开发 react 应用程序，减少冗长和复杂性。It *加强清晰的结构(容器和功能组件)和严格的数据流现在比以前更容易在我们的组件中创建合理的 UI 逻辑。*

如果我们想使用状态或生命周期方法，我们通常必须使用 *React 进行更改。组件*和类。相反，hooks 允许我们在功能组件中使用这些组件。

# 为什么挂钩:

React 挂钩是在我们的功能组件中访问状态和生命周期方法的方式，而无需编写类。使用 React 挂钩我们所知的功能，如道具钻取、渲染道具和高阶组件(HOC ),甚至减少不必要的冗余，所有这些都会导致代码难以编写、阅读和维护。少说两句，让我们深入代码示例。

# 挂钩类型:

这里我们将讨论常见的钩子类型

## 使用状态()

[https://gist.github.com/ishan-me](https://gist.github.com/ishan-me/65d9ebb35e34ca1d95cd092099572717)

第一个也是最重要的 react 钩子: [*useState*](https://reactjs.org/docs/hooks-state.html) 由要在顶部导入的 React 包公开。在代码中，我们可以注意到功能组件使用起来有多简单。导入 useState 后，我们将从 useState 中挑选一个包含两个变量的数组。传递给 useState 的参数是起始状态，即 DOM 中将要改变的数据。所以它返回两个绑定状态的实际值和更新值。

使用状态 [*对数组使用析构*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) 赋值，就像我们用*对象析构*拉道具一样。我们也可以在同一个函数中使用多个 useState 钩子。

## useRef()

[https://gist.github.com/ishan-me](https://gist.github.com/ishan-me/bea73a0bb6eeebc7fa4b0a2dc1bec6fc)

这个钩子将帮助我们允许直接访问 DOM 中的元素。`useRef`钩子让我们在功能组件内部创建可变变量。只有当组件在整个生命周期中被安装和保存时，才会创建这个挂钩。

*注意:更新 ref 是一个副作用，所以应该只在* `*useEffect*` */* `*useLayoutEffect*` *或者事件处理函数内部完成。*

## **useEffect()**

[https://gist.github.com/ishan-me](https://gist.github.com/ishan-me/201007d6fec5e3351b4c4f9f30f8dcd5)

这个钩子是添加和删除 DOM 侦听器的最佳位置。如果我们有一些高性能的数据获取、订阅或从 react 组件手动更改 DOM。我们将这些操作称为*“副作用”*，因为它们会影响其他组件，并且在渲染过程中无法完成。效果挂钩`useEffect`，增加了从功能组件执行副作用的能力。它的作用与 React 类中的`componentDidMount`、`componentDidUpdate`和`componentWillUnmount`相同，但是统一到一个 API 中。

## 一起使用 useState 和 useEffect:

[https://gist.github.com/ishan-me](https://gist.github.com/ishan-me/0c2082793cdb2cca55d7417a37fbf26c)

在代码中，我们同时使用了两个钩子。使用它们从 open API 端点获取数据没有问题。

## useReducer()

[https://gist.github.com/ishan-me](https://gist.github.com/ishan-me/983d934949a6258991811b0d7d7a0d90)

这个钩子用于管理应用程序的状态。这类似于 reducer 在幕后的工作方式。reducer 是一个纯粹的函数，它根据前一个状态和已经分派的动作计算下一个状态。同样，Reducer in [Redux](https://redux.js.org/) 它根据传递给它的动作返回下一个状态。

## 使用备忘录()

[https://gist.github.com/ishan-me](https://gist.github.com/ishan-me/611ed97be24619925ce7b1d97d52afbb)

在我们理解使用记忆挂钩之前，我们需要理解术语*记忆*。这是计算机科学中的一个行话，意思是缓存昂贵的函数调用的结果，并在参数相同时返回缓存的版本。

useMemo 帮助我们优化应用程序的性能。它有两个参数:一个内联函数和一个依赖数组。

> `React.useMemo(() => fn, deps)`

第一个参数 Inline function 从开销很大的计算中返回值- `() => doHeavyComputation(a, b)`。

第二个参数是一个依赖关系数组`[a, b]`，当其中一个依赖关系发生变化时，`useMemo()`会重新计算值，如果没有发生变化，则返回上一次存储的值。如果我们忘记传递依赖关系数组，那么每次组件渲染时都会计算新值。我们可以使用这些钩子进行昂贵计算，例如:过滤大型数组，递归函数，如阶乘、斐波那契等，避免不必要的子组件渲染。简单地说，它返回一个*记忆的*值。

该示例使用将在第一次渲染时运行的 *useMemo* 函数。它会阻塞线程，直到昂贵的函数完成，因为`useMemo`在渲染中运行。如果`listOfItems`没有改变，那么这些昂贵的函数就不会再次启动，我们仍然可以从它们那里得到返回值。它会让这些昂贵的功能看起来瞬间完成。

## useCallback()

[https://gist.github.com/ishan-me](https://gist.github.com/ishan-me/679b4e3e0fdb2b4cc2d4c9ff86562c3f)

它返回一个记忆化的回调。当我们有一个子组件经常重新呈现，并且我们向它传递一个回调时，这个钩子也很有用。`useCallback()`常与`useEffect()`连用，因为它可以防止函数的重新创建。

我们传递给`useCallback`钩子的函数只有在它的一个依赖项改变时才被重新创建。

## 使用上下文()或上下文应用编程接口

[https://gist.github.com/ishan-me](https://gist.github.com/ishan-me/258f53bcaf53d6544d888195b8683646)

这个钩子与上下文 API 结合使用。这样我们就可以得到当前的上下文值，它引用了最近的 *< MyContext。>供应商*组件。

React Context API 的主要目标是管理状态，而不使用道具在整个应用程序中传递数据。它还使我们能够避免使用 Redux，并简化代码结构。

# 结论

我希望本文在理解钩子的基础知识方面有所澄清。请找到 Github 的要点，并创造新的惊人的东西。关于钩子还有很多要学的。

为了了解更多关于 hooks 的信息，我建议您观看一年前由 [Sophie Alpert 在 React Conf 2018](https://www.youtube.com/watch?v=V-QO-KO90iQ) 上所做的演讲。

*———————————————快乐编码！—————————————————————*