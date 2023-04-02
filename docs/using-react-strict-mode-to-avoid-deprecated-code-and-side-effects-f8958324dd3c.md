# 使用 React Strict 模式避免不推荐使用的代码和副作用

> 原文：<https://javascript.plainenglish.io/using-react-strict-mode-to-avoid-deprecated-code-and-side-effects-f8958324dd3c?source=collection_archive---------1----------------------->

![](img/f87f2a38cebc7c4dd428ec7f6923344f.png)

Photo by [Julian Friedle](https://unsplash.com/@introoke?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建前端视图的库。它有一个庞大的图书馆生态系统与之合作。此外，我们可以用它来增强现有的应用程序。

在本文中，我们将研究如何使用 React 的严格模式来获得关于开发过程中不推荐使用的 API 和副作用的额外警告。

# 严格模式

严格模式是一种突出应用程序中潜在问题的工具。它不呈现任何可见的用户界面。

它只用于为它的后代激活额外的检查和警告。

严格模式不影响生产版本。

我们可以向 React 应用程序添加严格模式，如下所示:

```
class App extends React.Component {
  render() {
    return (
      <div>
        <p>foo</p>
        <React.StrictMode>
          <p>bar</p>
        </React.StrictMode>
      </div>
    );
  }
}
```

在上面的代码中，带有‘foo’的`p`标签没有被严格模式检查，因为它在`React.StrictMode`组件之外，但是里面的`p`元素被严格模式检查。

# 识别不安全的生命周期

严格模式检查不安全的生命周期。一些生命周期方法正在被否决，因为它们鼓励不安全的编码实践。

它们是:

*   `componentWillMount`
*   `componentWillReceiveProps`
*   `componentWillUpdate`

前缀`UNSAFE_`将在即将发布的版本中添加。

2 新的生命周期方法正在取代上述方法。它们是静态的`getDerivedStateFromProps`和`getSnapshotBeforeUpdate`方法。

`getSnapshotBeforeUpdate`在进行突变之前调用 lifecycle，其返回值将作为第三个参数传递给`componentDidUpdate`。

`getSnapshotBeforeUpdate`和`componentDidUpdate`一起涵盖了`componentWillUpdate`的所有用例。

严格模式将警告旧的生命周期钩子的废弃。

# 关于旧字符串引用 API 用法的警告

此外，React strict 模式将警告在我们的代码中使用字符串引用。

不推荐使用字符串引用，因为它们不能被静态类型化。它们需要始终保持一致。神奇的动态字符串打破了虚拟机中的优化，它只在一个层面上有效。

我们不能像回调和对象引用那样传递它。

因此，它会警告我们关于字符串引用的用法，因为它们已被弃用。

将来还支持回调引用和`createRef`。

# 关于已弃用的 findDOMNode 用法的警告

不推荐使用`findDOMNode`方法。我们可以用它来搜索给定类实例的 DOM 节点。

我们不需要这样做，因为我们可以将一个引用附加到一个 DOM 节点。

只返回第一个孩子，但是一个组件可以使用片段来呈现多个 DOM 级别。

因此，它现在不是很有用，因为它只搜索一个级别，我们可以使用 ref 来获得我们正在寻找的确切元素。

我们可以在包装元素上附加一个`ref`:

```
class App extends React.Component {
  constructor(props) {
    super(props);
    this.wrapper = React.createRef();
  }
  render() {
    return <div ref={this.wrapper}>{this.props.children}</div>;
  }
}
```

如果我们不想包装`div`来呈现，如果我们不想让节点成为布局的一部分，我们可以在 CSS 中设置`display: contents`。

![](img/f3d34e800848718c643e0b7a7096b98b.png)

Photo by [Timo Stern](https://unsplash.com/@timonrets?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 检测意外的副作用

React 分两个阶段工作:

*   呈现阶段对 DOM 进行任何更改。React 在这个阶段调用`render`，然后将结果与之前的渲染进行比较。
*   提交阶段运行任何生命周期方法来应用任何所需的更改。

渲染阶段生命周期包括以下类组件方法:

*   `constructor`
*   `componentWillMount`
*   `componentWillReceiveProps`
*   `componentWillUpdate`
*   `getDerivedStateFromProps`
*   `shouldComponentUpdate`
*   `render`
*   `setState`

因为它们可能被调用不止一次，所以它们不应该产生任何副作用。忽略这条规则可能会导致内存泄漏和应用程序状态无效等问题。

严格模式通过运行以下方法两次来检查是否有副作用:

*   类组件`constructor`方法
*   `render`方法
*   `setState`
*   静态`getDerivedStateFromProps`生命周期

# 检测旧上下文 API

严格模式将检测旧上下文 API 的使用。它将在未来版本中被删除。如果它是用过的，我们应该搬到新的。

# 结论

我们可以使用严格模式来检测弃用的生命周期方法、遗留上下文 API、字符串引用和一些会产生意外副作用的代码的使用。

它只显示开发中的警告，不影响生产代码。