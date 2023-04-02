# 用 MobX-React 对可观察到的事物做出反应

> 原文：<https://javascript.plainenglish.io/reacting-to-observables-with-mobx-react-96ec97e5f9c2?source=collection_archive---------2----------------------->

![](img/a0104e5a15e589be34fc957a2dab9946.png)

Photo by [Jens Johnsson](https://unsplash.com/@jens_johnsson?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们可以将 MobX 与 MobX-React 一起使用，通过观察 MobX 可观察对象和更改可观察数据来管理状态。

在本文中，我们将了解如何创建一个可观察对象，并在 React 组件中直接使用它。

# 创建可观察对象并在 React 组件中使用它们

我们必须安装`mobx`和`mobx-react`包来创建可观察对象，并在 React 组件中使用它们。

为此，我们可以编写以下代码:

```
npm i mobx mobx-react
```

然后，我们可以创建我们的可观察对象和一个使用它的 React 组件，如下所示:

```
import React from "react";
import ReactDOM from "react-dom";
import { observer } from "mobx-react";
import { observable } from "mobx";const countData = observable({
  count: 0
});const App = observer(({ countData }) => (
  <>
    <button onClick={() => countData.count++}>Increment</button>
    <p>{countData.count}</p>
  </>
));
const rootElement = document.getElementById("root");
ReactDOM.render(<App countData={countData} />, rootElement);
```

在上面的代码中，我们通过编写以下内容创建了`countData`可观察对象:

```
const countData = observable({
  count: 0
});
```

我们将能够获得 React 组件中的最新状态，然后将它作为 React 组件的道具传入。

为了让我们的 React 组件监视来自可观察对象的最新值，并让我们在组件内部更改它的值，我们编写:

```
const App = observer(({ countData }) => (
  <>
    <button onClick={() => countData.count++}>Increment</button>
    <p>{countData.count}</p>
  </>
));
const rootElement = document.getElementById("root");
ReactDOM.render(<App countData={countData} />, rootElement);
```

上面的代码从可观察的`countData`中得到`countData`道具。然后在`onClick`处理程序中，我们只是传递函数来改变它的值。

然后，当我们引用最后一行中的`App`组件时，我们只是将`countData`可观察对象作为`countData`道具的值传入。

`countData.count++`将改变可观察的状态，然后从道具中获得最新值。

我们可以使用带有类组件的`countData`可观察对象，如下所示:

```
import React from "react";
import ReactDOM from "react-dom";
import { observer } from "mobx-react";
import { observable } from "mobx";const countData = observable({
  count: 0
});@observer
class App extends React.Component {
  render() {
    return (
      <>
        <button onClick={() => countData.count++}>Increment</button>
        <p>{countData.count}</p>
      </>
    );
  }
}const rootElement = document.getElementById("root");
ReactDOM.render(<App countData={countData} />, rootElement);
```

唯一的区别是它是一个具有 render 方法的类组件，并且我们使用了`observer` decorator 而不是`observer`函数。

# 使用上下文传递观察值

我们可以使用 React 上下文 API 将可观察对象的值传递给另一个组件。

例如，我们可以编写以下代码:

```
import React, { useContext } from "react";
import ReactDOM from "react-dom";
import { observer } from "mobx-react";
import { observable } from "mobx";const countData = observable({
  count: 0
});const CountContext = React.createContext();const Counter = observer(() => {
  const countData = useContext(CountContext);
  return (
    <>
      <button onClick={() => countData.count++}>Increment</button>
      <p>{countData.count}</p>
    </>
  );
});const App = ({ countData }) => (
  <CountContext.Provider value={countData}>
    <Counter />
  </CountContext.Provider>
);const rootElement = document.getElementById("root");
ReactDOM.render(<App countData={countData} />, rootElement);
```

在上面的代码中，我们有`CountContext`。我们通过书写来创造它:

```
const CountContext = React.createContext();
```

然后在`App`中，我们包含了`CountContext`组件，这样我们就可以通过传入`countData`属性来获得`countData`可观察值，该属性的值设置为`countData`可观察值。

在`Counter`组件中，我们调用内部有函数组件的`observer`函数。

在组件内部，我们使用`useContext`钩子从`CountContext`获取值。

然后在 handler 函数中，我们传入按钮的`onClick`属性，我们改变了`count`的值，这将自动改变`countData`可观察对象中的值，并设置为`countData.count`的值。

因此，当我们点击增量按钮时，它下面的数字会上升。

![](img/9331fc4ae2235b21490abbb2cd117b19.png)

Photo by [Max Rovensky](https://unsplash.com/@fivepointseven?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 在本地组件状态中存储可观察值

我们还可以在 React 组件中使用 Observables 作为本地状态。

为此，我们可以通过传入一个函数来使用`useState`钩子，该函数返回一个可观察的对象，如下所示:

```
import React, { useState } from "react";
import ReactDOM from "react-dom";
import { observer } from "mobx-react";
import { observable } from "mobx";const App = observer(() => {
  const [countData] = useState(() =>
    observable({
      count: 0
    })
  ); return (
    <>
      <button onClick={() => countData.count++}>Increment</button>
      <p>{countData.count}</p>
    </>
  );
});
const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

在上面的代码中，我们通过传入一个函数来使用`useState`钩子，该函数返回一个具有`count`属性的可观察对象。然后我们将它赋给了`countData`变量。

一旦我们这样做了，`countData.count`将被`onClick`处理程序更新，当我们单击 Increment 按钮时，我们将看到`countData.count`值自动更新。

如果我们单击“增量”按钮，那么值将会上升。

我们真的不需要使用 MobX 可观察对象来存储本地状态，除非涉及复杂的计算，因为 MobX 会针对这些进行优化。

同样，我们可以使用 MobX 来存储带有类组件的本地状态，如下所示:

```
import React, { useState } from "react";
import ReactDOM from "react-dom";
import { observer } from "mobx-react";
import { observable } from "mobx";@observer
class App extends React.Component {
  @observable count = 0; render() {
    return (
      <>
        <button onClick={() => this.count++}>Increment</button>
        <p>{this.count}</p>
      </>
    );
  }
}
const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

在上面的代码中，我们有`this.count`，这是一个可观察的字段。在`onClick`监听器中，我们只需直接修改`this.count`，然后它就会反映在`p`标签之间的`this.count`中。

`@observer`自动执行`memo`或`shouldComponentUpdate`，这样就不会有任何不必要的重新渲染。

# 结论

我们可以使用 MobX Observable 来存储 React 应用程序的状态或 React 组件的状态。

为了使用它来存储 React app 的状态，我们可以将一个可观察对象作为一个属性传递给一个组件，或者我们可以使用上下文 API 将数据发送给不同的组件。

我们还可以通过在函数组件中使用`useState`钩子和在类组件中使用`observable`装饰器来使用它存储本地状态。

## JavaScript 用简单的英语写的一个注释:

我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，**用你的中级用户名发送电子邮件到 submissions@javascriptinplainenglish.com**[](mailto:submissions@javascriptinplainenglish.com)**，我们会将你添加为作者。**

**我们还推出了三种新的出版物！请关注我们的新出版物: [**AI in Plain English**](https://medium.com/ai-in-plain-english) ，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！****