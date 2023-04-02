# 反应提示—共享数据

> 原文：<https://javascript.plainenglish.io/react-tips-sharing-data-623f60925446?source=collection_archive---------11----------------------->

![](img/42d525221c04fbbb9984e17523848d3f.png)

Photo by [Nicate Lee](https://unsplash.com/@nicn10?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将探讨一些编写更好的 React 应用程序的技巧。

# 从子组件内部更新反应上下文

我们可以通过首先创建一个上下文，并在上下文消费者中包装子组件来更新 React 上下文。

然后，我们可以使用子组件中的`useContext`挂钩返回的函数来进行更新。

例如，我们可以写:

```
const NameContext = React.createContext({
  name: "",
  setName: () => {}
});const NameSwitcher = () => {
  const { name, setName } = useContext(NameContext);
  return (
    <button onClick={() => setName("mary")}>
      change name
    </button>
  );
};const App = () => {
  const [name, setName] = useState(""); return (
    <NameContext.Provider value={{ name, setName }}>
      <h2>Name: {name}</h2>
      <div>
        <NameSwitcher />
      </div>
    </NameContext.Provider>
  );
};
```

首先，我们用`React.createContext`方法创建`NameContext`。

然后，我们有`App`组件，它有`name`状态和`setName`来更新`name`状态。

然后我们将`NameContext.Provider`包裹在我们想要访问`NameContext`的所有组件周围。

嵌套可以处于任何级别。

当我们使用`value`道具时，它里面的所有东西都会被`useContext`退回。

因此，在`NameSwitcher`中，我们可以使用`setName`功能来设置`name`状态，该状态也由`useContext`返回。

在类组件中，我们可以做同样的事情。

例如，我们可以写:

```
const NameContext = React.createContext({
  name: "",
  setName: () => {}
});class NameSwitcher extends Component {
  render() {
    return (
      <NameContext.Consumer>
        {({ name, setName }) => (
          <button onClick={() => setName("mary")}>
            change name
          </button>
        )}
      </NameContext.Consumer>
    );
  }
}class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      language: "en",
      setLanguage: this.setLanguage
    };
    this.setName = this.setName.bind(this);
  } setName(name) {
    this.setState({ name });
  }; render() {
    return (
      <NameContext.Provider value={{ 
        name: this.state.name, 
        setName: this.setName 
      }}>
        <h2>Name: {name}</h2>
        <div>
          <NameSwitcher />
        </div>
      </NameContext.Provider>
    );
  }
}
```

我们有相同的上下文对象，但状态改变函数，并且状态在不同的地方。

我们把它们传到`value`道具中放到对象上。

由于我们将它们传递到了`value`道具的对象中，因此我们可以在`NameSwitcher`中访问它们。

唯一的区别在于，我们需要在其中添加一个`NameContext.Consumer`和一个回调来访问这些属性。

它们在回调的参数中。

# useCallback 和 useMemo 之间的区别

我们使用`useMemo`来记住函数调用和呈现的计算结果。

`useCallback`用于在渲染之间记住回调本身。

参考已快取。

`useRef`是保持数据之间呈现。

`useState`同样如此。

因此，我们可以使用`useMemo`来避免繁重的计算。

`useCallback`修复了内联处理程序导致`PureComponent`的子代再次呈现的性能问题。

出现这种情况是因为函数表达式每次呈现时引用的内容不同。

# 在 Redux 中发出 Ajax 请求

如果使用 Redux Thunk 中间件，我们可以用 Redux 发出 Ajax 请求。

要安装它，我们运行:

```
npm install redux-thunk
```

为了使用它，我们写:

```
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';
import rootReducer from './reducers/index';const store = createStore(rootReducer, applyMiddleware(thunk));
```

我们导入了`rootReducer`和`thunk`中间件。

我们调用`applyMiddleware`来应用`thunk`中间件。

现在，我们可以通过编写以下代码来创建自己的 thunk:

```
const fetchUsers = () => {
  return dispatch => {
     fetch(`https://randomuser.me/api/`)
       .then(res => res.json())
       .then(res => {
          dispatch(saveUserData(res))
       }) 
   }
}store
  .dispatch(fetchUsers())
  .then(() => {
    //...
  })
```

我们将一个带有`dispatch`参数的函数，也就是 Redux `dispatch`函数，传递给`store.dispatch`方法。

调用`dispatch`参数将数据放入 Redux 存储中。

然后，为了将该函数映射到组件的道具，我们可以编写:

```
function mapDispatchToProps(dispatch) {
  return {
    fetchUsers: () => dispatch(fetchUsers()),
  };
}
```

现在我们可以调用`this.prop.fetchUsers`来调用调度我们的异步动作。

`fetchUsers`是一个动作创建函数，但它是异步的。

这要感谢 Redux Thunk 插件。

![](img/167ec1a5c6a0d0ebe7495fbd7a9b2534.png)

Photo by [Matthew Foulds](https://unsplash.com/@fouldsmatt?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用上下文 API 与类和函数组件共享数据。

如果我们想要一种更灵活的方式来共享数据，我们可以使用 Redux with Redux Thunk。

这让我们能够以异步方式设置数据。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**