# 理解 React 16.8 生命周期、钩子、上下文 API、懒惰和悬念

> 原文：<https://javascript.plainenglish.io/understanding-react-16-8-life-cycles-hooks-context-api-lazy-and-suspense-d80760f1b8f2?source=collection_archive---------0----------------------->

![](img/6d2787291b64486fe4bf9fe78cd27794.png)

自从 React 出现以来，前端开发已经有了显著的创新。开发的容易程度显著提高，它使开发人员能够以非常直观的方式对应用程序的行为进行编码。它还带来了[设计原则](https://reactjs.org/docs/design-principles.html)，如组合、公共抽象、逃生舱口等。通过函数式编程。但最重要的是，它对用户体验的关注使其成为前端图书馆的领导者。

但是同样的原则，如果理解不正确，会导致某些反模式。例如，不是构建可重用的组件(组合)，而是多次构建具有微小差异的组件将会浪费精力和时间，并引入不可预测的性能问题。类似地，没有以适当的方式利用常见的 React 抽象，比如生命周期，或者没有明智地使用 Escape Hatches，都会导致糟糕的用户体验。

作为一个库，React 并不提倡开发人员采用最佳实践。但是随着它扩散到企业应用程序中，反模式开始影响性能。因此，回应迫在眉睫，这种转变将使 React 更强大，在编码实践中更有影响力，并将大量新的用例集成到平台中。脸书如何对 React 的核心无缝和向后兼容进行结构性转变，这本身就是另一个[有趣的故事](https://code.fb.com/web/react-16-a-look-inside-an-api-compatible-rewrite-of-our-frontend-ui-library/)。此外，脸书一直在尝试将[异步引入渲染](https://www.youtube.com/watch?v=nLF0n9SACd4)本身，这将使用户体验更好。

**总之，维护好的设计原则和模式、酷的新特性、异步渲染、钩子、上下文管理和对代码库的彻底检查，以实现上述所有东西，这就是 React v 16.8 及更高版本的原因。**

# 生命周期

React 中的生命周期函数提供了必要的通用抽象功能，以利用 React 组件从安装到卸载的生命周期。但是对这些的曲解会导致糟糕的用户体验。例如，如果我们需要一个 I/O 调用来获取组件中显示的数据，我们通常倾向于在组件呈现之前进行。所以我们把 I/O 调用放在威尔蒙特。但这将导致用户盯着一个空白页面，混淆一些可能是错误的。相反，如果我们调用 didMount 并在初始渲染中显示一个加载器，用户就知道有事情发生了，他只需要等待。

尽管逻辑表明在展示任何东西之前你需要数据，但是用户体验是前端开发的重中之重。尽管 React 直到 15 版才开始流行，但从 16 版开始，它要求人们使用更好的生命周期，如 getDerivedStateFromProps，而不是 componentWillReceiveProps。componentWillMount、componentWillReceiveProps 和 shouldComponentUpdate 被标记为不安全，以后将不再推荐使用。

图片来源:[巴托什·什切青斯基的](https://medium.com/@baphemot/understanding-react-react-16-3-component-life-cycle-23129bc7a705)和[马赫什·哈尔达尔的](https://hackernoon.com/reactjs-component-lifecycle-methods-a-deep-dive-38275d9d13c0)博客

![](img/2f3f29b6c602a406af4304acb470fd09.png)

React 15 to 16 — Lifecycles

componentWillReceiveProps 的目的是捕捉任何必要的状态操作和属性的变化。有副作用是不理想的，因为它们在更新周期中抑制了渲染。为了消除这种反模式，将 willReceiveProps 声明为不安全的，并使用静态 getDerivedStateFromProps 来操纵更新周期中的状态。因为它是静态的(你不能访问“this”)，组件特定的副作用是不可能的。

*这里讨论的概念直接引用自 React 文档。我把我自己的理解添加到他们当中，但是只有在参考了文档的情况下，彻底的理解才是可能的。我建议浏览下面的材料，作为对每个概念是什么以及为什么会这样发展的简要概述。*

# 钩住

钩子是 React 的新增功能，它让你不用写类就可以使用状态和其他 React 特性。沿着 React 组件类创建钩子的动机是—

1.  **在组件之间重用有状态逻辑:**创建模块化组件的一般趋势是将其分解成多个功能组件块，这些功能组件块又被包装在有状态组件中。尽管这是一个很好的实践，但是 React DOM 树显示了用状态包装的多个组件以一种令人困惑的方式连接在一起。如果我们可以将有状态逻辑本地化到每个功能组件，甚至在逻辑相似的情况下重用它，会怎么样？
2.  **复杂的组件变得难以理解:**随着组件规模和复杂性的增长，副作用也在增长。尤其是嵌入到生命周期中的副作用和有状态逻辑使得组件更加难以理解和扩展。分解组件也越来越难。如果我们可以构建没有生命周期的组件，但仍然使用状态，那会怎么样？
3.  **类语法:**类语法很难理解和实现，尤其是因为“this”的用法。在构造函数中绑定事件和回调上下文的必要性似乎是不必要的麻烦。如果使用功能组件，就可以完全消除。

钩子用一个解决方案解决了上面的问题，虽然不是直截了当的，但是一旦理解了，感觉很容易实现。

挂钩是让您从功能组件“挂钩”React 状态和生命周期特性的功能。钩子在类内部不起作用——它们让你在没有类的情况下使用 React。(不建议一夜之间重写现有的组件，但是如果你愿意，可以在新的组件中使用钩子。)

## 内置挂钩:

1.  [**状态挂钩**](https://reactjs.org/docs/hooks-state.html) **:**

基本计数器的传统 React 类实现如下所示:

```
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```

相同的钩子实现如下所示:

```
import React, { useState } from 'react';

function Example() {
  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);
const [a, modifyA] = useState(1) return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

我们能够在功能组件中使用状态。所以我们没有任何“这个”。

**a.** useState 钩子只接受一个参数——状态的初始值，在本例中为“0”。

**b.** useState 返回状态变量引用和用于更新它的函数，使用数组析构将它们赋给“count”和“setCount”。

2. [**效果挂钩**](https://reactjs.org/docs/hooks-effect.html) **:**

在一个组件中，我们通常需要嵌入副作用，如数据获取、订阅、直接 DOM 操作等。这些副作用不能成为渲染的一部分，因为抑制渲染是一个糟糕的设计。所以这些曾经是其他生命周期的一部分，比如 componentWillMount、componentDidMount、componentWillReceiveProps(非常糟糕的主意！)，componentWillUnmount 等。

但是钩子，作为功能组件，没有生命周期。但是有一种方法可以使用 Effect Hook 实现所有的副作用。但是，Effect Hook 并没有假设从装载到更新再到卸载的生命周期，而是只假设渲染后渲染的生命周期。这意味着初始渲染和后续渲染会触发相同的效果。

下面是传统的副作用实现:

```
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  componentDidMount() {
    document.title = `You clicked ${this.state.count} times`;
  }

  componentDidUpdate() {
    document.title = `You clicked ${this.state.count} times`;
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```

请注意，尽管效果完全相同，但是初始和后续渲染都需要设置文档的标题。现在，看看效果挂钩的实现:

```
import React, { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  // Similar to componentDidMount and componentDidUpdate:
  useEffect(() => {
    // Update the document title using the browser API
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

这里，useEffect 钩子接受一个函数作为参数。这个功能叫做效果。它在每次渲染后运行。这是一个没有清理效果的例子。但是，当侦听器在初始渲染后被设置时，必须在卸载它之前将其删除。这是在钩子中通过将它指定为效果函数的返回值来实现的。

下面是使用生命周期的实现:

```
componentDidMount() {
    ChatAPI.subscribeToFriendStatus(
      this.props.friend.id,
      this.handleStatusChange
    );
  }

  componentWillUnmount() {
    ChatAPI.unsubscribeFromFriendStatus(
      this.props.friend.id,
      this.handleStatusChange
    );
  }
```

下面是它使用效果钩子的实现:

```
useEffect(() => {
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    // Specify how to clean up after this effect:
    return function cleanup() {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });
```

正如可以观察到的，React 为每个渲染创建一个新的效果，为每个渲染分解它，这不是生命周期的情况。这实际上是由设计决定的，其中效果总是被假定为每个渲染都是特定的。如果这是一个性能问题，有一个[的方法来停止重复](https://reactjs.org/docs/hooks-effect.html#tip-optimizing-performance-by-skipping-effects)每次渲染的效果。

除了使用内置钩子，React 还提供了一种[构建定制钩子](https://reactjs.org/docs/hooks-custom.html)的方法。

# 上下文 API

当一个孩子被深深地嵌入到 DOM 树中时，它需要从最顶层的父节点获得的任何道具都需要作为道具不断地传递下去。为了避免这种麻烦，React 提供了上下文 API。它的功能类似于 Redux，它维护一种全局状态，并且[将道具传递给组件，避免中间组件。](// Context lets us pass a value deep into the component tree // without explicitly threading it through every component. // Create a context for the current theme (with "light" as the default). const ThemeContext = React.createContext('light');  class App extends React.Component {   render() {     // Use a Provider to pass the current theme to the tree below.     // Any component can read it, no matter how deep it is.     // In this example, we're passing "dark" as the current value.     return (       <ThemeContext.Provider value="dark">         <Toolbar />       </ThemeContext.Provider>     );   } }  // A component in the middle doesn't have to // pass the theme down explicitly anymore. function Toolbar(props) {   return (     <div>       <ThemedButton />     </div>   ); }  class ThemedButton extends React.Component {   // Assign a contextType to read the current theme context.   // React will find the closest theme Provider above and use its value.   // In this example, the current theme is "dark".   static contextType = ThemeContext;   render() {     return <Button theme={this.context} />;   } })

没有上下文:

```
class App extends React.Component {
  render() {
    return <Toolbar theme="dark" />;
  }
}

function Toolbar(props) {
  // The Toolbar component must take an extra "theme" prop
  // and pass it to the ThemedButton. This can become painful
  // if every single button in the app needs to know the theme
  // because it would have to be passed through all components.
  return (
    <div>
      <ThemedButton theme={props.theme} />
    </div>
  );
}

class ThemedButton extends React.Component {
  render() {
    return <Button theme={this.props.theme} />;
  }
}
```

带上下文:

```
// Context lets us pass a value deep into the component tree
// without explicitly threading it through every component.
// Create a context for the current theme (with "light" as the default).
const ThemeContext = React.createContext('light');

class App extends React.Component {
  render() {
    // Use a Provider to pass the current theme to the tree below.
    // Any component can read it, no matter how deep it is.
    // In this example, we're passing "dark" as the current value.
    return (
      <ThemeContext.Provider value="dark">
        <Toolbar />
      </ThemeContext.Provider>
    );
  }
}

// A component in the middle doesn't have to
// pass the theme down explicitly anymore.
function Toolbar(props) {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

class ThemedButton extends React.Component {
  // Assign a contextType to read the current theme context.
  // React will find the closest theme Provider above and use its value.
  // In this example, the current theme is "dark".
  static contextType = ThemeContext;
  render() {
    return <Button theme={this.context} />;
  }
}
```

尽管使用起来似乎很简单，但是上下文 API 应该只在多个嵌套组件需要访问一个公共状态时使用。相反，如果唯一关心的是沿着中间组件多次传递 props，[组件组合](https://reactjs.org/docs/composition-vs-inheritance.html)是更好的选择。

1.  **React.createContext** 函数创建上下文对象。它接受上下文的初始值作为参数。返回值将被赋给变量，此后称为上下文。“ThemeContext”是上例中的上下文，“light”是默认值。必要时，使用“Provider”属性在嵌套组件中覆盖该值。在上面的示例中，它被更改为“暗”。当没有匹配的提供程序时，组件采用默认的上下文值。
2.  **上下文。提供者**允许消费组件订阅上下文变化。它充当上下文变化的发布者。这种变化可以被多个消费者感知。它接受新的上下文值作为属性。在上面的例子中，每当 App component 运行时，主题都被提供者设置为“黑暗”。任何消费者都会听到这个消息，并触发重新呈现(不受 shouldComponentUpdate 的影响)。另请参见 [Class.contextType](https://reactjs.org/docs/context.html#classcontexttype) 了解上下文如何绑定到特定的类。
3.  **语境。消费者**允许组件订阅上下文值的变化。它需要一个函数作为子函数，该子函数接受最新的值作为参数。

```
<MyContext.Consumer>
  {value => /* render something based on the context value */}
</MyContext.Consumer>
```

# 懒惰和悬念

惰性特性的行为类似于惰性加载的概念。但是，我们可以简单地依靠 React 本身在加载组件时加载特定的代码束，而不是依赖 webpack 来完成这项工作。这也可以应用于路由级别。

暂停特性有助于在代码包延迟下载的情况下退回到替代组件。例如，我们可以展示一个装载机。

[举例](https://reactjs.org/docs/code-splitting.html#route-based-code-splitting) *:*

```
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import React, { Suspense, lazy } from 'react';

const Home = lazy(() => import('./routes/Home'));
const About = lazy(() => import('./routes/About'));

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Switch>
        <Route exact path="/" component={Home}/>
        <Route path="/about" component={About}/>
      </Switch>
    </Suspense
  </Router>
);
```

在这里，只有在点击根路径时才加载 Home 组件。类似地，仅当点击 about path 时，才会加载 About 组件。当加载需要时间时，“正在加载…”文本显示为后备。

采用这种技术可以使初始加载速度极快，从而增强用户体验。虽然延迟加载每个组件看起来会有不必要的性能问题，但是下载和加载小组件应该只需要很短的时间。此外，这些下载是并行进行的，这是一个更大的优势。因此，总之，延迟加载每个组件可能是一种反模式，开发人员应该明智地选择在初始下载期间保留不必要的代码，并根据需要延迟加载它们，而不会牺牲用户体验。

React 16 中有许多令人兴奋的功能，如门户、转发参考、备忘录、片段等。请仔细阅读 React 文档，以深入了解每个文档以及上面讨论的概念。