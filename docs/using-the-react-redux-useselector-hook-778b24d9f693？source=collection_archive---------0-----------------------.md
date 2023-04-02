# 使用 React-Redux useSelector 挂钩

> 原文：<https://javascript.plainenglish.io/using-the-react-redux-useselector-hook-778b24d9f693?source=collection_archive---------0----------------------->

![](img/fece6f40b4ff9b7cc6408df24e16fea0.png)

Photo by [Clark Street Mercantile](https://unsplash.com/@mercantile?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

有了 Redux，我们可以用它在 JavaScript 应用程序的中央位置存储数据。它可以单独工作，当与 React-Redux 结合使用时，它也是一个受欢迎的 React 应用程序状态管理解决方案。

在本文中，我们将看看新的钩子 React-Redux 钩子 API。

# 在 React Redux 应用程序中使用 useSelector 挂钩

钩子 API 由`useSelector`、`useDispatch`和`useStore`钩子组成。

`useSelector`挂钩使用一个`selector`函数从存储中选择数据，并使用另一个函数`equalityFn`在返回结果之前对它们进行比较，并确定如果来自前一状态和当前状态的数据不同，何时进行渲染。

该调用采用以下格式:

```
const result : any = useSelector(selector : Function, equalityFn? : Function)
```

相当于 connect 中的`mapStateToProps`。将以整个 Redux 存储状态作为唯一参数来调用选择器。

选择器函数可以返回任何值作为结果，而不仅仅是一个对象。选择器的返回值将作为`useSelector`钩子的返回值。

当一个动作被分派时，`useSelector`将对先前的选择器结果值和当前值做一个简单的比较。

`selector`函数不接受`ownProps`参数。道具可以通过关闭或使用 curried 选择器来使用。

我们可能在一个函数组件中多次调用`useSelector`。每个调用都会创建一个对 Redux store 的单独订阅。React 更新批处理行为将导致同一组件中的多个`useSelector`返回新值，应该只在一次重新呈现中返回。

# 相等比较和更新

将调用提供的选择器函数，其结果将从`useSelector`钩子返回。如果选择器运行并返回相同的结果，则可能会返回缓存的结果。

`useSelector`仅当选择器结果与上一次结果不同时，才强制重新渲染。

这与`connect`不同，后者对`mapState`调用的结果使用浅层相等检查来确定是否需要重新渲染。

如果我们想从存储中检索多个值，我们可以多次调用`useSelector`,每次调用返回一个字段值，使用 Reselect 或类似的库来创建一个内存化的选择器，该选择器返回一个对象中的多个值，但当其中一个值发生变化时返回一个新的对象。

我们还可以使用 React-Redux 中的`shallowEqual`函数作为`useSelector`的`equalityFn`参数。

例如，我们可以如下使用`useSelector`:

```
import React from "react";
import ReactDOM from "react-dom";
import { Provider, useSelector, useDispatch } from "react-redux";
import { createStore } from "redux";
function count(state = 0, action) {
  switch (action.type) {
    case "INCREMENT":
      return state + 1;
    case "DECREMENT":
      return state - 1;
    default:
      return state;
  }
}const store = createStore(count);function App() {
  const count = useSelector(state => state);
  const dispatch = useDispatch(); return (
    <div className="App">
      <button onClick={() => dispatch({ type: "INCREMENT" })}>Increment</button>
      <button onClick={() => dispatch({ type: "DECREMENT" })}>Decrement</button>
      <p>{count}</p>
    </div>
  );
}const rootElement = document.getElementById("root");
ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  rootElement
);
```

在上面的代码中，我们像往常一样创建了商店。然后在`App`组件中，我们使用`useSelector`钩子通过传入`state => state`函数来获取`count`。

然后我们将`dispatch`函数传递给`useDispatch`。

我们可以使用`useSelect`钩子的第二个参数如下:

```
import React from "react";
import ReactDOM from "react-dom";
import { Provider, useSelector, useDispatch, shallowEqual } from "react-redux";
import { createStore } from "redux";
function count(state = 0, action) {
  switch (action.type) {
    case "INCREMENT":
      return state + 1;
    case "DECREMENT":
      return state - 1;
    default:
      return state;
  }
}
const store = createStore(count);
function App() {
  const count = useSelector(state => state, shallowEqual);
  const dispatch = useDispatch();
  return (
    <div className="App">
      <button onClick={() => dispatch({ type: "INCREMENT" })}>Increment</button>
      <button onClick={() => dispatch({ type: "DECREMENT" })}>Decrement</button>
      <p>{count}</p>
    </div>
  );
}
const rootElement = document.getElementById("root");
ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  rootElement
);
```

React-Redux 有一个`shallowEqual`函数来比较数据，以确定何时更新数据。我们还可以定义自己的函数来比较以前和当前的状态，以确定更新。

![](img/d0994d962a6b109b9768fd92cd82781e.png)

Photo by [Artem Gavrysh](https://unsplash.com/@tmwd?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用`useSelector`钩子从 React 组件中的 Redux 存储中获取数据。

它需要两个参数。第一个参数是返回状态的函数，第二个参数是检查前一个和当前状态是否相等以确定何时更新的函数。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****