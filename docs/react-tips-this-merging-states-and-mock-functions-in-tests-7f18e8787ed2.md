# React 提示——这是测试中的合并状态和模拟功能

> 原文：<https://javascript.plainenglish.io/react-tips-this-merging-states-and-mock-functions-in-tests-7f18e8787ed2?source=collection_archive---------10----------------------->

![](img/15b127f6c5bc4128b5a07210209c1eb1.png)

Photo by [ALFI CANIAGO](https://unsplash.com/@alfi413?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 用 setState 合并状态而不是覆盖

如果我们传入一个回调到方法而不是一个对象，我们可以用`setState`合并状态而不是覆盖。

例如，不写:

```
this.setState({ x: 1 });
```

我们写道:

```
this.setState((prevState, props) => ({
  point: {
    ...prevState.point,
    y: 1
  },
));
```

我们传入一个回调，该回调将前一个状态作为第一个参数。

然后，我们将先前状态的值合并到返回的对象中。

我们在它后面加上`y`。

# 如何在每个测试中改变 Jest 模拟函数的返回值

我们可以通过用`jest.fn()`创建模拟函数来改变模拟函数的实现。

然后我们可以通过传入一个返回我们想要的值的回调函数来调用`mockImplementation`方法。

或者我们可以将返回值作为参数传递。

例如，我们可以写:

```
import { foo, bar } from './module';jest.mock('./module', () => ({
  foo: jest.fn(),
  bar: jest.fn()
}));
```

我们用`jest.fn()`模拟模块的方法。

然后我们在上面写下调用`mockImplementation`:

```
foo.mockImplementation(() => true)
```

或者:

```
foo.mockImplementation(true)
```

在我们的测试中。

在不同的测试中，我们可以写:

```
foo.mockImplementation(() => false)
```

或者:

```
foo.mockImplementation(false)
```

还有我们可以使用的`mockReturnValueOnce`方法。

例如，我们可以写:

```
import { foo, bar } from './module';jest.mock('./module', () => ({
  foo: jest.fn(),
  bar: jest.fn()
}));describe('test suite', () => {
  it('some test', () => {
    foo.mockReturnValueOnce(true);
    bar.mockReturnValueOnce(false);
  });
});
```

# React 出错时更换 img src

我们可以向 img 元素添加一个错误处理程序，以便在出错时加载另一个图像。

例如，我们可以写:

```
import React, { Component } from 'react';
import PropTypes from 'prop-types';class Image extends Component {
  constructor(props) {
    super(props); this.state = {
      src: props.src
    };
  } onError = () => {
    this.setState({
      src: this.props.fallbackSrc,
    });
  } render() {
    const { src } = this.state; return (
      <img
        src={src}
        onError={this.onError}
        {...props}
      />
    );
  }
}Image.propTypes = {
  src: PropTypes.string,
  fallbackSrc: PropTypes.string,
};
```

我们在错误处理程序中用`fallbackSrc`替换`src`状态。

这样，当带有给定的`src`字符串的图像无法加载时，我们将它设置为`fallbackSrc`值，作为 img 的`src`属性值。

# 用 React 创建“如果”组件

我们可以使用布尔表达式来创建一个组件，有条件地显示一些东西。

例如，我们可以写:

```
<div>
  {(cond
    ? <div>It's true</div>     
    : <div>It's false</div>
  )}
</div>
```

如果`cond`为`true`，则显示`<div>It’s true</div>`。

否则，显示`<div>It’s false</div>`。

我们也可以将`null`或`false`作为不呈现任何内容的空组件。

例如，我们可以写:

```
<div>
  {showChild ? <Child /> : false}
</div>
```

如果`showChild`是`true`，我们显示`Child`组件。

否则，我们什么也不显示。

我们也可以用`if`来表述:

```
renderPerson() {
  const personName = '';
  if (personName) {
    return <div>{personName}</div>;
  }
}
```

我们显示`personName`是否存在。

然后在`render`中，我们写道:

```
render() {
  return (
    <div>
      {this.renderPerson()}
    </div>
  );
}
```

我们渲染`personName`如果它存在的话。

此外，我们可以写:

```
{cond && (<div>It's true</div>)}
```

# 将物体作为道具传递给 JSX

要用 JSX 传递一个对象作为道具，我们可以使用 spread 操作符。

例如，我们可以写:

```
<Child  {...commonProps} />
```

其中`commonProps`是一个对象。

所有的键都作为道具传入。

所有的值都作为道具的值传入。

# 修复此问题。状态未定义

如果我们在类方法中使用了`this.state`，那么我们应该调用`bind`将类设置为方法中`this`的值。

例如，我们可以写:

```
<Form onClick={this.onClick.bind(this)}/>
```

然后，`this.onClick`方法中的`this`值将被设置为该类，因为`this`是该类。

将`this.onClick.bind(this)`放到道具中会在每次渲染时创建一个新函数，所以我们应该将它放在构造函数中以避免这种情况:

```
constructor(props) {
  super(props);
  this.state = {
    users: null
  }
  this.onClick = this.onClick.bind(this);
}
```

![](img/060778bf5a589b20603dc6fc367e335d.png)

Photo by [Kedar Redekar](https://unsplash.com/@kredekar?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用一些方法模仿 Jest 方法的返回值。

此外，我们可以合并状态，而不是覆盖它们。

我们可以有条件地渲染物品。

我们必须调用`bind`来将`this`设置为类。

# 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**