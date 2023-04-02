# 反应提示—状态与道具、焦点输入和条件渲染

> 原文：<https://javascript.plainenglish.io/react-tips-state-vs-props-focus-inputs-and-conditional-rendering-a948ef1f4a7e?source=collection_archive---------5----------------------->

![](img/682f0176b648c68a4e3de3e575dbf28f.png)

Photo by [Sharon McCutcheon](https://unsplash.com/@sharonmccutcheon?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 呈现后在输入字段上设置焦点

我们可以通过给输入字段分配一个 ref 来设置焦点，并对其调用`focus`。

例如，我们可以写:

```
class App extends React.Component{
  componentDidMount(){
    this.nameInput.focus();
  }
  render() {
    return(
      <div>
        <input 
          ref={(input) => { this.nameInput = input; }} 
        />
      </div>
    );
  }
}

ReactDOM.render(<App />, document.getElementById('app'));
```

我们用带有`ref`属性的 ref 设置输入。

然后我们在`componentDidMount`里把所有`focus`放在它身上，让它专注于负载，

如果我们使用函数组件，我们可以写:

```
import React, { useEffect, useRef } from 'react';const Page = () => {
  const textInput = useRef(null); useEffect(() => {
    textInput.current.focus();
  }, []); return (
    <div>
      <input ref={textInput} />
    </div>
  );
};
```

我们使用带有的`useRef`钩子来获取一个 ref，并将其传递给输入的`ref`属性。

然后我们调用`useEffect`钩子中的`current.focus()`来将输入集中在 load 上。

作为第二个参数的空数组将使回调只在第一次加载时运行。

# 用 React 获取初始状态

我们可以用各种方法用 React 得到初始状态。

用`createClass`，我们可以写:

```
const App = React.createClass({ 
  getInitialState () { 
    return {
      items: []
    }; 
  }
});
```

我们使用`getInitialState`方法返回初始数据。

对于类组件，我们编写:

```
class App extends React.Component {
  constructor () {
    super()
    this.state = {
      items: []
    }
  }
};
```

我们将`this.state`设置为一个对象来设置`constructor`中的初始状态。

对于功能组件，我们通过编写使用`useState`钩子:

```
import React, { useState } from 'react';function App() {
  const items = useState([]);
}
```

# React 中状态和道具之间的差异

知道 React 中状态和道具的区别很重要。

道具是不变的，从父代传递到子代。

状态是动态的，它们停留在一个组件中。

我们可以用`setState`设置状态。

例如，我们可以通过以下方式传递道具:

```
<Child name={this.state.childsName} />
```

`name`是道具，`this.state.childName`是状态。

我们可以通过书写来改变状态:

```
this.setState({ childsName: 'some name' });
```

我们传入一个对象来改变状态。

以上都适用于类组件。

对于函数组件，属性在函数的参数中。

例如，如果我们有:

```
function A(props) {
  return <h1>{props.message}</h1>
}
```

`props`拥有所有的道具。

因此，我们也可以通过编写以下内容来析构这些属性:

```
function A({ message }) {
  return <h1>{message}</h1>
}
```

# 有条件地向反应组件添加属性

我们可以用布尔表达式有条件地添加属性。

例如，我们可以写:

```
let cond = true;return (
  <Button bsStyle={cond ? 'success' : undefined} />
);
```

如果是`true`我们可以检查`cond`返回`'success'`，否则返回`undefined`。

此外，我们可以写:

```
const cond = true;const component = (
  <div
    value="foo"
    { ...( cond && { disabled: true } ) } />
);
```

我们检查`cond`，如果是`true`，则返回`{ disabled: true }`。

# 在 React 中显示或隐藏元素

我们可以用一些 boolan 表达式显示或隐藏 React 中的元素。

例如，我们可以写:

```
const App = () => {
  const [toggle, setToggle] = React.useState(false);
  const onClick = () => setToggle(true);
  return (
    <div>
      <button onClick={onClick}>click me</button>
      { toggle ? <Results /> : null }
    </div>
  )
}
```

我们使用`useState`钩子来创建`toggle`状态，使用`setToggle`函数来设置状态。

该函数用在`onClick`函数中，它是由一次点击触发的，因为我们将它传递到按钮的`onClick`道具中。

当`toiggle`是`true`时，我们渲染`Result`组件。

![](img/4104af65aa8c064c7d06ea5b1ea3e706.png)

Photo by [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以切换状态以有条件地显示某些内容。

为了聚焦一个输入，我们给这个输入分配一个 ref，并对它调用`focus`。

我们可以用各种方式设置组件的初始值。

React 中状态和道具有很大的区别。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**