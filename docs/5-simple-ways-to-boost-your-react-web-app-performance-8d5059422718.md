# 提高 React Web 应用程序性能的 5 种简单方法

> 原文：<https://javascript.plainenglish.io/5-simple-ways-to-boost-your-react-web-app-performance-8d5059422718?source=collection_archive---------9----------------------->

![](img/11b5c24798d48f09eea203c7eb1dec64.png)

Photo by [chuttersnap](https://unsplash.com/@chuttersnap?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Web 应用性能在为我们的用户带来最佳体验方面发挥着重要作用。下面是你现在可以用来优化 React 应用程序性能的 5 个技巧。

# 1.避免使用匿名函数

匿名函数通常在使用回调或事件处理程序时使用。

```
function renderSignInButton() {
  return (
    <button onClick={() => console.log(‘Sign In’)}>
      Sign In
    </button>
  );
}
```

匿名函数有时使用起来很方便。然而，这引起了一个问题。匿名函数不赋给变量。因此，每次组件被重新呈现时，都会分配一个新的内存区域来为匿名提供服务。

为了避免分配不必要的内存，我们应该使用命名函数:

```
const handleSignButtonClick = () => console.log(‘Sign In’);function renderSignInButton() {
  return (
    <button onClick={handleSignInButtonClick}>
      Sign In
    </button>
  );
}
```

这样，一个内存区域将只被分配一次。

# 2.避免使用内嵌样式

就像 CSS 一样，我们可以使用内联样式来样式化 React 组件。我们通常利用这种能力来快速设计一些组件。

```
function renderUser() {
  return (
    <User style={{background: ‘red’, size: 15}}></User>
  );
}
```

当我们过度使用它时，我们会生成重复的代码，这不仅增加了我们需要进行更改的时间，还会产生性能问题。

这个问题就像匿名函数一样，每次重新渲染都会分配新的内存。

我们可以用同样的方法来解决这个问题，命名该样式:

```
const userStyle = {
  background: ‘red’,
  size: 15
};function renderUser() {
  return (
    <User style={userStyle}></User>
  );
}
```

# 3.使用 React Virtualized 显示大数据

如果没有正确呈现，一长串数据会降低应用程序的速度。您应该在浏览器的可见视口中显示一些行。当您向下滚动或单击“下一步”按钮时，将显示下一行。

让这个过程更容易的一个方法是使用虚拟化的。

# 4.使用纯组件

我们使用**shouldcomponentdupdate**函数，通过比较当前数据和下一个数据来检查一个组件是否应该更新。如果数据很复杂，我们就要检查很多。

或者我们应该把它留给 PureComponent，这将减少不必要的重新渲染。

因此，与其说:

```
class User extends React.Component { shouldComponentUpdate(nextProps, nextState) {
    return nextState.data !== this.state.data;
  } render() {
    return (
      <div>
        <h1>Hello</h1>
      </div>
    )
  }}
```

我们应该这样做:

```
class User extends React.PureComponent {
  render() {
    return (
      <div>
        <h1>Hello</h1>
      </div>
    )
  }
}
```

# 5.使用 React。碎片

你能从下面的例子中看出问题吗？

```
class HelloComponent extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello {this.props.name}</h1>
        <h2>Start editing me</h2>
      </div>
    )
  }
}
```

父标签 **div** 有点多余。我的意思是，如果你移除它，将会发生错误，但是在渲染的时候有办法消除它。一种方法是使用 **React。片段**。

```
class HelloComponent extends React.Component { render() {
    return ( 
      <React.Fragment>
        <h1>Hello {this.props.name}</h1>
        <h2>Start editing me</h2>
      </React.Fragment>
    )
  }
}
```

甚至更简洁:

```
class HelloComponent extends React.Component { render() {
    return (
      <>
        <h1>Hello {this.props.name}</h1>
        <h2>Start editing me</h2>
      </>
    )
  }
}
```

你可能认为移除一个 HTML 标签可能不会改变太多，但是你的项目并不是那么小，对吗？这意味着您可以通过消除所有不必要的标签来削减大量渲染成本。

# 结论

有很多方法可以提高 React 应用程序的性能，但以上是我现在正在使用的方法。

我还在学习过程中。我认为优化 React app 的最好方法是深入理解组件如何工作，然后设计你的方式来提升它的性能。

您目前在 React 应用性能优化上使用的方法是什么？请在下面的评论中告诉我。

## 进一步阅读

[](https://medium.com/javascript-in-plain-english/18-useful-javascript-snippets-for-common-tasks-you-can-use-from-today-96fa03ce3df6) [## 18 个有用的 JavaScript 代码片段，你可以从现在开始使用

### 生命是短暂的，不要浪费时间一遍又一遍地写同样的代码。

medium.com](https://medium.com/javascript-in-plain-english/18-useful-javascript-snippets-for-common-tasks-you-can-use-from-today-96fa03ce3df6) [](/5-tools-practices-to-help-you-develop-faster-in-react-b884c1b20fc2) [## 帮助您在 React 中更快开发的 5 种工具和实践

### React 工具、技巧和最佳实践将帮助您更快地构建应用

javascript.plainenglish.io](/5-tools-practices-to-help-you-develop-faster-in-react-b884c1b20fc2) 

*更多内容请看*[***plain English . io***](https://plainenglish.io/)*。报名参加我们的* [***免费周报***](http://newsletter.plainenglish.io/) *。关注我们关于*[***Twitter***](https://twitter.com/inPlainEngHQ)[***LinkedIn***](https://www.linkedin.com/company/inplainenglish/)*[***YouTube***](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)*[***不和***](https://discord.gg/GtDtUAvyhW) *。***