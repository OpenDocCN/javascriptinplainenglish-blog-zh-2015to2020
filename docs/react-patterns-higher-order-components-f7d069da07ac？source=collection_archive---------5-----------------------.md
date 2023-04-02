# 反应模式—高阶组件

> 原文：<https://javascript.plainenglish.io/react-patterns-higher-order-components-f7d069da07ac?source=collection_archive---------5----------------------->

![](img/b600b71800c53a7ce175d4d06f9039b5.png)

Photo by [Henry Be](https://unsplash.com/@henry_be?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将研究如何组合 React 组件。

# 高阶组件

高阶组件是接受其他组件作为参数并在修改后返回它们的组件。

高阶组件如下所示:

```
const HoC = Component => EnhancedComponent
```

我们接受一个组件并返回一个增强的组件。

例如，我们可以写:

```
const withClassName = Component => props => (
  <Component {...props} className="foo" />
)
```

然后我们可以返回一个添加了类名属性的组件。

我们不直接使用它，而是写:

```
const ComponentWithClassName = withClassName(Component)
```

我们总是以`with`开始高阶分量，以便区分它们。

有了更高阶的组件，我们可以创建不同的彼此相似，但基本相同的组件。

# 重写

重组包有各种高阶组件，我们可以单独使用或组合使用。

要安装它，我们可以运行:

```
npm install recompose --save
```

然后我们可以通过写来使用它:

```
import React from "react";
import { withState } from "recompose";const enhance = withState("count", "setCount", 0);
const Counter = enhance(({ count, setCount }) => (
  <div>
    Count: {count}
    <button onClick={() => setCount(n => n + 1)}>Increment</button>
  </div>
));export default function App() {
  return <Counter />;
}
```

我们用`withState`函数创建了一个`Counter`组件来保存状态，而不是使用`useState`钩子。

然后我们将组件传递给`enhance`函数，这样我们就可以得到我们需要的道具。

设置它的状态和功能可以作为道具使用。

这使得我们的状态可以在多个组件中使用。

我们还可以使用许多其他功能。

例如，我们可以写:

```
import React from "react";
import { withProps, flattenProp, compose } from "recompose";
const enhance = compose(
  withProps({
    obj: { a: "a", b: "b" },
    c: "c"
  }),
  flattenProp("obj")
);const Foo = enhance(({ a, b, c }) => (
  <div>
    {a} {b} {c}
  </div>
));export default function App() {
  return <Foo />;
}
```

我们调用`compose`将多个高阶分量合并成一个。

我们需要`withProps`来传递一些道具。

我们调用`flattenProp`来展平嵌套的`obj`道具。

然后，为了创建`Foo`，我们将一个组件传递到`enhance`函数中，并显示道具的值。

然后我们在`App`中显示`Foo`。

# 像孩子一样生活

函数可以作为另一个组件的子属性传入。

例如，我们写道:

```
const FunctionAsChild = ({ children }) => children()
```

要使用它，我们可以写:

```
import React from "react";const FunctionAsChild = ({ children }) => children();export default function App() {
  return <FunctionAsChild>{() => <div>hello/div>}</FunctionAsChild>;
}
```

我们在标签之间传递了一个函数，这样它就可以在`FunctionAsChild`中被调用。

此外，我们可以传入参数。例如，我们可以写:

```
import React from "react";const Greeting = ({ children }) => children("james");export default function App() {
  return <Greeting>{name => <div>Hello, {name}!</div>}</Greeting>;
}
```

我们有一个`Greeting`组件，我们称之为`children`，它位于标签之间。

然后在`App`中，我们传入一个在`Greeting`中调用的函数。

这使得我们的功能更加灵活。

# 数据流

React 具有单向数据流。

它总是从父母流向孩子。

为了让孩子和父母交流，我们必须调用一个从父母传给孩子的函数。

例如，我们可以写:

```
import React from "react";const Greeting = ({ greeter }) => greeter("james");export default function App() {
  return <Greeting greeter={name => <div>Hello, {name}!</div>} />;
}
```

我们传入一个`greeter`道具，这是一个从`App`到`Greeting`的函数，我们在那里调用它来呈现它返回的元素。

![](img/e9f3b412f9294665ee22b6372f6e82b8.png)

Photo by [Alvan Nee](https://unsplash.com/@alvannee?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用高阶组件将逻辑从表示中分离出来。

它们可以拥有在多个组件中重用的逻辑。

还有一些库可以帮助我们分离逻辑。

React 的数据流是单向的，所以我们总是要把道具从父母传给孩子。

但是子组件可以调用父组件传递的函数。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在 [**plainenglish.io**](https://plainenglish.io/) 和 [**找到它们订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**