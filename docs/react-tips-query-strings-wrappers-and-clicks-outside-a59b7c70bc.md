# React 提示—查询字符串、包装器和外部点击

> 原文：<https://javascript.plainenglish.io/react-tips-query-strings-wrappers-and-clicks-outside-a59b7c70bc?source=collection_archive---------6----------------------->

![](img/d12e9c4538315bf1a626145b2638d443.png)

Photo by [Michał Parzuchowski](https://unsplash.com/@mparzuchowski?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 修复“相邻的 JSX 元素必须包含在封闭标记中”错误

所有组件都必须有外部元素围绕。

例如，我们可以写:

```
return (
  <div>
    <Child1 />
    <Child2 />
  </div>
)
```

我们有一个围绕所有子元素的 div。

此外，如果我们不想呈现包装器元素，可以使用片段来包围我们的组件。

例如，我们可以写:

```
return (
  <>
    <Child1 />
    <Child2 />
  </>
)
```

或者:

```
return (
  <React.Fragment>
    <Child1 />
    <Child2 />
  </React.Fragment>
)
```

# 在 React 中修改状态数组的正确方法

为了在 React 中正确地修改状态数组，我们应该用一个返回新数组的回调函数调用`setState`的状态改变函数。

这样，我们知道新的值是从最新的值中得到的。

例如，我们可以写:

```
this.setState(prevState => ({
  array: [...prevState.array, newThing]
}))
```

我们将`newThing`添加到数组的末尾。

如果我们使用一个函数组件，我们可以写:

```
const [arr, setArr] = useState([]);
//...
setArr(prevArr => [...prevArr, newThing]);
```

# 检测 React 组件外部的单击

我们可以通过监听`documen`的点击事件来检测 React 组件外部的点击。

这样，我们可以处理任何元素的点击。

例如，我们可以写:

```
import React, { Component } from 'react'; export default class App extends Component {
  constructor(props) {
    super(props); this.setWrapperRef = this.setWrapperRef.bind(this);
    this.handleClickOutside = this.handleClickOutside.bind(this);
  } componentDidMount() {
    document.addEventListener('mousedown', this.handleClickOutside);
  } componentWillUnmount() {
    document.removeEventListener('mousedown', this.handleClickOutside);
  } setWrapperRef(node) {
    this.wrapperRef = node;
  } handleClickOutside(event) {
    if (this.wrapperRef && !this.wrapperRef.contains(event.target)) {
      alert('clicked outside');
    }
  } render() {
    return <div ref={this.setWrapperRef}>hello</div>;
  }
}
```

我们调用`docuyment.addEventListener`方法来监听`componentDidMount`钩子中的点击事件。

我们移除监听器，组件在`componentWillUnmount`钩子中的`removeListener`被卸载。

然后我们设置 div 的 ref，这样我们就可以检查哪个元素被点击了`handleclickOutside`，以及它是否在带有`contains`的组件内部。

同样，我们可以用钩子对功能组件做同样的事情。

例如，我们可以写:

```
import React, { useRef, useEffect } from "react"; function useClickOutside(ref) {
  useEffect(() => {
    function handleClickOutside(event) {
      if (ref.current && !ref.current.contains(event.target)) {
        console.log("clicked outside");
      }
    } document.addEventListener("mousedown", handleClickOutside);
    return () => {
      document.removeEventListener("mousedown", handleClickOutside);
    };
  }, [ref]);
}export default function App() {
  const wrapperRef = useRef(null);
  useClickOutside(wrapperRef);

  return <div ref={wrapperRef}>hello</div>;
}
```

我们创建了`useClickOutside`挂钩，以便在挂钩加载时添加事件监听器。

然后在函数中，我们在`useEffect`回调中返回，我们移除点击监听器。

我们观察`ref`的变化，因此我们将`[ref]`作为`useEffect`的第二个参数。

然后我们调用`useRef`来创建 ref，将它分配给 div，并用它调用`useClickOutside`。

# 如何从查询字符串中获取参数值

如果我们使用 React Router，我们可以用`URLSearchParams`构造函数和`location.search`属性从查询字符串中获取参数值。

例如，我们可以写:

```
new URLSearchParams(this.props.location.search).get("foo")
```

`this.prop.location.search`有查询字符串。

然后我们用`URLSearchParams`构造函数把它解析成一个对象。

我们用想要得到的查询参数的键调用`get`。

此外，我们可以使用:

```
this.props.match.params.foo
```

获取带有关键字`foo`的查询参数。

使用 React 路由器的 hooks 版本，我们可以编写:

```
import { useLocation } from 'react-router';
import queryString from 'query-string';const App = React.memo((props) => {
  const location = useLocation();
  console.log(queryString.parse(location.search)); return <p>search</p>;
}
```

我们使用 React 路由器的`useLocation`钩子从钩子中获取`location`对象。

然后我们可以使用`queryString`包来解析查询字符串。

我们也可以用`URLSearchParams`构造函数替换查询字符串包:

```
import { useLocation } from 'react-router';const App = React.memo((props) => {
  const location = useLocation();
  console.log(new URLSearchParams(location.search)); return <p>search</p>;
}
```

![](img/60fc47763b661bf63e7eb6ea0cd55f89.png)

Photo by [Blake Wisz](https://unsplash.com/@blakewisz?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该用根元素或片段包装我们的组件。

修改数组的正确方法是传递一个函数给`setState`或者状态改变函数。

我们可以通过添加事件侦听器来监视组件外部的点击。

此外，我们可以使用 React Router 从组件中获取查询字符串。

# 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**