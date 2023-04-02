# 反馈提示—点击、观看宽度、自定义链接

> 原文：<https://javascript.plainenglish.io/react-tips-clicks-watching-widths-custom-links-7a1459b0c681?source=collection_archive---------15----------------------->

![](img/aeb2240fa94bcfc3bf694b901af66659.png)

Photo by [Lucija Ros](https://unsplash.com/@lucija_ros?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 在 React 中手动触发点击事件

为了手动触发 React 组件中的 click 事件，我们可以为 DOM 元素分配一个 ref。

然后我们可以在 ref 上调用`click`。

例如，我们可以写:

```
class App extends React.Component {
  constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this);
  } render() {
    return (
      <div onClick={this.handleClick}>
        <input ref={input => this.inputElement = input} />
      </div>
    );
  } handleClick(e){
    this.inputElement.click();
  }
}
```

我们已经给`inputElement` ref 分配了回调。

然后我们通过调用`this.inputElement` DOM 对象上的`click`来点击`handleClick`方法中的输入元素。

这也适用于函数组件:

```
const UploadsWindow = () => {
  const inputRef = useRef(); const uploadClick = e => {
    e.preventDefault();
    inputRef.current.click();
  }; return (
    <>
      <input
        type="file"
        name="fileUpload"
        ref={inputRef}
        multiple
        onClick={uploadClick}
      />
    </>
  )
}
```

我们用`useRef`钩子创建了一个 ref，并将其分配给输入。

然后，我们有 click handler `uploadClick`函数，它调用输入 DOM 元素上的`click`方法。

`current`属性拥有被赋予了 ref 的 DOM 对象。

# 在 React 中响应自动调整大小的 DOM 元素的宽度

为了观察 React 上 DOM 元素的宽度，我们可以使用 react-measure 包。

要安装它，我们运行:

```
yarn add react-measure
```

或者:

```
npm install react-measure --save
```

然后我们通过书写来使用它:

```
import * as React from 'react'
import Measure from 'react-measure'const Width = () => (
  <Measure bounds>
    {({ measureRef, contentRect: { bounds: { width }} }) => (
      <div ref={measureRef}>{width}</div>
    )}
  </Measure>
)
```

我们有`Measure`组件，它有一个回调函数，回调函数有许多属性作为它的参数。

`measureRef`是一个引用，我们可以用它来获取元素。

`contentRect`具有关于已经分配给`measureRef`的元素的信息。

`width`有宽度。

因此，我们观察元素的宽度随`width`属性的变化。

还有 react-sizeme 组件，它也可以用来获取组件的大小。

例如，我们可以写:

```
import SizeMe from 'react-sizeme';class Circle extends Component {
  render() {
    const { width, height } = this.props.size; return (
     <svg width="100" height="100">
       <circle cx="150" cy="100" r="80" fill="green" />
     </svg>
    );
  }
}export default SizeMe()(Circle);
```

它带有`SizeMe`高阶组件，我们可以将我们的组件传递给它，以获得它的`width`和`height`。

它们可以通过 props 获得，因为我们用它调用了`SizeMe`高阶组件。

# 在渲染函数之外反应上下文

有了函数组件，我们可以使用`useContext`钩子来访问组件中任何地方的上下文。

例如，我们可以写:

```
const App = () => {
  const contextValue = useContext(AppContext);
  // ...
}
```

在类组件中，我们可以设置静态的`contextTyoe`属性，这样我们就可以在类组件中的任何地方使用上下文。

我们用`this.context`属性访问上下文。

例如，我们可以写:

```
class App extends React.Component {
  componentDidMount() {
    const value = this.context;
  } componentDidUpdate() {
    const value = this.context;
    /* ... */
  } componentWillUnmount() {
    const value = this.context;
    /* ... */
  } render() {
    const value = this.context;
    /* render content */
  }
}App.contextType = AppContext;
```

一旦我们将`contextType`属性设置为一个上下文，就可以在任何地方使用`this.context`。

# 将 React 路由器链接包装在 HTML 按钮中

我们可以用任何想要显示的东西包装`Link`组件。

例如，我们可以写:

```
<Link to="/profile">
   <button type="button">
     click me
   </button>
 </Link>
```

我们还可以嵌入组件:

```
<Link to="/profile">
   <Button>
     click me
   </Button>
 </Link>
```

![](img/07dfeb16a82bccd054e7af42c13f7575.png)

Photo by [Frida Bredesen](https://unsplash.com/@fridooh?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以通过将一个元素分配给 ref 并对其调用方法来触发 click 事件。

此外，我们可以使用`render`函数之外的上下文。

有一些库可以让我们观察组件维度的变化。

React 路由器`Link`组件内部可以有元素或者组件。

# 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**