# React 提示和技巧—片段和错误处理

> 原文：<https://javascript.plainenglish.io/react-tips-and-tricks-fragments-and-error-handling-dbf4e7754993?source=collection_archive---------8----------------------->

![](img/a1666eaeb07465d3f35280e485521330.png)

Photo by [Victoria Strukovskaya](https://unsplash.com/@struvictoryart?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是构建现代交互式前端 web 应用最常用的前端库。它还可以用来构建移动应用程序。在本文中，我们将了解一些让 React 应用程序构建更容易的技巧和诀窍。

# 反应碎片

React 组件中不能有多个根节点。然而，我们可以有一个不呈现任何组件的根节点，使用 React 片段作为根节点。

我们可以引用带有`React.Fragement`组件或空标签的 React 片段。

例如，我们可以编写以下内容:

```
import React from "react";export default function App() {
  return (
    <React.Fragment>
      <p>foo</p>
      <p>bar</p>
    </React.Fragment>
  );
}
```

或者:

```
import React from "react";export default function App() {
  return (
    <>
      <p>foo</p>
      <p>bar</p>
    </>
  );
}
```

`<React.Fragment>`与`<>`相同。

# 使用错误边界来优雅地处理错误

错误边界是出现错误时显示的组件。它们有类似`componentDidCatch`的特殊钩子，让我们检索错误细节，并相应地对错误采取行动。

我们将错误边界组件包装在可能抛出错误的组件周围，以便它们工作。

错误边界组件总是基于类的组件。对此没有等效的功能组件。

例如，我们可以定义一个误差边界组件，并按如下方式使用它:

```
import React from "react";function Foo() {
  throw new Error("error");
  return <div>foo</div>;
}class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }
  componentDidCatch(error, info) {
    this.setState({ hasError: true });
  }
  render() {
    if (this.state.hasError) {
      return <h1>Error occurred.</h1>;
    }
    return this.props.children;
  }
}export default function App() {
  return (
    <ErrorBoundary>
      <Foo />
    </ErrorBoundary>
  );
}
```

在上面的代码中，我们定义了`ErrorBoundary`组件，该组件有一个`componentDidCatch`钩子，它接受带有错误的`error`参数，以及带有错误信息的`info`对象。

然后，我们调用`setState`到`hasError`到`true`，以便呈现错误消息。当没有错误时，我们返回`this.props.children`,这样我们就可以显示放在`ErrorBoundary`组件中的组件。

因此，当我们有`Foo`时，我们抛出一个错误，然后我们显示“错误发生”消息，因为`Foo`在渲染任何东西之前抛出一个错误。

# 高阶组件

高阶组件是渲染其他组件的组件。它很有用，因为它可以用来产生被高阶组件修改的组件。

例如，我们可以创建一个`colorizeElement`高阶组件，用值`blue`作为缺省值将颜色属性应用到一个组件。如果设置了`color`属性，那么它将覆盖我们传入的颜色属性的值。

我们可以按如下方式创建和使用它:

```
import React from "react";const colorizeElement = Element => props => <Element color="blue" {...props} />;const Foo = ({ color }) => {
  return <p style={{ color }}>foo</p>;
};const ColoredFoo = colorizeElement(Foo);export default function App() {
  return (
    <>
      <ColoredFoo color="red" />
      <ColoredFoo />
    </>
  );
}
```

在上面的代码中，我们有从`colorizeElement`高阶组件创建的`ColoredFoo`组件。在组件中，我们传入了`Element`，这是一个 React 组件，它以`props`作为参数返回一个新函数，并返回将`color`属性设置为`'blue'`的`Element`，还传入了传入的其他属性。

然后在`App`中，我们有`ColoredFoo`组件，一个有颜色道具组，另一个没有。那么第一个是红色，第二个是蓝色。

![](img/5666c2560bf32f015b1713c5813677ff.png)

Photo by [Barn Images](https://unsplash.com/@barnimages?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# React 开发工具

React dev tools 是 Chrome 和 Firefox 的扩展，由 React 核心团队维护。它让我们检查组件内部的属性和状态的值。

我们最终会遇到难以解决的错误和问题，所以这是一个方便的调试工具。

# 结论

错误边界和高阶组件分别非常适合显示错误和修改组件。

片段非常适合在一个根节点中呈现多个项目。它本身不呈现任何 HTML 元素。

React dev tools 是调试的一个很好的扩展。

## 来自简明英语团队的说明

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意吧:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！我们还推出了一个 YouTube，希望你能通过 [**订阅我们的英语频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) 来支持我们**

**一如既往,“简明英语”希望帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****