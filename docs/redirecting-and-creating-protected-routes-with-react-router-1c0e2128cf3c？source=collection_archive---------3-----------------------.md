# 使用 React 路由器重定向和创建受保护的路由

> 原文：<https://javascript.plainenglish.io/redirecting-and-creating-protected-routes-with-react-router-1c0e2128cf3c?source=collection_archive---------3----------------------->

![](img/1ed33341e3f400b734c3f569b6fa24a0.png)

Photo by [Joshua Sortino](https://unsplash.com/@sortino?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建前端视图的库。它有一个庞大的图书馆生态系统与之合作。此外，我们可以用它来增强现有的应用程序。

要构建单页面应用程序，我们必须有某种方法将 URL 映射到 React 组件来显示。

在本文中，我们将研究如何重定向到不同的路由，并使用 React 路由器创建认证路由。

# 再直接的

我们可以使用`Redirect`组件重定向到不同的路线。

它需要以下道具:

*   `to` string prop —包含要重定向到的 URL 的字符串
*   `to`对象属性——带有`pathname`、`search`查询参数和`state`的对象，带有可通过`this.props.location.state`访问的附加数据
*   `push` —如果`true`，则向历史添加新条目的布尔属性
*   `from` —要重定向的字符串属性。这只能用于在`Switch`外渲染`Rediect`时匹配位置
*   `exact` —一个布尔属性，指示我们是否希望仅在 URL 完全匹配时重定向到该 URL。
*   `strict` —一个布尔属性，用于在重定向到 URL 之前检查尾部斜杠是否匹配
*   `sensitive` —指示重定向 URL 匹配是否必须区分大小写的布尔属性。

我们可以用它来制作一个需要登录的受保护页面，如下所示:

```
import React from "react";
import ReactDOM from "react-dom";
import { BrowserRouter as Router, Switch, Route, Link } from "react-router-dom";
import { useHistory, useLocation, Redirect } from "react-router";function PublicPage() {
  return <h3>Public</h3>;
}function ProtectedPage() {
  return <h3>Protected</h3>;
}function LoginPage() {
  let history = useHistory();
  let location = useLocation();
  let { from } = location.state || { from: { pathname: "/" } };
  let login = () => {
    localStorage.setItem("loggedin", true);
    history.replace(from);
  }; return (
    <div>
      <button onClick={login}>Log in</button>
    </div>
  );
}function AuthButton() {
  let history = useHistory();
  return localStorage.getItem("loggedin") ? (
    <p>
      Welcome!{" "}
      <button
        onClick={() => {
          localStorage.clear();
          history.push("/");
        }}
      >
        Sign out
      </button>
    </p>
  ) : (
    <p>You are not logged in.</p>
  );
}function PrivateRoute({ children, ...rest }) {
  return (
    <Route
      {...rest}
      render={({ location }) =>
        localStorage.getItem("loggedin") ? (
          children
        ) : (
          <Redirect
            to={{
              pathname: "/login",
              state: { from: location }
            }}
          />
        )
      }
    />
  );
}function App() {
  return (
    <Router>
      <AuthButton />
      <div>
        <ul>
          <li>
            <Link to="/">Public Page</Link>
          </li>
          <li>
            <Link to="/private">Private Page</Link>
          </li>
        </ul> <hr />
        <Switch>
          <Route exact path="/">
            <PublicPage />
          </Route>
          <Route path="/login">
            <LoginPage />
          </Route>
          <PrivateRoute path="/private">
            <ProtectedPage />
          </PrivateRoute>
        </Switch>
      </div>
    </Router>
  );
}const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

在上面的代码中，我们有一个`PrivateRoute`组件，如果定义了`localStorage.get('loggedin')`的话，我们将显示开始和结束标记之间的内容。

否则，我们通过呈现`Redirect`组件重定向到`Login`组件。

我们通过从道具中获取`path`并添加一个`render`道具，然后将该值设置为一个函数，如果定义了`localStorage.get('loggedin')`的话，该函数将呈现`children`道具。否则，我们呈现`Redirect`组件，将`to`设置为`/login`以返回登录页面。

`Login`组件有一个登录按钮，点击该按钮会将`loggedin`设置在本地存储器中。

我们还有`AuthButton`来显示用户的身份验证状态，还有一个注销按钮来清除本地存储，并在点击时重定向回主页。

然后在`App`中，我们添加了`Switch`中的所有路线，这样当我们点击`Link`时，我们就可以走我们期望的路线

我们列出了所有的路线，除了在`Switch`组件标签之间的私人路线。

![](img/a2d3f10b9c17bd1046a40e72cc5400aa.png)

Photo by [Toa Heftiba](https://unsplash.com/@heftiba?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以呈现`Redirect`组件来重定向到一条路线。

如果用户没有登录或者没有满足其他条件，它对于定义我们想要对用户隐藏的受保护的路由是很有用的。

为了隐藏受保护的路由，我们可以创建一个返回`Route`对象的组件，在那里我们得到传入的`path`属性，然后我们添加一个`render`属性，如果满足某个条件，我们就在那里显示`props.children`，如果不满足，就重定向到登录页面。

## **用简单英语写的 JavaScript 笔记**

我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到[**submissions@javascriptinplainenglish.com**](mailto:submissions@javascriptinplainenglish.com)给我们，我们会把你添加为作者。

我们还推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**