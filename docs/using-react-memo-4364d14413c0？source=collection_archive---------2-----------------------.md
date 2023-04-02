# React .备忘录解释道

> 原文：<https://javascript.plainenglish.io/using-react-memo-4364d14413c0?source=collection_archive---------2----------------------->

![](img/e294f68131d2012fbe1696201f826a51.png)

google.com

# **react . memo()是什么？**

React.memo 是一个函数，它接受一个函数作为参数，并返回一个记忆化的函数作为返回*或*换句话说，它是一个高阶组件，用于通过防止在属性没有改变的情况下重新呈现功能组件来提高该组件的性能。这类似于类组件中的纯组件。

它将一个函数作为可选的第二个参数，这类似于类组件中的`shouldComponentUpdate`生命周期方法，并返回一个布尔值，该值允许定制 memo 生效或不生效的条件。

> React.memo 在内部会对组件树中传递的每个组件的属性进行浅层比较，如果属性与之前的渲染相同，可以选择不重新渲染。

## 我们什么时候使用它？

在大多数情况下，我们在创建功能组件时不需要使用 memo。这是一种优化，应该只在必要的时候谨慎使用。话虽如此，但在某些情况下它确实是有帮助的。例如，如果子组件在父组件触发的事件上连续呈现。在这种情况下，我们可能不想过于频繁地渲染孩子，除非它的道具发生了变化。

## **下面举个例子来解释一下**

让我们看看下面的代码，我们可以看到一个名为`Inventory`的组件，它异步获取项目并在 UI 中更新它们。它有两个子组件，每个都是一个`Item`。

这里有一个重要的观察，即`updateItem1`和`updateItem2`没有绑定到组件，而是关联到组件实例的特定呈现，这意味着对于`Inventory`组件的重新呈现，会为这些方法中的每一个创建一个新的引用( *useCallback* 会防止这种情况发生，但我们稍后会谈到)。

Code to show a use-case for memoizing using React.memo()

当`updateItem1`被调用并且`item1`的值被重置时，由于上面的解释`Item1`和`Item2`都将更新。我们不希望这种情况发生，我们只希望`Item1`更新，因为`Item2`的值保持不变。

我们可以在 React.memo 中实现这个包装`Item`组件，确保组件只在`Item`的属性改变时更新。

但是请记住，对函数的引用作为道具传递给每次渲染时更新的`Item`？我们需要先解决这个问题。这时`useCallback`来救援了。

我们同样可以将函数`updateItem1`和`updateItem2`与组件的实例绑定，方法是将它们封装在一个名为`useCallback`的 react 钩子中，该钩子将充当类组件中的`bind(this)`，防止在每次渲染时创建新的引用。我们修好了！

就是这样！现在，我们的组件`Item2`将只在`item2`更新时呈现，如下所示。

Memoized solution to the problem above using useCallback and React.memo()

## **结论**

虽然这样的优化是好的，但是因为在依赖数组中保存道具副本的成本可能比实现的优化量高得多，所以应该小心使用，并且用在组件更新太快的地方，比如在动画或显示交互式图表/地图等的时候

要了解如何处理频繁变化的值，请点击链接 [**查看**](https://reactjs.org/docs/hooks-faq.html#how-to-read-an-often-changing-value-from-usecallback) **。**