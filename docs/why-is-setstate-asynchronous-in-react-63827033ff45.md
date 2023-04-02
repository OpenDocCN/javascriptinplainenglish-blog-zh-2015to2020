# 为什么在 React 中 setState()是异步的？

> 原文：<https://javascript.plainenglish.io/why-is-setstate-asynchronous-in-react-63827033ff45?source=collection_archive---------2----------------------->

## 还是同步的？

![](img/91484f343bd92fc3f883898652236cda.png)

Photo by [bruce mars](https://unsplash.com/@brucemars?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 问题是

在 React 中，我们使用`setState()`来更新任何组件的状态。现在`setState()`不会立即改变这个状态，而是创建一个挂起的状态转换。调用`setState()`后立即访问状态会返回现有值，而不是更新后的值。作为 React 的初学者，我相信我们大多数人都会经常面临这个问题。

不能保证`setState()`上的同步操作，为了提高性能，可能会批量调用。人们很容易忘记`setState()`是异步的，这使得我们很难调试代码中的问题。`setState()`也不回承诺。使用`async` / `await`或任何类似的方法都不起作用。还有另一种情况，我们倾向于在同一个块中使用多个`setState()`函数，有时状态没有正确更新。

# 可能的解决方案

我们经常用一个参数调用`setState()`，但实际上，该方法支持两个参数。您可以传递的第二个参数是一个回调函数，它总是在状态更新后执行。

让我们来看一个问题的例子:

```
...
state = {value: 5}
...
...// --WRONG WAY-- //this.setState({value: this.state.value + 1})
console.log(this.state.value) // Prints 5 and not 6
```

编写更好的`setState`函数的解决方案是:

```
// --RIGHT WAY-- //this.setState((prevState) => ({
   value: prevState.value +1}), () => {
   console.log(this.state.value)});
// Provide a callback to setState as second argument
```

上面给出了更新状态和访问更新值的正确方法。我们将一个回调函数作为第二个参数传递给在状态更新后被调用的`setState()`函数。还应该注意，使用函数作为第一个参数来返回对象是编写 React 代码的一种非常好的方式，因为您可以访问状态的当前值(`prevState`)。在一个块中使用一个`setState()`函数来避免状态更新时出现问题也是一个很好的做法。

# 为什么会这样？

如果你看一下 React 的代码库中`setState()`函数内部的代码，你会发现`setState()`根本不是一个异步函数，它总是同步的。只是它在后台更新时调用了 **enqueueState** 或 **enqueueCallback** ，因此它的执行感觉像是异步的。

因此，真正的同步或异步是 React 中调用`setState()`的效果——[**协调**](https://reactjs.org/docs/reconciliation.html) 算法进行虚拟 DOM 比较，并调用 render 来更新真实 DOM。

出于性能原因，React 批量更新并每帧刷新一次。然而，在某些情况下，React 无法控制批处理，因此更新是同步可用的，例如在 AJAX、`setTimeOut()`等中。

# 结论

通常，更新状态发生在下一次渲染时，但有时为了提高性能，可以进行批处理。这种批处理可能不受 React 控制，因此`setState()`可能不总是连续的。因此，如果您想要对状态进行顺序更新，请使用第二个参数。

我希望你能从中学到一些新的东西！感谢阅读！