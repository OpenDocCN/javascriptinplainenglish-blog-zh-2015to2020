# 2020 年反应状态管理

> 原文：<https://javascript.plainenglish.io/react-state-management-in-2020-4186766426da?source=collection_archive---------3----------------------->

![](img/8f8cffc0aae70172edc255f6f12d6140.png)

Photo by [Martin Sanchez](https://unsplash.com/@martinsanchez?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/stack?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

*如果您正在使用 React 或 React Native 构建 web 或移动应用程序，您可能知道管理应用程序的状态是关键，如果有数百个甚至数千个组件，它会很快变成一个复杂的难题。那么目前使用的不同方法是什么，它们的优缺点是什么？*

# **老类组件方式**

React 有两种类型的组件:类和函数。函数组件最初是“哑”的，不包含任何状态，而状态是存储在类组件中的。状态值在`state`对象中初始化，并通过`this.setState()`进行更新，后者把要更新的值作为参数，例如`this.setState({ counter: 1 })`会把名为 counter 的状态变量的值设置为 1。

语法遵循 javascript 类的规则，但是在 React 组件的上下文中，它很快变得相当沉重:到处都有许多`this.`和`this.state`。

因此，它逐渐被抛弃，转而支持函数组件中使用的 React 挂钩。

# 新反应钩道

在 2018 年 10 月的 [React Conf](https://www.youtube.com/watch?v=dpw9EHDh2bM) 期间推出，承诺让我们的组件逻辑更清晰，通过功能组件处理状态和生命周期方法。

我们使用钩子`useState`而不是将所有状态变量存储在同一个对象中，例如:

`const [counter, setCounter] = useState(0)`

我们声明了一个变量`counter`，提供了一个初始值`0`，还声明了一个特定的 setter。对于改变计数器的值，语句`setCounter(1)`就足够了，不需要更多的`this.state`，我们只调用`counter`。

语法上的这一重大改进使得 React 挂钩流行起来，构建类组件的需求也逐渐减少。

状态很容易通过 props 沿着组件树向下移动，但是反过来需要一些状态提升。

# 提升状态

在 React 中，当数据通过 props 传递时，它总是沿着组件树向下流动，但是由于我们经常需要将数据向上移动到组件，React 应用程序经常需要所谓的“状态提升”。对于类组件和钩子来说都是如此。

要解除状态:

*   函数在父组件中声明或赋值
*   它通过 props 传递给子组件(或更高)
*   该函数的执行导致状态更新
*   当该函数沿树向下执行时，它可以将数据作为参数，然后将其提升到父组件状态

下面是一个非常简单的例子，使用 React 钩子:

```
// This component executes the function that results in state updateconst ChildComponent = ({incrementCounter}) => { return (
      <React.Fragment>
         <Button onClick={incrementCounter}
      </React.Fragment>
   )
}// This component receives the stateconst ParentComponent = () => {
   const [counter, setCounter] = useState(0)
   const incrementCounter = () => {
      setCounter(counter+1)
   } return (
      <React.Fragment>
         <p>Counter is at value: {counter}</p>
         <ChildComponent incrementCounter={incrementCounter}/>
      </React.Fragment>
   )
};
```

这里子组件没有给出数据，但是状态更新的动作是通过按钮触发的。如果我们直接传递`setCounter`,我们也可以用来自子组件的任何值来填充它，它将再次被提升到父组件。

我不会深入细节，而是链接到更多的[复杂例子](https://medium.com/javascript-in-plain-english/react-components-lifting-state-up-59175b9efc9e)以及[教程](https://dev.to/aryanjnyc/freecodecamp-pomodoro-clock-02-lifting-state-up-and-react-props-1bp0)。几乎所有 React 应用程序都使用状态提升，优点是:

*   易于实施
*   状态保留在使用它的组件中
*   不需要额外的依赖

现在，如果我们看看不利的一面:

*   当许多变量需要提升时，很快变得混乱
*   当国家需要跨越许多层面时，很难理解
*   大量额外的代码

直截了当地说，在一个像样的应用程序中，仅仅使用状态提升通常不足以解决状态管理问题。不过好在有些库，其中著名的 [Redux](https://redux.js.org/) 带来了不同的解决方案。

![](img/94ad7605905dce386065ffb7e4e904a2.png)

credits to [https://redux.js.org/](https://redux.js.org/), obviously

# Redux 方式

Redux 可以与 React 一起使用，也可以与 Angular、Vue.js 等一起使用，通过所谓的“Redux Store”为状态管理提供集中和灵活的解决方案，Redux Store 充当全局状态管理工具，可以存储整个应用程序的状态。

听起来不错，不是吗？全局管理状态确实非常方便。Redux 与其他状态管理方法兼容，与 React 挂钩兼容，并附带一套 Chrome 开发工具，您可以在其中检查您的应用程序。中型 web 应用程序实际上也使用 Redux。

它围绕以下要素工作:

*   **Store:**顾名思义，存储全局状态。它通过`createStore`启动，并以一个减速器作为参数。
*   **动作:**返回一个对象的函数，该对象包含一个或多个要执行的动作的名称。这些名称将被缩减器使用。
*   **减速器:**取初始状态和一组作用于该状态的动作。当一个动作发生时，状态会根据 reducer 中的描述进行更新。它可以比作`setState`功能。
*   **分派:**采取行动并执行。

起初这看起来有点混乱，事实上，完全掌握 Redux 的学习曲线相当陡峭。该库有时会被许多人批评引入了太多的复杂性和太多的样板文件:从代码 youtubers [在他们的 Redux 教程中抱怨它的](https://www.youtube.com/watch?v=CVpUuw9XSjY&t=1648)，到 Reddit 上的社区或联合创建者 Dan abra mov——他自己在 [Twitter](https://twitter.com/dan_abramov/status/1039570011986321408) 上承认他*“采取了错误的方法来解释它”*。

也就是说，Redux 仍然是大型应用中复杂状态管理的一个非常好的解决方案。

几个备选库可以帮助您处理同一个问题:[**Mobx**](https://mobx.js.org/)**是的热门挑战者，尽管只有 Redux 的十分之一， **Apollo GraphQL** 在某种程度上，因为本地状态可以存储在 [Apollo cache](https://www.apollographql.com/docs/tutorial/local-state/) 或 vey small challenger[**pull state**](https://www.npmjs.com/package/pullstate)中，受益于不断增长的人气。**

**相反，我想关注的是 React: Context 中自带的 API。**

# **反应上下文方式**

**如果我们可以采取一种类似 Redux 的方法，同时去掉所有不必要的复杂性，会怎么样呢？2018 年 React 版本 16.3.0 首次引入了上下文来解决这个问题，并由于其实现通过挂钩`useContext`和`useReducer`变得非常容易而逐渐变得流行。**

**上下文的原则是有一个`Provider`，它存储状态，包装你的整个应用，并通过`Reducers`调用状态更新。任何需要消耗状态的组件都称为`useContext`。**

**具体细节我就不赘述了，可以推荐以下 2 个教程来详细了解实现:[号 1](https://medium.com/javascript-in-plain-english/how-to-use-react-context-api-with-functional-component-472f1d5e0851) 和[号 2](https://dev.to/ibrahima92/redux-vs-react-context-which-one-should-you-choose-2hhh) 。**

**虽然它与 Redux 有相似之处，但我个人认为 React Context 很容易上手，非常灵活，并且它有一个很大的优势，即不需要额外的外部库。**

**然而，不利的一面是，需要注意的是，当状态更新时，任何使用上下文的组件都将重新呈现，这对于性能至关重要的大型应用程序来说是一个很大的警告。然而，这个问题可以通过引入多个上下文，限制重新渲染的次数来解决。**

# **最后的想法**

**管理每个 React 应用程序的状态没有一个完美的解决方案，而是一组提供不同灵活性、速度或简单性的技术。**

**应用程序的理想状态管理是几种最适合您需求的技术的完美结合。**