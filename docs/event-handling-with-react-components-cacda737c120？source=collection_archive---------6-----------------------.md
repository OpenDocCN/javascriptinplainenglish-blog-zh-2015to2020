# 用 React 组件处理事件

> 原文：<https://javascript.plainenglish.io/event-handling-with-react-components-cacda737c120?source=collection_archive---------6----------------------->

![](img/854666d4a6b892c42154434eebc8b3e0.png)

Photo by [Alasdair Elmes](https://unsplash.com/@alelmes?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建前端视图的库。它有一个庞大的图书馆生态系统与之合作。此外，我们可以用它来增强现有的应用程序。

在本文中，我们将研究如何在 React 组件中处理事件。

# 处理事件

在 React 元素中处理事件与在 DOM 元素中处理事件非常相似。

不同之处在于事件侦听器名称是 camelCase 而不是小写，并且我们传入一个函数作为事件处理程序，而不是一个字符串。

例如，我们可以添加一个按钮来显示警告框，如下所示:

```
import React from "react";
import ReactDOM from "react-dom";class App extends React.Component {
  constructor(props) {
    super(props);
  } showAlert() {
    alert("hello");
  } render() {
    return (
      <div>
        <button onClick={this.showAlert}>Click Me</button>
      </div>
    );
  }
}
```

在上面的代码中，我们有一个`showAlert`方法 show runs `alert(“hello”);`来显示一个带有 hello 的警告。

在`render`方法中，我们有一个`button` React 元素，它有一个采用`this.showAlert`方法的`onClick`属性。

请注意，我们传入了对该方法的引用，因为我们不会立即调用它。只有当我们点击“点击我”按钮时，它才会被调用。

我们不能返回`false`来防止 React 中的默认行为。我们必须显式调用`preventDefault`。

为了在 React 中调用它，我们编写:

```
showAlert(e) {
  e.preventDefault();
  alert("hello");
}
```

其中`e`是根据 [W3C 规范](https://www.w3.org/TR/DOM-Level-3-Events/)定义的合成事件对象。

创建 DOM 元素后，我们通常不需要调用`addEventListener`来添加侦听器。我们只是像上面一样在组件中提供侦听器。

正如我们所见，我们创建了`showAlert`方法作为按钮的事件处理程序。这是在 React 基于类的组件中创建事件处理程序的一种普遍接受的模式。

例如，我们可以创建一个带有按钮的组件，该组件可以切换按钮中的文本，如下所示:

```
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = { on: true };
  } handleClick() {
    this.setState({
      on: !this.state.on
    });
  } render() {
    return (
      <button onClick={this.handleClick.bind(this)}>
        {this.state.on ? "ON" : "OFF"}
      </button>
    );
  }
}
```

在上面的代码中，我们将`this.handleClick.bind(this)`传递给`onClick`属性，再传递给`button`元素。

我们需要`bind(this)`，这样`this`值将成为`this.setState`方法调用中的外部组件。

当我们点击按钮时，`handleClick`被调用，它通过`this.setState`调用用相反的值更新`this.state.on`。

或者，我们可以这样写`handleClick`:

```
handleClick() {
  this.setState(state => ({
    on: !state.on
  }));
}
```

当我们使用`this`时，为了避免调用`bind`，我们可以使用一个箭头函数来代替，因此我们可以写:

```
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = { on: true };
  } handleClick = () => {
    this.setState({
      on: !this.state.on
    });
  }; render() {
    return (
      <button onClick={this.handleClick}>{this.state.on ? "ON" : "OFF"}</button>
    );
  }
}
```

这样做是因为 arrow 函数没有自己的`this`，所以我们不必将`handleClick`函数内的`this`改为带有`bind(this)`的`App`类。

我们还可以传入一个调用事件监听器的函数。

所以我们可以这样修改它:

```
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = { on: true };
  } handleClick = () => {
    this.setState({
      on: !this.state.on
    });
  }; render() {
    return (
      <button onClick={e => this.handleClick(e)}>
        {this.state.on ? "ON" : "OFF"}
      </button>
    );
  }
}
```

`e => this.handleClick(e)`是一个调用`this.handleClick`方法的函数，传递了`e`事件对象。

这也避免了调用`bind(this)`的需要。然而，它的效率较低，因为我们每次运行`render`时都返回一个新的事件处理函数。

![](img/82c9bcb58d9e3d860bdff1816d2e224d.png)

Photo by [britt gaiser](https://unsplash.com/@brittanyg?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 将参数传递给事件处理程序

我们可以通过将参数作为`bind`的第二个参数传入来将参数传入事件处理程序，或者我们可以使用传入的参数返回一个事件处理程序函数。

例如，我们可以写:

```
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = { val: 0 };
  } handleClick(val) {
    this.setState({
      val
    });
  } render() {
    return (
      <button onClick={this.handleClick.bind(this, Math.random())}>
        {this.state.val}
      </button>
    );
  }
}
```

在代码中，我们将`Math.random()`作为`handleClick`方法的`val`参数的值传入。

我们也可以这样写:

```
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = { val: 0 };
  } handleClick(val, e) {
    e.preventDefault();
    this.setState({
      val
    });
  } render() {
    return (
      <button onClick={e => this.handleClick(Math.random(), e)}>
        {this.state.val}
      </button>
    );
  }
}
```

注意`Math.random()`在第一个参数中，而`e`在第二个参数中，但是反过来也可以，因为我们可以返回我们喜欢的任何函数。

# 结论

我们可以将事件侦听器附加到 React 元素，就像我们为 HTML DOM 元素设置事件侦听器一样。

事件名称是大写字母，而不是小写字母。如果我们的事件处理器不是一个箭头函数，我们必须调用`bind`来改变`this`的值到我们自己的组件类。

我们还可以传入一个调用另一个函数作为事件处理程序的函数。

要将参数传入事件处理程序，我们可以将它作为第二个和后续的参数传入`bind`。

如果我们传入一个调用另一个函数作为事件处理程序的函数，那么我们可以传入任何我们喜欢的函数，因为我们自己定义了这个函数。