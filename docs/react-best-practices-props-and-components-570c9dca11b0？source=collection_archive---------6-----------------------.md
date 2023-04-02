# 反应最佳实践—道具和组件

> 原文：<https://javascript.plainenglish.io/react-best-practices-props-and-components-570c9dca11b0?source=collection_archive---------6----------------------->

![](img/12149aa2e87bfe4bd42e94988c608cfd.png)

Photo by [Suryaansh Maithani](https://unsplash.com/@ximinvincible?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何种类的应用程序一样，React 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写 React 应用程序时的一些最佳实践。

# 组件方法顺序

我们可能想要加强生命周期组件方法的顺序，这样我们总是可以很容易地找到代码。

例如，我们可以对静态方法属性、定制方法、`render`方法进行排序。

此外，我们可以按照以下顺序保存生命周期方法:

`displayName`，`propTypes`，`contextTypes`，`childContextTypes`，`mixins`，`statics`，`defaultProps`，`constructor`，`getDefaultProps`，`state`，`getInitialState`，`getChildContext`，`getDerivedStateFromProps`，`componentWillMount`，`UNSAFE_componentWillMount`，`componentDidMount`，`componentWillReceiveProps`，`UNSAFE_componentWillReceiveProps`，`shouldComponentUpdate`，`componentWillUpdate`，`UNSAFE_componentWillUpdate`，`getSnapshotBeforeUpdate`，`componentDidUpdate`，`componentDidCatch`，`componentWillUnmount`

一致的排序顺序使东西更容易找到。

# 按字母顺序排序 PropType 声明

我们可以对适当类型进行排序，以便更容易地找到它们，就像生命周期方法一样。

例如，我们可以写:

```
class Foo extends React.Component {
  static propTypes = {
    a: PropTypes.string,
    b: PropTypes.number,
    c: PropTypes.any
  }
  render() {
    return <div />;
  }
}
```

现在我们可以毫不费力地找到他们。

# 状态初始化样式

如果我们使用普通的 JavaScript，那么我们必须写:

```
class Foo extends React.Component {
  constructor(props) {
    super(props)
    this.state = { bar: 1 }
  }
  render() {
    return <div>Foo</div>
  }
}
```

但是如果我们使用 TypeScript，我们也可以写:

```
class Foo extends React.Component {
  state = { bar: 0 }
  render() {
    return <div>Foo</div>
  }
}
```

我们可以选择并坚持一个一致性。

# 静态方法的定位

我们可以用最新版本的 JavaScript 将静态方法放入类中。

例如，我们可以写:

```
class Component extends React.Component {
  static get displayName() { /*...*/ }
  static get defaultProps() { /*...*/ }
  static get propTypes() { /*...*/ }
}
```

或者我们可以把它们放在外面:

```
class Component extends React.Component { /*...*/ }
Component.displayName = "Hello";
Component.defaultProps = { /*...*/ };
Component.propTypes = { /*...*/ };
```

我们应该选择并坚持使用一个来保持一致性。

# 样式属性值应该是一个对象

道具应该有一个对象传入其中。

例如，我们应该写:

```
<div style={{ color: "red" }} />
```

而不是:

```
<div style="color: 'red'" />
```

对象是 React 理解的对象，而不是字符串。

# Void DOM 元素不应该有子元素

像`<img />`和`<br />`这样的 Void DOM 元素不应该有子元素或者开始和结束标签。

所以我们不应该有这样的代码:

```
<br>Children</br>
<br children='foo' />
<br dangerouslySetInnerHTML={{ __html: 'HTML' }} />
```

相反，我们应该写:

```
<br />
```

# JSX 的布尔属性符号

为了使布尔属性更清晰，我们应该加入`true`,而不是省略布尔属性的值。

例如，我们写道:

```
const Hello = <Hello personal={true} />;
```

这样，我们知道`personal`就是`true`。

# 在 JSX，花括号内没有空格

React 移除元素之间无关的换行符，因此元素之间的空格不会被渲染。

所以我们真的不需要它们之间的空间。

如果我们想要空格，我们必须在花括号内添加一个字符串。

例如，我们写道:

```
<div>
  hello
  {' '}
  <b>world</b>
</div>
```

# JSX 文的右括号符号

如果我们的组件或元素跨越多行，我们应该将右括号放在靠近左括号的水平位置。

比如说。我们写道:

```
<Hello firstName="john" lastName="smith" />;
```

或者:

```
<Hello
  firstName="jane"
  lastName="doe"
/>;
```

这使得我们的代码更整洁，更容易阅读。

# JSX 的结束标记位置

我们应该把结束标签放在和开始标签相同的水平位置。

例如，我们写道:

```
<Hello>
  hello
</Hello>
```

它更整洁，更容易阅读。

# 没有多余的花括号

当组件或元素之间有字符串时，我们不应该有多余的花括号。

而不是写:

```
<App>{'Hello world'}</App>;
```

我们写道:

```
<App>Hello world</App>;
```

它们做同样的事情，所以花括号和引号是无关的。

# 花括号中的换行符

如果我们在花括号中有长表达式，我们应该添加换行符，这样我们就不必水平滚动来阅读整行。

例如，我们写道:

```
<div>
  {
    foo &&
    foo.bar
  }
</div>
```

保持线条简洁。

![](img/8bdebae6662f8fffe7d9abd3735cef95.png)

Photo by [cristina pop](https://unsplash.com/@pcristina18?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们的代码中应该有无关的字符。我们不需要花括号来包装文本。

此外，我们不应该在标签中有孩子。

我们可能还想对属性和方法进行排序，以便更容易阅读。

## **简单英语的 JavaScript**

你知道我们有四份出版物和一个 YouTube 频道吗？在 [**plainenglish.io**](https://plainenglish.io/) 和 [**找到它们订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**