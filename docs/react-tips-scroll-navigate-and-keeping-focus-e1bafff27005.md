# 反应提示—滚动、导航和保持焦点

> 原文：<https://javascript.plainenglish.io/react-tips-scroll-navigate-and-keeping-focus-e1bafff27005?source=collection_archive---------7----------------------->

![](img/851198cfab67044bdd701c04dd2557d1.png)

Photo by [Grant Durr](https://unsplash.com/@blizzard88?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将探讨一些编写更好的 React 应用程序的技巧。

# 借助挂钩，让路由器在每次过渡时都能滚动到顶部

我们可以创建一个包装器组件，以便在每次导航时将路由组件滚动到顶部。

例如，我们可以写:

```
import React, { useEffect, Fragment } from 'react';
import { withRouter } from 'react-router-dom';function ScrollToTop({ history, children }) {
  useEffect(() => {
    const unlisten = history.listen(() => {
      window.scrollTo(0, 0);
    });
    return () => {
      unlisten();
    }
  }, []); return <Fragment>{children}</Fragment>;
}export default withRouter(ScrollToTop);
```

我们称`history.listen`为倾听导航。

在对它的回调中，我们调用`window.scrollTo(0, 0)`滚动到顶部。

它返回一个函数来清除监听程序，所以我们在`useEffect`回调中返回的函数中调用该函数。

我们将一个空数组传递给`useEffect`，以便在安装`ScrollToTop`组件时仅连接一次监听程序。

然后我们可以通过写作来使用它；

```
<Router>
  <ScrollToTop>
    <Switch>
       <Route path="/" exact component={Home} />
    </Switch>
  </ScrollToTop>
</Router>
```

我们将`ScrollToTop`环绕在所有路线周围，以便在加载任何路线组件时滚动到顶部。

# 对路由器和 withRouter 做出反应

`withRouter`是一个我们可以调用的组件，让我们可以将`location`、`history`和`match`对象作为道具包含在我们的组件中。

例如，我们可以写:

```
import React from "react";
import PropTypes from "prop-types";
import { withRouter } from "react-router";class App extends React.Component {
  static propTypes = {
    match: PropTypes.object.isRequired,
    location: PropTypes.object.isRequired,
    history: PropTypes.object.isRequired
  }; render() {
    const { match, location, history } = this.props;
    return <div>{location.pathname}</div>;
  }
}const ShowTheLocationWithRouter = withRouter(App);
```

`match`是包含路由器路径信息的对象。

它具有`params`属性来获取路径的段。

如果路径与 URL 匹配，则`isExact`为`true`。

`path`是具有用于匹配的模式的字符串。

`url`是具有 URL 的匹配部分的字符串。

`location`是具有带路径名的`pathname`的对象。

`search`为 URL 的查询字符串部分。

`hash`为`#`符号后的 URL 部分。

`history`是本土历史的对象。

它的`length`与历史堆栈中的条目数相同。

`location`属性将 URL 的部分作为对象返回。

这与`location`对象本身相同。

`push`是一种让我们导航到新路径的方法。

`replace`让我们通过改写历史来开启新的征程。

`go`按步数导航至历史条目。

`goBack`移至上一个历史。

让我们转到下一个历史条目中的路径。

# 对来自对象阵列的组件进行反应

我们可以通过使用`map`方法将一个对象数组呈现为一个组件列表。

例如，我们可以写:

```
const Items = ({ items }) => (
  <>
    {items.map(item => (
      <div key={items.id}>{items.name}</div>
    ))}
  </>
);
```

我们所做的就是用回调来调用`map`，以返回从`items`条目派生的条目。

`key`应该有一个唯一的 ID，以便 React 可以区分每个条目，即使他们已经移动。

# 修复重新渲染时输入失去焦点的问题

如果我们创建自己的输入组件，并在`render`方法中创建它，这可能会发生。

这将导致每次呈现输入时都创建一个自定义输入组件的新实例。

所以要把它搬出`render`。

我们可以通过写下以下内容来保持输入的重点:

```
import React from 'react';
import styled from 'styled-components';const Container = styled.div``;
const Input = styled.input``;class App extends React.Component {
  constructor(){
    this.state = {
      name: ''
    };
    this.updateName = this.updateName.bind(this);
  } updateName(e){
    e.preventDefault();
    this.setState({ name: e.target.value });
  } render() {
    return (
      <Container>
        <Input
          type="text"
          onChange={this.updateName}
          value={this.state.name}
        />
      </Container>
    )
  }
}
```

我们将`Input`和`Container`移动到`App`组件的外部，以在外部创建组件，而不是在内部动态创建。

这意味着将始终使用同一个实例。

当`App`重新呈现时，输入将保持焦点。

![](img/0c9791fc93849f27fa8b5beb837c3c74.png)

Photo by [Marcus Wallis](https://unsplash.com/@marcus_wallis?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们不应该在没有组件的情况下创建任何组件，这样就可以一直使用同一个实例。

我们可以创建自己的组件，以便在导航到不同的路线时将组件滚动到顶部。

`withRouter`向我们的组件注入一些对象。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**