# 反应模式——创建可重用组件

> 原文：<https://javascript.plainenglish.io/react-patterns-creating-reusable-components-f7b9f299a5da?source=collection_archive---------8----------------------->

![](img/e5ed9f0834e7d0c99c50c3e49f6f3815.png)

Photo by [Mathew Schwartz](https://unsplash.com/@cadop?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将看看如何通过创建可重用的组件来清理 React 代码。

# 创建组件

我们可以通过使用功能组件来创建组件。

所以我们可以写:

```
import React from "react";export default function App() {
  return <button>click me</button>;
}
```

我们创建了一个带有按钮的组件。

同样，我们可以创建一个类组件:

```
class App extends React.Component {
  render() {
    return <button>click me</button>;
  }
}
```

他们做同样的事情。

# 差异

尽管功能组件和类组件做同样的事情，但它们之间的差别是很大的。

# 小道具

功能组件具有从组件的参数中获取的属性。

类组件可以使用`this.props`访问道具。

例如，在一个函数组件中，我们有一个带有属性的对象参数:

```
const Button = ({ text }) => {
  return <button>{text}</button>;
};
```

`text`是`props`参数的一个属性，它有适当的值。

在类组件中，我们可以编写:

```
class Button extends React.Component {
  render() {
    return <button>{this.props.text}</button>
  }
}
```

我们有`render`方法来呈现带有`this.props.text`的按钮，该按钮具有相同的`text`属性值。

我们可以用`getDefaultProps`来设置默认道具。

例如，我们可以写:

```
Button.defaultProps = {
  text: "click me"
};
```

这适用于类和函数组件。

它将`text`道具的值设置为`'click me'`。

我们也可以通过使用`prop-types`包来设置道具类型。

例如，我们可以写:

```
import React from "react";
import PropTypes from "prop-types";const Button = ({ text }) => {
  return <button>{text}</button>;
};Button.propTypes = {
  text: PropTypes.string
};
```

我们验证了`text`属性必须是一个字符串。

# 状态

为了存储状态，我们可以在函数组件中使用`useState`钩子，在类组件中使用`this.state`。

例如，我们可以写:

```
import React, { useState } from "react";export default function App() {
  const [msg, setMsg] = useState("foo");
  return <p>{msg}</p>;
}
```

我们有用`useState`钩子创建的`msg`状态。

可以用`setMsg`更新`msg`的值。

在类组件中，我们可以编写:

```
import React from "react";export default class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      msg: "foo"
    };
  } render() {
    return <p>{this.state.msg}</p>;
  }
}
```

我们在构造函数中设置状态，而不是使用`useState`钩子。

# 绑定到此

对于类组件，我们必须将事件处理程序绑定到正确的`this`以便我们可以使用它。

我们不必在功能组件中引用`this`。

例如，在函数组件中，我们编写:

```
import React from "react";export default function App() {
  const onClick = () => {
    console.log("clicked");
  }; return <p onClick={onClick}>hello</p>;
}
```

我们不必引用`this`来运行点击处理程序。

另一方面，在类组件中，我们必须编写:

```
import React from "react";export default class App extends React.Component {
  onClick() {
    console.log("clicked");
  } render() {
    return <p onClick={this.onClick.bind(this)}>hello</p>;
  }
}
```

我们必须调用`bind`来改变`onClick`方法中的`this`，这样我们就可以运行处理程序了。

# 无状态功能组件

无状态功能组件是没有状态的功能组件。

它让我们获得道具和渲染组件，而无需任何状态。

例如，我们有:

```
const Button = ({ text }) => {
  return <button>{text}</button>;
};
```

这是一个无状态的函数组件，因为它需要一个属性`text`并呈现它。

# 道具和背景

我们可以通过编写以下内容来定义带有验证的道具:

```
import React from "react";
import PropTypes from "prop-types";const Button = ({ text }) => {
  return <button>{text}</button>;
};Button.propTypes = {
  text: PropTypes.string
};
```

接受具有上下文的第二个参数的函数组件。

# 这

无状态函数组件中不需要`this`。

# 状态

无状态功能组件没有状态。

他们只能接收道具和上下文。

# 生命周期

无状态功能组件中没有生命周期方法。

它只是呈现 return 语句中的组件。

![](img/a24c2cb2358d58084b4a90d751fd5415.png)

Photo by [Zdeněk Macháček](https://unsplash.com/@zmachacek?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 参考文献

无状态函数组件可以有引用。

例如，我们可以写:

```
import React, { useRef } from "react";const Input = () => {
  const ref = useRef(); const focus = () => ref.current.focus();
  return (
    <>
      <input ref={ref} />
      <button onClick={focus}>focus</button>
    </>
  );
};
```

我们调用`useRef`钩子来创建一个 ref。

`current`属性有 DOM 方法，我们可以调用它们。

`focus`功能聚焦输入。

然后我们点击聚焦，我们聚焦输入。

# 结论

我们可以用类和函数组件来创建组件。

它们是等价的，但是功能组件没有生命周期方法或者需要引用`this`。