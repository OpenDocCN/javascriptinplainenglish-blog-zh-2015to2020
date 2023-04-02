# 反应模式—事件和动画

> 原文：<https://javascript.plainenglish.io/react-patterns-events-and-animations-b3980a318588?source=collection_archive---------11----------------------->

![](img/18278f1e501ea803170ac588d6a374a4.png)

Photo by [Jamie Street](https://unsplash.com/@jamie452?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解如何使用 React 处理事件和创建简单的动画。

# 事件

React 抽象了事件在不同浏览器中的工作方式，以便我们有一个一致的接口来编写事件处理代码。

它有一个合成事件的概念，合成事件是一个包装浏览器提供的原始事件的对象。

无论创建哪种浏览器，它都具有相同的属性。

为了将一个事件处理程序附加到一个节点，我们将一个 camelCased prop 传递到一个元素中。

例如，如果我们想要监听`keydown`事件，那么我们将一个事件处理函数传递给`onKeyDown`属性。

我们可以通过编写以下代码来创建一个按钮，在单击时执行某些操作:

```
import React from "react";export default function App() {
  return <button onClick={() => alert("clicked")}>click me</button>;
}
```

我们传入了一个在点击按钮时运行的函数。

此外，我们可以处理一个元素的多个事件。

例如，我们可以写:

```
import React from "react";export default function App() {
  return (
    <button
      onDoubleClick={() => console.log("double clicked")}
      onClick={() => console.log("clicked")}
    >
      click me
    </button>
  );
}
```

我们应该避免重复和减少样板文件，所以我们应该把两者合二为一。

为此，我们可以提取这两个函数，并将它们合并为一个，然后检查所触发事件的类型。

我们可以写:

```
import React from "react";export default function App() {
  const handleEvent = event => {
    switch (event.type) {
      case "click":
        console.log("clicked");
        break;
      case "dblclick":
        console.log("double clicked");
        break;
      default:
        console.log("unhandled", event.type);
    }
  }; return (
    <button onClick={handleEvent} onDoubleClick={handleEvent}>
      click me
    </button>
  );
}
```

我们有一个接受`event`参数的`handleEvent`函数，它是事件对象。

然后我们可以检查一下`type`的财产。

然后我们可以通过检查值来做一些事情。

# 参考文献

我们有时需要访问 React 呈现的 DOM 对象。

为此，我们可以给它分配一个 ref，然后在返回的 DOM 对象中调用 access 和 properties。

例如，我们可以写:

```
import React, { useRef } from "react";export default function App() {
  const inputRef = useRef(); return (
    <>
      <input ref={inputRef} />
      <button onClick={() => inputRef.current.focus()}>focus</button>
    </>
  );
}
```

我们有一个输入，我们通过使用`useRef`钩子给它分配了一个 ref。

然后我们有一个事件处理程序，当在输入元素上时调用`focus`。

`inputRef.current`拥有输入元素 DOM 对象。

当有更好的方法处理元素时，我们应该尽量避免引用。

# 动画片

我们可以向 React 组件添加动画。为此，我们使用 react-addons-CSS-transition-group 包。

要安装它，我们运行:

```
react-addons-css-transition-group
```

然后我们可以通过写来使用它:

```
import React from "react";
import CSSTransitionGroup from "react-addons-css-transition-group";
import "./styles.css";export default function App() {
  return (
    <>
      <CSSTransitionGroup
        transitionName="fade"
        transitionAppear
        transitionAppearTimeout={500}
      >
        <p>hello world</p>
      </CSSTransitionGroup>
    </>
  );
}
```

`styles.css`:

```
.fade-appear {
  opacity: 0.01;
}.fade-appear.fade-appear-active {
  opacity: 1;
  transition: opacity 1.5s ease-in;
}
```

然后，当我们加载组件时，我们在“hello world”上得到一个淡入过渡。

我们用`transitionName`道具为我们的动画设置类。

`transitionAppear`表示有东西显示时动画会运行。

`transitionAppearTimeout`是动画开始前的延迟时间，单位为毫秒。

所有的动画都是用 CSS 定义的。

![](img/0a483a02d09b94bde0f9811f68dafc46.png)

Photo by [Paweł Czerwiński](https://unsplash.com/@pawel_czerwinski?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

React 提供了一种统一的方式来处理来自浏览器的事件。

此外，我们可以使用附加包将动画添加到 React 组件中。