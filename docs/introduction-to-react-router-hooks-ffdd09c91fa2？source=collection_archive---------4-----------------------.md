# React 路由器挂钩简介

> 原文：<https://javascript.plainenglish.io/introduction-to-react-router-hooks-ffdd09c91fa2?source=collection_archive---------4----------------------->

![](img/8cf7a3b2ac7e55c0e8da033f831a0b31.png)

Photo by [Scott Walsh](https://unsplash.com/@outsighted?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React Router 是 React 最受欢迎的路由解决方案之一，用于构建单页 React 应用程序。

在本文中，我们将通过 React 路由器钩子 API 来清理我们的代码。

# `useParams`

`useParams`钩子用于从 URL 参数中获取路由器参数。

如果我们定义了采用 URL 参数的路由，我们可以使用这个钩子。例如，我们可以编写下面的代码来使用`useParams`钩子:

```
import React from "react";
import { BrowserRouter as Router, Switch, Route, Link } from "react-router-dom";
import { useParams } from "react-router";const Foo = () => {
  const { id } = useParams();
  return <p>{id}</p>;
};export default function App() {
  return (
    <Router>
      <div>
        <nav>
          <ul>
            <li>
              <Link to="/foo/1">Foo/1</Link>
            </li>
          </ul>
        </nav>
        <Switch>
          <Route path="/foo/:id">
            <Foo />
          </Route>
        </Switch>
      </div>
    </Router>
  );
}
```

在上面的代码中，我们用`path`属性设置为`/foo/:id`的`Route`组件定义了路线。

然后在它里面，我们有了`Foo`组件。在`Foo`组件中，我们有`useParams`钩子，我们从钩子中获取 URL 参数，其中参数值以 URL 参数的名称作为对象返回。

在我们的例子中，应该是`:id`。我们用析构语法通过键得到`id`的值。

然后在`Foo`组件中呈现`id`。然后我们有了带有`/foo/1`的`Link`组件作为`to`属性的值。

然后我们看到屏幕上显示 1，这是 URL 参数`id`的值。

# `useLocation`

我们可以使用`useLocation`属性来获取组件中的全局`location`对象。

例如，我们可以如下使用这个钩子:

```
import React from "react";
import { BrowserRouter as Router, Switch, Route, Link } from "react-router-dom";
import { useLocation } from "react-router";const Foo = () => {
  const loc = useLocation();
  return <p>{JSON.stringify(loc)}</p>;
};export default function App() {
  return (
    <Router>
      <div>
        <nav>
          <ul>
            <li>
              <Link to="/foo/1">Foo/1</Link>
            </li>
          </ul>
        </nav>
        <Switch>
          <Route path="/foo/:id">
            <Foo />
          </Route>
        </Switch>
      </div>
    </Router>
  );
}
```

在上面的代码中，我们使用了`Foo`中的`useLocation`钩子来获取`location`对象并将其分配给`loc`。

然后我们展示字符串化的版本来查看`location`对象内部的值。

这个钩子从 React 路由器的 5.1 版本开始可用。

![](img/3ca6895ad4cb0a4a4a268085598d8a5b.png)

Photo by [British Library](https://unsplash.com/@britishlibrary?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# `useHistory`

`useHistory`钩子返回`history`对象。它让我们像调用`goBack`和`push`方法一样调用`history`方法中的方法。

我们可以如下使用`useHistory`:

```
import React from "react";
import { BrowserRouter as Router, Switch, Route, Link } from "react-router-dom";
import { useHistory } from "react-router";const Foo = () => {
  const hist = useHistory();
  return (
    <div>
      <Link to="/foo">foo</Link>
      <Link to="/bar">bar</Link>
      <button onClick={() => hist.goBack()}>back</button>
      <p>foo</p>
    </div>
  );
};const Bar = () => {
  const hist = useHistory();
  return (
    <div>
      <Link to="/foo">foo</Link>
      <Link to="/bar">bar</Link>
      <button onClick={() => hist.goBack()}>back</button>
      <p>bar</p>
    </div>
  );
};export default function App() {
  return (
    <Router>
      <div>
        <Switch>
          <Route path="/foo">
            <Foo />
          </Route>
          <Route path="/bar">
            <Bar />
          </Route>
        </Switch>
      </div>
    </Router>
  );
}
```

在上面的代码中，我们有两个路由组件，`Foo`和`Bar`。在每个组件中，我们有以下回调:

```
() => hist.goBack()
```

它用于返回到以前的路线。每次我们点击每页中的按钮，它都会返回到上一页，因为我们有了返回`history`对象的`useHistory`钩子，然后将它赋给`hist`常量。

这让我们可以调用`history`对象中的方法。

# 结论

`useParams`钩子对于获取作为对象的给定路由的 URL 参数很有用。这样，我们可以使用 get 并轻松地使用它们来做任何我们想做的事情，比如通过 ID 获取数据。

`useLocation`钩子返回全局`location`对象，这样我们就可以使用类似`href`、`hostname`、`pathname`等属性。

同样地，`useHistory`钩子让我们获得组件中的`history`对象，这样我们就可以调用`go`、`goBack`等方法。在 a 组件中。

# 简明英语笔记

你知道我们推出了一个 YouTube 频道吗？我们制作的每个视频都旨在教给你一些新的东西。点击此处 查看我们，并确保订阅该频道😎