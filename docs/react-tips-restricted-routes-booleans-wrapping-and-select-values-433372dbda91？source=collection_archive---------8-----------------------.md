# 反应提示-受限路径、布尔值、换行和选择值

> 原文：<https://javascript.plainenglish.io/react-tips-restricted-routes-booleans-wrapping-and-select-values-433372dbda91?source=collection_archive---------8----------------------->

![](img/8a0b46b0dba0020e0a81e75297a98231.png)

Photo by [Ramiz Dedaković](https://unsplash.com/@ramche?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 限制对 React 路由器中路由的访问

我们可以通过检查凭证来限制对路由的访问。

然后，如果凭证有效，我们可以返回组件。

否则，我们重定向到我们想要重定向到的路径。

例如，我们可以写:

```
export default function AuthenticatedRoute({ component: Component, appProps, ...rest }) {
  return (
    <Route
      {...rest}
      render={props =>
        appProps.isAuthenticated
          ? <Component {...props} {...appProps} />
          : <Redirect
              to={`/login?redirect=${props.location.pathname}${props.location.search}`}
            />}
    />
  );
}export default function App() {
  const [authenticated, setAuthenticated] = useState(false); useEffect(() => {
    onLoad();
  }, []); async function onLoad() {
    try {
      await getSession();
      setAuthenticated(true);
    } catch (e) {
      console.log(e);
    }
  } return (
    <div>
      <Switch>
        <AuthenticatedRoute
          path="/profile"
          component={Profile}
          appProps={{ authenticated }}
        />
        <Route component={Login} path="/login" />
      </Switch>
    </div>
  );
}
```

我们创建了接受我们想要限制的组件的`AuthenticatedRoute`。

我们检查组件中的凭证。

如果它有效，我们返回组件来渲染它。

否则，我们返回`Redirect`组件来重定向到登录路由。

# 切换反应组件的布尔状态

我们可以通过调用与现在状态相反的值的`setState`来切换 React 组件的布尔状态。

例如，我们可以写:

```
class A extends React.Component {
  constructor() {
    super()
    this.handleCheckBox = this.handleCheckBox.bind(this)
    this.state = {
      checked: false
    }
  }

  handleCheckBox(e) {
    this.setState({
      checked: e.target.checked
    })
  }

  render(){
    return <input type="checkbox" onChange={this.handleCheckBox} checked={this.state.checked} />
  }
}
```

我们有一个复选框，我们通过`this.handleCheckBox`方法来更新`checked`状态。

然后我们通过调用那里的`setState`来更新状态。

然后我们将`checked`道具设置为`this.state.checked`状态。

使用函数组件，我们可以编写:

```
import React from "react";function App(props) {
  const [toggled, setToggled] = React.useState(false); 

  return  (
    <button
      onClick={(event) => {
       setToggled(!toggled);
     }}
    >
      {toggled.toString()}
    </button>;
}
```

当我们点击按钮时，我们将`toggled`状态设置为与`toggled`状态的现有值相反。

然后，我们通过将字符串设置为版本`toggled`来查看按钮的内容。

# 组件的默认导出

要将组件导出为默认导出，我们可以编写:

```
const Header = () => {
  return <p>hello</p>
};
export default Header;
```

或者:

```
export default () => (
  <p>hello</p>
)
```

我们分别定义我们的`const`变量，然后导出它。

或者我们直接出口组件。

# 有条件地包装 React 组件

为了有条件地包装 React 组件，我们可以创建一个更高阶的组件来进行条件包装。

例如，我们可以写:

```
const WithLink = ({ link, className, children }) => (link ?
  <a href={link} className={className}>
    {children}
  </a>
  : children
);const Link = ({ link, className, count }) => {
  return (
    <WithLink link={link} className={className}>
      <i className={styles.Icon}>
        {count}
      </i>
    </WithLink>
  );
}
```

我们创建了一个`WithLink`高阶组件来获取链接，然后用它来检查我们是否应该在组件周围放一个包装器。

然后在我们的`Link`组件中，我们得到`link`和`className`道具，并将它们传递给`WithLink`组件。

我们将`count`道具传递给图标类。

# 从 React 中带有多个选项的<select>元素获取值</select>

我们可以创建一个带有`select`元素的组件，它可以接受多重选择。

然后我们可以添加一个`onChange`处理程序来从 select 元素中获取选项。

例如，我们可以写:

```
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      multiValue: []
    }
  } handleChange(e) {
    this.setState({ multiValue: [...evt.target.selectedOptions].map(o => o.value) });
  } render() {
    return (
      <select multiple value={this.state.multiValue} onChange={this.handleChange}>
        <option value='apple'>Apple</option>
        <option value='orange'>Orange</option>
        <option value='grape'>Grape</option>
      </select>
    );
  }
}
```

我们用`evt.target.selectedOptions`属性得到所有选中的选项。

然后，我们通过返回`value`属性将每个条目映射到它们的值。

![](img/f5a47f85984463272de0405231de22c6.png)

Photo by [Raoul Droog](https://unsplash.com/@raouldroog?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用 `evt.target.selectedOptions`得到多个选择元素的所有选择值。

此外，我们可以使用 React 路由器创建受限路由。

当我们对 React 组件做一些事情时，我们可以切换布尔状态。

# 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**