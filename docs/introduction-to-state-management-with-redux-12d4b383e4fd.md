# Redux 状态管理简介

> 原文：<https://javascript.plainenglish.io/introduction-to-state-management-with-redux-12d4b383e4fd?source=collection_archive---------8----------------------->

![](img/04e6e57a97fa84be81b101094deb5539.png)

Photo by [Kit Suman](https://unsplash.com/@cobblepot?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

有了 Redux，我们可以用它在 JavaScript 应用程序的中央位置存储数据。它可以单独工作，当与 React-Redux 结合使用时，它也是一个受欢迎的 React 应用程序状态管理解决方案。

在本文中，我们将了解如何在应用程序中创建一个基本商店并使用它。

# 装置

Redux 可作为 NPM 包。我们可以通过运行以下命令来安装它:

```
npm install --save redux
```

与 NPM 或:

```
yarn add redux
```

用纱线。

# 基本示例

我们可以使用 redux 通过创建一个 store 和 reducer 来存储一个 app 的状态。

然后，我们可以订阅存储来获取数据，并调度操作来更改状态。

例如，我们可以写:

```
import { createStore } from "redux";function counter(state = 0, action) {
  switch (action.type) {
    case "INCREMENT":
      return state + action.payload || 1;
    case "DECREMENT":
      return state - action.payload || 1;
    default:
      return state;
  }
}let store = createStore(counter);
store.subscribe(() => console.log(store.getState()));store.dispatch({ type: "INCREMENT", payload: 2 });
store.dispatch({ type: "DECREMENT", payload: 1 });
```

在上面的代码中，我们有`counter` reducer 函数，它获取动作类型和有效负载，然后相应地应用它。

`counter`函数检查动作类型`INCREMENT`和`DECREMENT`，并返回添加了`payload`的新状态。

然后，我们通过运行以下命令来创建`store`:

```
let store = createStore(counter);
```

之后，我们可以通过调用商店上的`subscribe`来订阅商店，然后在回调中调用`getState`，如下所示:

```
store.subscribe(() => console.log(store.getState()));
```

然后我们可以通过用一个带有`type`和`payload`的对象调用商店上的`dispatch`来分派动作，这些动作将被传递到`action`参数中的`counter`函数中。

因此，`state`将被`action`更新，因为对于第一个`dispatch`调用:

```
store.dispatch({ type: "INCREMENT", payload: 2 });
```

`type`将作为`action.type`传入，有效载荷将设置为`action.payload`至`counter`。

`action.type`是`‘INCREMENT’`，`action.payload`是 2。

同样，在第二次调用中，`action.type`为`‘DECREMENT’`，`action.payload`为 1。

注意，我们没有在`counter`中直接改变状态。我们通过从现有状态派生出一个新状态来返回它。

# 冗余的三个原则

三个原则适用于 Redux 的构建方式。

## 真理的单一来源

Redux 将状态存储在一个地方。这使得获取数据变得容易，因为我们只需要在一个地方获取数据。

因此调试也很容易。这使得开发速度更快。

一些以前很难实现的功能现在变得很容易，因为现在状态存储在一个单独的树中。

## 状态为只读

该状态是只读的，因此不会被意外更改。我们必须表达改变国家的意图。

没有微妙的竞争条件需要注意，因为所有的变化都是集中的，并以严格的顺序一个接一个地发生。

因为动作只是普通的对象，所以它们可以被记录、序列化、存储和重放，用于调试和测试目的。

## 变化是由纯函数产生的

Reducers 是纯函数，它接受前一个状态和动作，并返回下一个状态。我们返回新的状态对象，而不是改变以前的状态。

我们可以从单个缩减器开始，然后将它分成管理状态树其他部分的更小的缩减器。

我们可以控制如何调用它们，传递额外的数据，或者制造可重用的 reducers。

# 通量架构

Redux 将所有模型更新逻辑集中在一个 app 的一层。这是 Redux 采用的 Flux 架构的一部分。

Redux 没有调度员。它依靠纯函数而不是事件发射器来改变状态。

纯函数很容易组合，不需要额外的管理。

Redux 假设我们从不改变我们的数据。我们应该总是返回新的对象。这很容易，因为我们有了对象的 spread 操作符。

![](img/a6f3c58c098f7f129d069f28b1803b92.png)

Photo by [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# RxJS

我们可以通过订阅 Redux 存储并调用`nexr`来返回最新状态，从而将 Redux 存储转换为 Rxjs 可观察存储。

例如，我们可以写:

```
import { createStore } from "redux";function counter(state = 0, action) {
  switch (action.type) {
    case "INCREMENT":
      return state + action.payload || 1;
    case "DECREMENT":
      return state - action.payload || 1;
    default:
      return state;
  }
}let store = createStore(counter);function toObservable(store) {
  return {
    subscribe({ next }) {
      const unsubscribe = store.subscribe(() => next(store.getState()));
      next(store.getState());
      return { unsubscribe };
    }
  };
}const counterObservable = toObservable(store);counterObservable.subscribe({
  next(count) {
    console.log(count);
  }
});store.dispatch({ type: "INCREMENT", payload: 2 });
store.dispatch({ type: "DECREMENT", payload: 1 });
```

在上面的代码中，我们有:

```
function toObservable(store) {
  return {
    subscribe({ next }) {
      const unsubscribe = store.subscribe(() => next(store.getState()));
      next(store.getState());
      return { unsubscribe };
    }
  };
}const counterObservable = toObservable(store);counterObservable.subscribe({
  next(count) {
    console.log(count);
  }
});store.dispatch({ type: "INCREMENT", payload: 2 });
store.dispatch({ type: "DECREMENT", payload: 1 });
```

`toObservable`返回一个我们可以订阅的可观察值，因为它返回一个`subscribe`方法。

Redux store 的`subscribe`方法返回一个`unsubscribe`函数，这样当我们不再需要它时就可以取消订阅。

然后，当我们调用上面的`dispatch`时，我们将获得记录在上面的`next`函数中的新值。

# 结论

我们可以使用 Redux 将应用程序的数据存储在一个中心位置。为此，我们创建一个存储，然后订阅它以从中获取最新状态。

然后，我们用一个带有动作类型和有效负载的普通对象对存储调用`dispatch`,将存储更改为最新状态。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****