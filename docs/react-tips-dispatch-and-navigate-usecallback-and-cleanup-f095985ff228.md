# 反应提示—分派和导航、使用回调和清理

> 原文：<https://javascript.plainenglish.io/react-tips-dispatch-and-navigate-usecallback-and-cleanup-f095985ff228?source=collection_archive---------5----------------------->

![](img/6cd10e83f3c9bd2ba9bfe3e6911da2e7.png)

Photo by [Carl Flor](https://unsplash.com/@carlflor?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# React.useState 不从 Props 重新加载状态

`React.useState`不从道具重新加载状态。

它只能设置初始状态。

如果我们需要在道具改变时更新状态，那么我们需要使用`useEffect`钩子。

例如，我们可以写:

```
function Avatar({ username }) {
  const [name, setName] = React.useState(username); React.useEffect(() => {
    setName(name);
  }, [username]) return <img src={`${name}.png`}/>
}
```

我们得到了`username`道具。

并且我们用`useState`把它设为`name`状态的初始值。

然后我们使用`useEffect`钩子来观察`name`值的变化。

我们观察它的变化，然后调用`setName`用`username`的最新值设置名称作为`name`的新值。

然后我们可以在我们渲染的 JSX 中使用`name`。

# 用于 useCallback

我们可以使用`useCallback`来缓存回调，这样我们就不会在每次渲染时都调用创建回调函数的新实例。

例如，不写:

```
function Foo() {
  const handleClick = () => {
    console.log('clicked');
  }
  <button onClick={handleClick}>click me</button>;
}
```

我们写道:

```
function Foo() {
  const memoizedHandleClick = useCallback(
    () => console.log('clicked'), [],
  ); return <Button onClick={memoizedHandleClick}>Click Me</Button>;
}
```

`useCallback`钩子缓存点击回调，以便在每次渲染时使用相同的实例，而不是创建一个新的。

# 检测反应组分与反应元素

我们可以通过检查一个对象是否是一个函数以及它是否包含`'return React.createElement'`代码来检查它是否是一个函数组件。

例如，我们可以写:

```
const isFunctionComponent = (component) => {
  return (
    typeof component === 'function' && 
    String(component).includes('return React.createElement')
  )
}
```

为了检查类组件，我们可以检查类型`'function'`。

我们可以在组件的`prototype`中检查`isReactComponent`属性。

例如，我们可以写:

```
const isClassComponent = (component) => {
  return (
    typeof component === 'function' && 
    !!component.prototype.isReactComponent
  )
}
```

为了检查一个变量是否是一个有效的元素，我们可以使用`React.isValidElement`方法来完成。

例如，我们可以写:

```
const isElement = (element) => {
  return React.isValidElement(element);
}
```

# 当组件卸载时，React 挂钩 useEffect()清理

我们可以在`useEffect`回调中返回一个函数来返回一个运行清理代码的函数。

例如，我们可以写:

```
const { useState, useEffect } = React;const ForExample = () => {
  const [name, setName] = useState("");
  const [username, setUsername] = useState(""); useEffect(
    () => {
      console.log("do something");
    },
    [username]
  ); useEffect(() => {
    return () => {
      console.log("cleaned up");
    };
  }, []); const handleName = e => {
    const { value } = e.target;
    setName(value);
  }; const handleUsername = e => {
    const { value } = e.target;
    setUsername(value);
  }; return (
    <div>
      <div>
        <input value={name} onChange={handleName} />
        <input value={username} onChange={handleUsername} />
      </div>
    </div>
  );
};
```

我们有自己的变更事件处理程序。

我们有我们的`useEffect`电话。

都有自己的试镜。

第一个观察`username`功能的变化。

在第二个例子中，回调函数返回一个在组件卸载时运行代码的函数。

# Redux 操作后反应路由器重定向

我们可以在 Redux 操作提交后重定向。

为此，我们安装了`history`包。

我们通过运行以下命令来安装它:

```
npm install --save history
```

然后我们可以创建一个函数，比如:

```
import { createBrowserHistory } from 'history';const browserHistory = createBrowserHistory();const actionName = () => (dispatch) => {
  axios
    .post('url', { body })
    .then(response => {        
       dispatch({
         type: ACTION_TYPE_NAME,
         payload: payload
       });      
       browserHistory.push('/foo');
    })
    .catch(err => {
      // Process error code
    });
  });
};
```

我们向 Axios 发出 POST 请求。

然后在`then`回调中，我们调用`dispatch`来调度我们的动作。

然后我们呼叫`browserHistory.push`来导航。

我们调用`createBrowserHistory`来获取`browserHistory`对象。

![](img/ddd3d2e9d06573e5ce7d9146e4b33883.png)

Photo by [Premkumar Masilamani](https://unsplash.com/@smileprem?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们使用`useEffect`钩子来更新我们的值。

它还可以用于在组件卸载时运行清理代码。

如果我们需要在 Redux 动作完成后导航到另一条路线，我们可以在分派动作后使用`browserHistory.push`方法来完成。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**