# 使用反应避免时的 4 个常见错误

> 原文：<https://javascript.plainenglish.io/4-common-mistakes-when-using-react-to-avoid-ed9de4307de5?source=collection_archive---------0----------------------->

## 你应该知道不要给你的代码库带来更多的问题

![](img/e68281fd55345207ea383700238cff67.png)

Photo by [Daniela Holzer](https://unsplash.com/@matscha?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

反应是当今一个伟大的图书馆。它简化了构建优美流畅的 web 应用程序的过程。

但是有一些常见的错误，如果你没有注意到，它会给你以后的项目带来负担。

没有任何进一步的麻烦，让我们深入了解在使用 reactor 时应该避免哪些错误。

# 1.创建超人组件

*超人*组件是可以做很多事情的组件。

我曾经从我的同事那里体验过这种成分。他将许多不相关的用户界面块放入一个组件中。然后，他使用一个属性来控制每个块的显示。类似于如果`viewId=0`然后显示`view_0`。

```
class SupermanComponent extends React.Component {
  render() {
    return (
      <div>
        <div>
          /* UI Block 1 */
        </div>
        <div>
          /* UI Block 2 */
        </div>
        <div>
          /* UI Block 3 */
        </div>
        <div>
          /* UI Block 4 */
        </div>
      </div>
    )
  }
}
```

起初你可能会认为它很方便。然而，当这个项目发展起来的时候，很快就会变得一团糟。更不用说在出现问题时进行调查需要时间。

一个组件应该只关注一件事。为了可重用性和可伸缩性，不要给它太多处理大量任务的能力。

例如，您应该限制导航栏组件的功能。应删除不相关的其他功能。通过这样做，您可以在任何需要的地方重用它。

不要因为将所有功能打包到一个组件中而偷懒。花时间想想你想要的组件应该是什么样子，并尽可能设计得独特。这些独特的组件更容易维护，在整个可组合系统中可以很好地协同工作。

块一组件:

```
class BlockOne extends React.Component {
  render() {
    return (
      <div>
        { /* Block 1 */ }
      </div>
    )
  }
}export default BlockOne;
```

块二组件:

```
class BlockTwo extends React.Component {
  render() {
    return (
      <div>
        { /* Block 2 */ }
      </div>
    )
  }
}export default BlockTwo;
```

块三组件:

```
class BlockThree extends React.Component {
  render() {
    return (
      <div>
        { /* Block 3 */ }
      </div>
    )
  }
}export default BlockThree;
```

# 2.将数字作为字符串传递

看看下面的组件:

```
class UserDetails extends React.Component {
  printGender() {
    if (this.props.gender === 0) {
      return ‘Male’;
    }

    if (this.props.gender === 1) {
      return ‘Female’;
    }

    return ‘Other’;
  } render() {
    return (
      <div>
        Gender: {printGender()}
      </div>
    )
  }
}
```

我们用`gender`属性作为数字来决定性别。`0`代表`Male`、`1`代表`Female`，以及与`0`和`1`不同的任何东西代表`Other`。

现在，让我们在某个地方使用`UserDetails`组件:

```
<UserDetails gender=“0” />
```

你期望性别会是`Male`但事实并非如此。因为你把`0`当作字符串而不是数字传递。因此，实际值将为`Other`。

若要解决此问题，请在传递数字作为道具时使用花括号:

```
<UserDetails gender={0} />
```

# 3.直接修改状态

初学者最常见的错误之一是直接修改状态。像这样:

```
showButton() {
  this.state.buttonVisibility = true;
}
```

您直接更改状态，而反应系统并不知道。因此，它可能不会触发重新渲染。

反应状态应该是不可变的。如果您试图直接修改它，可能会出现错误或意外行为，就像在上面的示例中没有重新呈现 UI 一样。

不要直接改变状态，应该用`setState()`。它会告诉 React，“嘿，伙计，我要改变一些东西。请注意。”然后，React 将做好一切准备，以正确应对变化。

```
showButton() {
  this.setState({ buttonVisibility: true });
}
```

# 4.在 setState()之后使用状态的值

```
showAvatar() {
  this.setState({avatarVisibility: true}); if (this.state.avatarVisibility) {
    // Do something
  }
}
```

你能指出上面片段的问题吗？

那就是`avatarVisibility`被`setState()`更新后马上就用了。

为什么这是一个问题？

这是一个您可能没有注意到的细微错误，但是`setState()`是异步工作的。

这意味着如果你使用`setState()`来更新一个属性，改变可能不会立即生效。这就是为什么在更新后使用产权会导致意想不到的行为。

但是，您可以通过使用`setState()`的第二个参数来解决这个问题

```
showAvatar() {
  this.setState({avatarVisibility: true}, () => {
    if (this.state.avatarVisibility) {
      // Do something
    }
  });
}
```

通过使用回调函数，您的计算将准确地工作，因为该回调函数将在状态更新后被调用。

# 结论

以上是您在使用 React 时可能会犯的常见错误。我希望这篇文章能帮助你避免所有这些错误，以确保一切工作正常。

# 进一步阅读

[](https://medium.com/javascript-in-plain-english/9-helpful-github-repositories-for-web-development-you-should-follow-right-now-d3c9f84d7e2b) [## 9 个对 Web 开发有用的 Github 库，你应该马上关注

### 不要只是用 Github 来拉和推你的代码。

medium.com](https://medium.com/javascript-in-plain-english/9-helpful-github-repositories-for-web-development-you-should-follow-right-now-d3c9f84d7e2b) 

加入我，了解更多关于编程的有用见解。