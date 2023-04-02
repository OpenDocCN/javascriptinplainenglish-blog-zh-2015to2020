# 反应提示—强制渲染，条件，监听导航

> 原文：<https://javascript.plainenglish.io/react-tips-force-render-conditionals-listen-for-navigation-d1121912eae8?source=collection_archive---------10----------------------->

![](img/f91aba33b858a85e736300e494e889c1.png)

Photo by [Casey Chae](https://unsplash.com/@chaseycasey?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 如何强制一个函数组件呈现

我们可以用 use-force-update 包强制一个函数组件进行渲染。

要安装它，我们运行:

```
npm install use-force-update
```

或者:

```
yarn add use-force-update
```

然后我们可以通过写来使用它:

```
import useForceUpdate from 'use-force-update';const App = () => {
  const forceUpdate = useForceUpdate(); const rerender = () => {
    console('rerendering');
    forceUpdate();
  }; return <button onClick={rerender} />;
};
```

我们让`useForceUpdate`钩子返回`forceUpdate`函数，让我们随时更新。

此外，我们可以设置一个状态来更新 React 组件。

例如，我们可以写:

```
import React, { useState } from 'react';function useForceUpdate(){
  const [value, setValue] = useState(0); 
  return () => setValue(value => ++value);
}function App() {
  const forceUpdate = useForceUpdate(); return (
    <div>  
      <button onClick={forceUpdate}>
        re-render
      </button>
    </div>
  );
}
```

我们创建了自己的`useForceUpdate`钩子来更新一个`value`状态。

`useState`钩子返回一个状态变量和一个函数来更新它。

每当我们更新一个状态，React 将再次呈现组件。

# React 函数组件中的 componentDidMount 等效项

函数组件中`componentDidMount`的等价物是带有空数组的`useEffect`钩子。

例如，我们可以写:

```
useEffect(() => {
  //...
}, []);
```

使回调中的代码仅在组件挂载时加载。

我们还可以将变量传递给数组，以观察这些值的变化:

```
useEffect(() => {
  //...
}, [foo, bar]);
```

内容可以是任何值，如状态、道具等。

# 使用反应路由器检测路由变化

我们可以使用 React 路由器通过`history.listen`方法检测路由变化。

例如，我们可以写:

```
history.listen((location, action) => {
  console.log(location, action);
})
```

`location`是本地位置对象，它拥有所有 URL 数据，就像路径名的`pathname`一样。

`search`为查询字符串。

`hash`为哈希后的字符串。

`action`具有导航动作的名称。

# 正在 componentDidMount()上设置状态

在`componentDidMount`方法中设置状态不是反模式。

这是在组件挂载时设置状态的推荐做法。

例如，我们可以用它来获取 API 数据并设置状态:

```
componentDidMount() {
  fetch("https://randomuser.me/api")
    .then(res => res.json())
    .then(
      (result) => {
        this.setState({
          isLoaded: true,
          user: results[0]
        });
      },
      (error) => {
        this.setState({
          isLoaded: true,
          error
        });
      }
    )
}
```

我们用 fetch API 从一个 API 获取数据。

然后，我们在第一个`then`回调中获取结果数据，为数据设置状态。

在第二次回调中，我们将`isLoaded`状态设置为`false`和`error`状态。

# 在 React 渲染函数中使用 if…else…语句

有几种方法可以用 React 有条件地显示事物。

我们可以使用各种布尔表达式来做到这一点。

例如，我们可以写:

```
render() {
  const { isLoggedIn } = this.state;
  return (
    <div>
      {isLoggedIn ? (
        <LogoutButton onClick={this.handleClick} />
      ) : (
        <LoginButton onClick={this.handleClick} />
      )}
    </div>
  );
}
```

我们获取`isLoggedIn`状态，并使用它来检查用户是否登录。

如果用户不是，那么我们返回`LogoutButton`。

否则，我们返回`LoginButton`。

我们还可以使用 if-else 语句将组件赋给变量/

例如，我们可以写:

```
render() {
  let button;
  if (isLoggedIn) {
    button = <LogoutButton onClick={this.handleClick} />;
  } else {
    button = <LoginButton onClick={this.handleClick} />;
  } return (
    <div>
      {button}
    </div>
  );
}
```

我们用 if-else 语句而不是三元表达式来检查`isLoggedIn`。

我们将组件分配给了`button`变量，并对其进行渲染，而不是内联编写所有内容。

这让我们可以在条件语句中编写更长的表达式。

同样，我们可以使用`&&`操作符来显示给定条件下的事物。

例如，我们可以写:

```
render() {
  return (
    <div>
      {cartItems.length > 0 &&
        <h2>
          You have {cartItems.length} in the cart.
        </h2>
      }
    </div>
  );
}
```

如果`cartItems.length > 0`是`true`，那么它之后的东西被渲染。

![](img/3e18b3685210a96f257b2dbecc1cb3e1.png)

Photo by [Josh Howard](https://unsplash.com/@thejoshhoward?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以强制组件以各种方式呈现。

此外，我们可以用几种语句和表达式有条件地显示事物。

我们可以使用`history.listen`来监听 React 路由器中的变化。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**