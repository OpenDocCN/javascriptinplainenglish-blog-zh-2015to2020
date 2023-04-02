# React:为什么我的状态没有被更新？

> 原文：<https://javascript.plainenglish.io/react-why-is-my-state-not-being-updated-63a078db9c4e?source=collection_archive---------18----------------------->

## 一个常见的堆栈溢出问题

![](img/654ef94e0aa20e634075c506d1933865.png)

Photo by [noor Younis](https://unsplash.com/@nooryounis?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

所以我经常在 StackOverflow 中读到这个问题的一些变体，后面跟着下一个代码

```
const doSomethingWithTheState = () => {
  setState(newValue);
  console.log(state); // this prints the old value
};
```

而且我总是用同样的 [React 文档页面](https://reactjs.org/docs/react-component.html#setstate)的摘录来回答: *setState()并不总是立即更新组件。它可以批处理或推迟更新，直到以后。这使得在调用 setState()之后立即读取 this.state 成为一个潜在的陷阱。*

# 为什么 React 会这样处理？

由于更新状态将触发[协调](https://reactjs.org/docs/reconciliation.html)算法，并将导致重新渲染，React 将决定何时是更新状态的最佳时刻，以便保持良好的性能。

让我们再次检查我们的示例代码:

```
const doSomethingWithTheState = () => {
  setState(newValue);
  console.log(state); // this prints the old value
};
```

如果立即更新状态，这将意味着在到达第一行时调用协调算法，这将触发重新呈现器，为了能够继续执行下一行，React 将需要保持下一步执行什么的上下文，这将是昂贵的。这就是为什么 React 决定不这么做，而首先执行 *console.log* 的原因。

此外，如果您连续更新状态中的一个以上的值(调用 setState *n* 次),这将意味着执行 *n* 次算法和 *n* 次重新呈现器，您很快就会发现这对性能不利。

# 热到解决？

如果您需要使用更新的状态值，您有以下选项:

## 如果你正在使用一个[类组件](https://reactjs.org/docs/components-and-props.html#function-and-class-components)

使用一个[回调](https://reactjs.org/docs/react-component.html#setstate)函数，它将在应用更新后被调用

```
doSomethingWithTheState = () => {
  this.setState(newValue, state => {
    console.log(state); // this prints the updated value
  })
};
```

重新渲染后，所有状态变量将被更新为最近的值，并且将调用[**componentDidUpdate**](https://reactjs.org/docs/react-component.html#componentdidupdate)函数，在该函数中，您可以访问新值和以前的值。

```
componentDidUpdate(prevProps, prevState) {
  if (this.state !== prevState) {
    console.log(this.state); // this prints the updated value
  }
}
```

## 如果您正在使用[功能组件](https://reactjs.org/docs/components-and-props.html#function-and-class-components)

使用 [**useEffect**](https://reactjs.org/docs/hooks-reference.html#useeffect) 钩子指定要在依赖项数组中监视的值，如下一段代码所示:

```
const [state, setState] = useState()useEffect(() => {
  console.log(state); // this prints the updated value
}, [state]); // this will be triggered only when state value is different
```

# 结论

现在你知道你的状态不被更新的最常见原因了。

本文中讨论的所有信息都在 React 文档中，我知道开始做事情总是比阅读更令人兴奋，但是我向您保证，如果您正确使用该工具，您将会省去许多麻烦。