# 反应最佳实践—分隔旧功能和道具

> 原文：<https://javascript.plainenglish.io/react-best-practices-spacing-and-props-b54e28119567?source=collection_archive---------9----------------------->

![](img/992e366fc8eaa89f33e43c71f37dd498.png)

Photo by [Henry Be](https://unsplash.com/@henry_be?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何种类的应用程序一样，React 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写 React 应用程序时的一些最佳实践。

此外，我们还会查看应该避免的旧 React 特性。

# 没有未被空格分隔的相邻行内元素

我们不应该有没有被空格分隔的嵌套的行内元素。

例如，我们应该有这样的东西:

```
<div><a></a><a></a></div>
```

这很难读，我们可能会因此而犯错误。

相反，我们写道:

```
<div><a></a> <a></a></div>
```

所以我们有一些空间。

# 不要对键使用数组索引

我们不应该使用数组索引作为`key`属性的值。

这是因为数组索引不能唯一地标识我们的元素。

如果对其进行排序或重新排列，索引会发生变化。

因此，我们需要在每个数组条目中有一个唯一可识别的值来传递给 key prop。

所以，我们不应该写:

```
things.map((thing, index) => (
  <Foo key={index} />
));
```

相反，我们应该写:

```
things.map((thing, index) => (
  <Foo key={thing.id} />
));
```

# 路过的孩子当道具

不应该在我们的组件中显式写出`children`属性。

这是因为`children`是在标签之间传递元素和组件的特殊道具。

例如，我们不应该有这样的代码:

```
<div children='Children' />
```

相反，我们应该写:

```
<Foo>
  <span>Child 1</span>
  <span>Child 2</span>
</Foo>
```

# 不要使用危险的 JSX 道具

我们应该避开`dangerouslySetInnerHTML`。

这是因为它不经过任何净化就呈现 HTML。

这肯定不好，因为任何人都可以运行任何类型的代码，包括恶意代码。

# 不要同时使用儿童道具和危险道具

`children`道具和`dangerouslySetInnerHTML`不能同时使用。

例如，我们不应该写:

```
<div dangerouslySetInnerHTML={{ __html: "HTML" }}>
  Children
</div>
```

相反，我们写道:

```
<Hello>
  Children
</Hello>
```

或者:

```
<div dangerouslySetInnerHTML={{ __html: "HTML" }} />
```

# 不要使用不推荐的方法

有些 React 方法已被弃用。

它们包括`render`、`unmountComponentAtNode`、`findDOMNode`、`renderYoString`、`renderToStaticMarkup`和`createClass`。

这些方法不应该再使用了，因为它们将来会被删除。

# 不要在 componentDidMount 中使用 setState

组件挂载后更新状态将触发第二次渲染，因此性能将受到影响。

例如，我们不应该写:

```
componentDidMount: function() {
  this.setState({
    name: this.props.name
  });
}
```

# componentDidUpdate 中没有 setState

像使用`componentDidMount`一样，使用`setState`和`componentDidUpdate`会导致第二次渲染，因此性能会受到影响。

而不是写:

```
componentDidUpdate() {
  this.setState({
    name: this.props.name.toUpperCase()
  });
}
```

我们写道:

```
componentDidUpdate() {
  onUpdate();
}
```

这样可以提高性能。

# 不要对这个状态进行直接突变

要改变一个组件的状态，我们应该调用`setState`。

这是因为如果调用`setState`，直接设置`this.state`将不起作用。

`setState`将覆盖`this.state`的值。

所以我们不应该写:

```
this.state.name = this.props.name;
```

相反，我们写道:

```
this.setState({
  name: this.props.name;
});
```

# 不要使用 findDOMNode

`findDOMNode`阻止了 React 的某些改进，所以我们不应该使用它来获取 DOM 元素。

相反，我们应该使用参考文献。

而不是写:

```
findDOMNode(this).scrollIntoView();
```

我们写道:

```
function Foo {
  const ref = useRef(); useEffect({} => {
    ref.current.scrollIntoView()
  }, []); return <div ref={ref} />
}
```

# 不要使用 isMounted

`isMounted`不应该使用，因为它是反模式。

我们不应该检查一个组件是否与它一起安装。

相反，我们应该用状态来检查组件是否被安装。

它在类或函数组件中不可用。

所以如果我们有:

```
if (this.isMounted()) {
  return;
}
```

我们应该移除它们。

# 为每个文件定义一个组件

如果我们为每个文件定义一个组件，就更容易跟踪组件。

因此，我们应该这样做。

# 扩展 React 时不要使用 shouldComponentUpdate。纯组件

拥有一个纯组件的全部在于我们想要使用内置的`shouldComponentUpdate`实现。

所以我们不应该在我们的类组件中有`shouldComponentUpdate`，即使我们可以使用它。

![](img/a80b8c2fb84de1232446c75de435dccf.png)

Photo by [Ramiz Dedaković](https://unsplash.com/@ramche?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们不应该使用被否决的方法。

此外，我们不应该对键使用数组索引，因为如果我们重新排列它们，条目的索引可能会改变。

## 简单英语的 JavaScript

你知道我们有四种出版物吗？通过[**plain English . io**](https://plainenglish.io/)找到他们——通过关注我们的出版物和 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达爱意吧！**