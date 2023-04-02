# 反应提示—函数属性、查询字符串和默认属性

> 原文：<https://javascript.plainenglish.io/react-tips-function-props-query-strings-and-default-props-9b2536818f30?source=collection_archive---------4----------------------->

![](img/e1e5a1a726d90b04db652016fbde9a10.png)

Photo by [Ramiz Dedaković](https://unsplash.com/@ramche?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 听 React 中的状态变化

我们可以通过几种方式监听状态变化来做出反应。

在类组件中，我们可以使用`componentDidUpdate`钩子来监听状态和属性的变化。

我们可以写:

```
componentDidUpdate(object prevProps, object prevState)
```

还有一个不推荐使用的`componentWillUpdate`方法。

对于功能组件，我们可以使用`useEffect`钩子来观察变化。

例如，我们可以写:

```
const [name, setName] = useState('foo');
useEffect(getSearchResults, [name])
```

我们传入一个数组，其中包含我们希望在`useEffect`钩子的第二个参数中观察的变量。

第一个参数是当`name`改变时调用的回调。

# 在与 React 路由器的链接中传递属性

我们可以向`Link`传递一个查询字符串。

例如，我们可以写:

```
<Link to={{ pathname: `/foo/bar`, query: { backUrl } }} />
```

我们用一个对象设置了`query`属性。

对象的键和值被转换成一个查询字符串，附加在`pathname`之后。

我们也可以使用带占位符的 URL 参数:

```
<Route name="/foo/:value" handler={CreateIdeaView} />
```

那么我们可以通过使用:

```
this.props.match.params.value
```

要获得查询字符串，我们可以编写:

```
this.props.location.search
```

来获取值。

然后我们可以使用`URLSearchParams`构造函数来解析它和其中的值:

```
new URLSearchParams(this.props.location.search).get("foo")
```

如果我们有一个查询字符串`?foo=bar`，那么它将返回`'bar'`。

# 用 React 调用父方法

如果我们将父组件的函数作为道具传递给子组件，我们可以调用父组件的方法。

例如，我们可以写:

```
import React, {Component} from 'react';class Child extends Component {
  render () {
    return (
      <div>
        <button
          onClick={() => this.props.someFunc('foo')}
        >
          click me
        </button>
      </div>
    )
  }
}class Parent extends Component {
  someFunc(value) {
    console.log(value)
  } render() {
    return (
      <div>
        <Child
          someFunc={this.someFunc.bind(this)}
        />
      </div>
    )
  }}
```

我们用`Parent`组件中的`someFunc`道具将函数从父节点传递给子节点。

然后在`Child`组件中，我们用`this.props.someFunc`调用函数。

使用函数组件，我们可以编写:

```
import React from "react";function Child(props){
  return(
    <> 
      <button onClick={props.parentFunc}>
        click me
      </button>
    </>
  );
}function Parent(){
  const parentFunc = () => {
    console.log("parent func called");
  }

  return (
    <>    
      <Child 
        parentFunc={parentFunc}
      />
    </>
  );
}
```

我们有`Parent`组件，它有作为道具传递给`Child`组件的`parentFunc`。

然后，当我们单击`Child`组件中的按钮时，我们通过调用`props.parentFunc`来调用它。

# 使用 React、ES6 和 Webpack 导入和导出组件

要导出 React 项目中的组件，我们应该将其导出为默认导出。

例如，我们可以写:

`SomeNavBar.js`

```
import React from 'react';
import Navbar from 'react-bootstrap/lib/Navbar';export default class SomeNavBar extends React.Component {
  render(){
    return (
      <Navbar className="navbar-dark" fluid>
        {//...}
      </Navbar>
    );
  }
}
```

然后我们用任何我们想要的名字导入它。

例如，我们可以写:

```
import SomeNavBar from './SomeNavBar';
```

导入导航栏组件。

我们不会在默认导出中使用花括号。

# 在 React 组件上设置组件默认属性

要在 React 组件上设置默认属性，我们可以使用`prop-types`包。

我们必须通过运行以下命令来安装它:

```
npm i prop-types
```

然后我们可以写:

```
import PropTypes from 'prop-types';class Address extends React.Component {
  render() {
    const { street, city } = this.props;
    <p>{street}, {city}</p>
  }
}Address.defaultProps = {
  street: 'street',
  city: 'city',
};Address.propTypes = {
  street: PropTypes.string.isRequired,
  city: PropTypes.string.isRequired
}
```

我们设置`defaultProps`属性来设置默认的属性值。

该对象将属性名作为键，将默认值作为值。

`propTypes`有每个道具的道具类型。

我们将它们都设置为 string，并用`isRequired`使它们成为必需的。

![](img/3ce0814ec6aba268053fe771931d04d4.png)

Photo by [Herbert Goetsch](https://unsplash.com/@hg_photo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以设置默认道具，并用`prop-types`包验证它们的格式。

我们用类组件生命周期方法或功能组件的`useEffect`钩子来监听状态变化。

此外，我们可以使用 React Router 获取和设置查询字符串和 URL 参数。

我们可以将功能作为道具从父母传递给孩子。

# 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**