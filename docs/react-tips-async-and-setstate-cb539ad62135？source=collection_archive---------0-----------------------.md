# 反应提示—异步和设置状态

> 原文：<https://javascript.plainenglish.io/react-tips-async-and-setstate-cb539ad62135?source=collection_archive---------0----------------------->

![](img/9daedda805eeeba7ddb4f6c399d9b320.png)

Photo by [Kev Kindred](https://unsplash.com/@theotherkev?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是构建现代交互式前端 web 应用最常用的前端库。它还可以用来构建移动应用程序。

在本文中，我们将看看`setState`的异步特性，以及我们应该如何编写代码来顺序运行多个`setState`调用。

# setState 的异步特性

`setState`方法是更新组件内部状态的方法。这是一个异步的批处理方法。这意味着在用新状态重新呈现组件之前，多个`setState`调用被批处理。

`setState`不会立即改变状态，而是创建一个挂起的状态事务。这意味着在调用`setState`之后立即访问`state`可能会返回旧值。

`setState`方法最多接受 2 个参数。我们通常只传入一个。第一个参数可以是用于更新状态的对象或回调。

第二个参数是一个函数，它总是在`setState`运行后运行。例如，我们可以在第二个参数中传递一个回调，如下所示:

```
import React from "react";class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  } update() {
    this.setState(
      ({ count }) => ({
        count: count + 1
      }),
      () => {
        this.setState(({ count }) => ({
          count: count + 2
        }));
      }
    );
  } render() {
    return (
      <>
        <button onClick={this.update.bind(this)}>Increment</button>
        <p>{this.state.count}</p>
      </>
    );
  }
}export default App;
```

在上面的代码中，我们有:

```
update() {
  this.setState(
    ({ count }) => ({
      count: count + 1
    }),
    () => {
      this.setState(({ count }) => ({
        count: count + 2
      }));
    }
  );
}
```

它调用`setState`一次，然后在回调中再次调用`setState`。这将确保一个在另一个之后运行。

因此，当我们点击增量时，`count`状态每次都会增加 3。

# setState 接受一个对象或函数

从上面的代码中我们可以看到，`setState`可以接受一个回调，该回调基于先前的状态返回新的状态。这对于更新基于先前状态的状态很有用。

在上面的代码中，新的`count`基于旧的`count`，所以传入回调比传入对象更合适，因为我们可以保证当原始状态作为回调参数传入时，它是最新的。

它还将一个`props`参数作为具有属性的第二个参数。例如，我们可以如下使用它:

```
import React from "react";class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  } update() {
    this.setState(({ count }, { incrementVal }) => ({
      count: incrementVal + count
    }));
  } render() {
    return (
      <>
        <button onClick={this.update.bind(this)}>Increment</button>
        <p>{this.state.count}</p>
      </>
    );
  }
}export default function App() {
  return <Counter incrementVal={5} />;
}
```

在上面的代码中，我们有一个`Counter`组件，它有一个`update`方法，就像我们在前面的例子中一样。但是这个 ti，e，`setState`方法接受一个有第二个参数的回调。它有道具对象。

我们可以像上面一样通过析构赋值语法来获取属性的值。此外，我们可以像访问任何其他 JavaScript 对象属性一样，使用点或括号符号来访问 prop 对象的属性。

因此，既然我们已经:

```
<Counter incrementVal={5} />
```

在`App`中，当我们点击增量按钮时，`Counter`中的`count`状态将更新 5，因为这是我们指定的。

我们传递给`setState`的最常见的实体可能是一个对象。我们只是传入一个对象，它有我们想要改变的状态属性。没有包括的将保持不变。

例如，如果我们写:

```
import React from "react";class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0, foo: "foo" };
  } update() {
    this.setState({ count: this.state.count + 1 });
  } render() {
    return (
      <>
        <button onClick={this.update.bind(this)}>Increment</button>
        <p>{this.state.count}</p>
        <p>{this.state.foo}</p>
      </>
    );
  }
}export default function App() {
  return <Counter />;
}
```

那么即使在运行`update`中的`this.setstate`之后，`this.state.foo`也具有值`'foo'`。

因此，当我们单击增量按钮时，无论我们单击多少次，都会看到“foo”显示。

![](img/c1e5edbbab015389d139a71a2933ce78.png)

Photo by [Ankush Nath Sehgal](https://unsplash.com/@ankushsehgal?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

`setState`通话可能不总是连续的。配料有时可能不受 React 控制。因此，如果我们想确保多个`setState`调用始终按顺序运行，我们应该运行回调中的第二个`setState`，它作为`setState`的第二个参数传入。

另外，`setState`可以把一个具有前一状态的对象或函数以及 props 对象分别作为第一和第二参数。

## **简明英语团队的笔记**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**此外，我们总是有兴趣帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****