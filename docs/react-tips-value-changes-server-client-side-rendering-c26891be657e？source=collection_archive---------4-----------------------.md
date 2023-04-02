# React 提示—值更改、服务器/客户端呈现

> 原文：<https://javascript.plainenglish.io/react-tips-value-changes-server-client-side-rendering-c26891be657e?source=collection_archive---------4----------------------->

![](img/41ef25457a9689b2e5f08306f258797f.png)

Photo by [FLOUFFY](https://unsplash.com/@theflouffy?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 为什么不能在 React 中更改输入值

我们不能用 React 改变输入值，因为我们必须手动设置输入的`value`属性。

例如，我们写道:

```
<input
  className="form-control"
  type="text" 
  value={this.state.name}
  onChange={e => this.onChange(e.target.value)}
/>
```

那么在`onChange`方法中，我们可以写:

```
onChange(value){
  this.setState({
     name: value
  });
}
```

我们在方法中设置了`name`状态。

# 如何使用 React JSX 渲染带单引号的文本

我们可以渲染一个字符串来渲染单引号。

例如，我们可以写:

```
return (
   <div>
     <p>{"I've eaten."}</p>
   </div>
 )
```

我们也可以使用单引号的 HTML 实体版本。

例如，我们可以写:

```
<Text>I&quot;ve eaten.</Text>
```

# 检查变量是反应节点还是数组

我们可以用`React.isValidElement`来检查一个变量是节点还是数组。

它需要一个变量来检查。

如果是则返回`true`，否则返回`false`。

# 检测组件是从客户端还是从服务器呈现

我们可以使用生命周期挂钩来检测服务器端呈现。

如果它们运行，那么它会在客户端呈现。

因此，我们可以创建自己的钩子来进行检查。

我们写道:

```
import { useState, useEffect } from 'react'function useIsClient () {
  const [isClient, setIsClient] = useState(false)
  useEffect(() => {
    setIsClient(true)
  }, [])
  return setIsClient
}
```

来制造`useIsClient`挂钩。

如果运行了`useEffect`回调，那么我们知道它是在客户端呈现的。

然后我们可以检查它是一个客户端渲染的应用程序。

然后我们可以通过写来使用它:“

```
import { useState, useEffect } from 'react'function useIsClient () {
  const [isClient, setIsClient] = useState(false)
  useEffect(() => {
    setIsClient(true)
  }, [])
  return setIsClient
}function App() {
  const isClient = useIsClient();
  return isClient
    ? 'client'
    : 'serve'
}
```

我们使用`useIsClient`钩子来检查客户端渲染。

然后我们可以根据它来渲染我们想要的东西。

# 更改复选框状态

我们可以通过向复选框传递`checked`属性来改变复选框的状态。

例如，我们可以写:

```
class CheckBox extends React.Component {
  render() {
    return (<input type="checkbox" checked={this.props.checked} />);
  }
}class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = { checked: true };
  } render() {
    return (
      <div>
        <a href="#" onClick={ () => { this.setState({ checked: !this.state.checked }); }}>Toggle</a>
        <hr />
        <CheckBox checked={this.state.checked} />
      </div>
    );
  }
}
```

当我们点击链接时，我们切换复选框。

然后我们将`checked`状态传递给我们的`CheckBox`组件，它有复选框。

然后在`CheckBox`组件中，我们将`checked`属性传递给`checked`值。

# React 路由器使用普通锚链路

如果我们使用 React 路由器，React 路由器哈希链接包可以让我们在 React 应用程序中轻松创建哈希链接。

要安装它，我们运行:

```
yarn add react-router-hash-link
```

或者:

```
npm install --save react-router-hash-link
```

然后我们可以通过写来使用它:

```
import { HashLink as Link } from 'react-router-hash-link';//...
const App = () => {
  //...
  return <Link to="/some/path#some-hash">hash link</Link>
}
```

我们还可以添加一个导航链接，让我们设置活动样式。

我们可以这样写:

```
import { NavHashLink as NavLink } from 'react-router-hash-link';//...
const App = () => {
  //...
  return <NavLink 
    to="/some/path#some-hash" 
    activeClassName="acrive"
  >
     hash nav link
  </NavLink>
}
```

当链接处于活动状态时，需要一个`activeClassName` prop 来让我们设置活动的类名。

此外，当我们导航到具有给定 ID 的元素时，它支持更改滚动行为。

该链接也可以自定义。

![](img/13cc273d81522f5372da1860614f73a6.png)

Photo by [Charlie Green](https://unsplash.com/@charliegreen998?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

如果我们的应用程序使用 React 路由器，我们可以使用一个包来创建哈希链接。

如果我们为文本输入设置`value`属性，可以显示输入更改。

对于复选框输入，我们可以向`checked`属性传递一个值。

我们可以将单引号呈现为 HTML 实体或字符串。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**