# 反应最佳实践—组件和状态

> 原文：<https://javascript.plainenglish.io/react-best-practices-components-and-states-551709d7df6f?source=collection_archive---------7----------------------->

![](img/62e888beed4954cd61722768f7b9affe.png)

Photo by [JOHN TOWNER](https://unsplash.com/@heytowner?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，React 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写 React 应用程序时的一些最佳实践。

# 使用带挂钩的 ES6 类或功能组件

我们应该为我们的 React 组件采用现代 JavaScript 语法。

我们可以使用类或者功能组件。

它们在不久的将来都会得到支持。

例如，我们可以写:

```
class Hello extends React.Component {
  render() {
    return <div>Hello {this.props.name}</div>;
  }
}
```

或者:

```
function Hello({ name }) {
  render() {
    return <div>Hello {name}</div>;
  }
}
```

他们都很好。

如果我们正在构建现代应用程序，就不应该再使用它了。

# 强制只读属性

如果有只读道具，那么就要强制执行。

例如，我们可以写:“

```
function Hello(props: { -name: string }) {
  return <div>Hello {props.name}</div>;
}
```

或者:

```
const Hello = (props: {|name: string|}) => (
  <div>Hello {props.name}</div>
);
```

它们都用 Flow 使我们的道具只读。

没有办法用普通的 JavaScript 将属性设为只读。

# 将无状态 React 组件编写为纯函数

使用无状态组件的全部意义在于没有自己的状态。

所以他们应该只拿道具渲染。

例如，我们可以写:

```
const Foo = function(props) {
  return <div>{props.foo}</div>;
};
```

我们所做的就是获取`props`并渲染它们。

他们不应该做别的。

# 添加属性验证

适当验证将省去我们许多麻烦。

如果我们只有:

```
function Hello({ name }) {
  return <div>Hello {name}</div>;
}
```

然后我们可以为`name`传递任何东西。

相反，我们应该为`prop-types`包添加如下验证:

```
import PropTypes from 'prop-types';function Hello({ name }) {
  return <div>Hello {name}</div>;
}Hello.propTypes = {
  name: PropTypes.string.isRequired,
};
```

现在我们确保`name`被传入并且是一个字符串。

# 当有 JSX 时，添加反应进口

当有 JSX 时，我们应该加上反动派。

没有它我们会得到一个错误。

例如，我们应该写:

```
import React from 'react';

const Hello = ({ name }) => <div>Hello {name}</div>;
```

# 为每个不是必需道具的道具添加默认道具

如果它不是一个必需的属性，那么我们应该添加一个默认值，这样即使没有传入任何东西，我们也可以为这个属性赋值。

例如，我们可以写:

```
const HelloWorld = ({ first, last }) => (
  <h1>Hello, {first} {last}!</h1>
);

HelloWorld.propTypes = {
  first: PropTypes.string.isrequired,
  last: PropTypes.string,
};HelloWorld.defaultProps = {
  last: 'smith'
};
```

然后我们将`first`属性设置为 required，因此我们需要为它传入一个值。

但是`last`不是必需的。

因此，我们应该为它设置一个默认值，这样无论我们有什么，我们都可以为`last`渲染一些东西。

# 添加一个 shouldComponentUpdate 以减少渲染

我们应该添加一个`shouldComponentUpdate`方法，这样我们就可以比较道具或状态来决定我们是否应该重新渲染组件。

例如，我们可以写:

```
class Component extends React.Component {
  shouldComponentUpdate () {
    return false;
  }
}
```

这将防止组件发生任何重新渲染。

# 对组件使用 ES6 类或函数

如果我们有 React 组件的 ES6 类函数，我们使用`render`方法来呈现我们想要的项目。

如果是函数组件，我们直接返回项目。

例如，我们可以写:

```
class Hello extends React.Component {
  render() {
    <div>Hello</div>;
  }
}
```

或者:

```
function Hello {
  <div>Hello</div>;  
}
```

# 向没有子级的组件或元素添加结束标记

JSX 遵循类似 XML 的语法，所以我们应该为没有子组件的组件设置结束标记。

例如，我们写道:

```
const Hello = <Hello name="james" />;
```

或者:

```
const Image = <img src="picture.png" />
```

![](img/518559b642528110c9ed3e4872d4397d.png)

Photo by [Fábio Alves](https://unsplash.com/@barncreative?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该用现代语法创建组件。

用新的语法编写更容易。

我们还可以调用`shouldComponentUpdate`来确定我们的组件是否需要重新渲染，这样我们就可以减少更新的频率。

## **说白了**

你知道我们有四种出版物吗？通过 [**plainenglish.io**](https://plainenglish.io/) 找到他们——通过关注我们的出版物和 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**