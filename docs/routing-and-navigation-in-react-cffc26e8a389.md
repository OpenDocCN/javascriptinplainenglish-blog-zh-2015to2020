# 如何在 React JS 中处理路由和导航

> 原文：<https://javascript.plainenglish.io/routing-and-navigation-in-react-cffc26e8a389?source=collection_archive---------0----------------------->

![](img/8933e4fa3854e49902a096a7f2866351.png)

在 React 应用程序的开始阶段，我认为应该遵循两个步骤来提高生产率:

1.  **用 ESLint、appellizer、EditorConfig、Jest、Reactotron 等工具配置 app 基础**。
2.  **配置路线和导航**以控制用户将访问的路线。

在这篇文章中，我将向你展示我是如何处理第二个项目，让我们的应用程序变得井井有条！

我假设您已经配置了 react 应用程序。如果没有，就按照这里的步骤:[https://github.com/facebook/create-react-app](https://github.com/facebook/create-react-app)

# 装置

第一步是安装 **react-router-dom** lib，它将负责为我们处理它:

```
yarn add react-router-dom
```

安装完成后，我们将创建文件夹`./src/routes`来添加我们的路由配置。在这个文件夹中，我们将有两个文件:

*   **index.js** →负责返回用户告知路线对应的页面。它将像路由器一样为我们的应用程序工作。
*   **Route.js** →该组件将负责根据用户的权限重定向用户。例如，当用户试图访问需要认证的路由时，我们将使用它将用户重定向到登录页面。

# 配置路线

先说`index.js`。在这个文件中，我们将从 **react-router-dom** 导入两个组件:

*   **开关**
*   **路线**

稍后我们将用我们自己的组件替换这条路线，但现在我们不必担心它。

我们将进口我们所有的页面(来自`./src/pages/COMPONENT_NAME/index.js`)。它们将在每条路线中返回，在这里我们将链接到每条路线的期望路径。

还要注意，我们正在传递一个名为`isPrivate`的属性，这个属性现在没有影响，但是稍后会帮助我们根据用户在应用程序中的权限重定向用户。

```
import React from 'react';
import { Switch, Route } from 'react-router-dom';import SignIn from '../pages/SignIn';
import SignUp from '../pages/SignUp';
import Dashboard from '../pages/Dashboard';export default function Routes() {
  return (
    <Switch>
      <Route path="/" exact component={SignIn} />
      <Route path="/register" component={SignUp} /> <Route path="/dashboard" component={Dashboard} isPrivate /> {/* redirect user to SignIn page if route does not exist and user is not authenticated */}
      <Route component={SignIn} />
    </Switch>
  );
}
```

Switch 按照路径在组件中的排列顺序检查路径，并总是返回包含路径值的第一个路由作为子字符串。因为我们有一个带有“/”的路径和另一个带有“/register”的路径，所以第二个路径永远不会被调用，因为第一个路径总是被选择。为了解决这个问题，我们需要添加`exact`属性，所以如果路由正好是“/”，它将返回登录组件。

还要注意，我们需要在这个文件中导入 React，因为我们使用的是 JSX。

# 配置历史记录

我们希望能够根据一些事件在我们的应用程序中自动重定向我们的用户，例如，如果他按下注销按钮，就让他返回到登录页面。为了处理这个问题，我们将使用一个名为 **history** 的库。

第一步是安装它:

```
yarn add history
```

现在我们将创建文件`./src/services/history.js`:

```
import { createBrowserHistory } from 'history';const history = createBrowserHistory();export default history;
```

# 配置路由器

现在我们需要配置我们的路由器，我们在`./src/App.js`里面。我们将导入三个文件:

*   **路由器** →该组件将执行重定向。
*   **历史** →将负责允许我们在应用程序中进行重定向，而无需用户在浏览器中键入路径。
*   **Routes** →我们的 Routes 文件将根据我们通知的路径返回其中一页。

现在，我们只需要将路由器放在 Routes 组件周围，我们的应用程序导航就可以工作了！

```
import React from 'react';
import { Router } from 'react-router-dom';import history from './services/history';
import Routes from './routes';function App() {
  return (
    <Router history={history}>
      <Routes />
    </Router>
  );
}export default App;
```

在我们的应用程序中有一些我们不想在用户登录之前访问的路径是很常见的，这样我们就可以检索他的信息。到目前为止，我们所做的让用户访问他想要的任何路线，因为他输入了正确的路径。

现在是创建我们自己的路由组件的时候了，这是我们在本文开头提到的。

在这个文件中，我们将有一个名为 RouteWrapper 的功能组件(这个名字不是 Route，因为我们将使用来自 **react-router-dom 的`Route`和`Redirect`。**

```
import React from 'react';
import { Route, Redirect } from 'react-router-dom';export default function RouteWrapper() {}
```

下一步是向我们的新组件导入一些属性:

*   **组件** →这里我们将接收我们想要呈现的页面，由我们的文件索引发送。
*   **isPrivate** →该属性将用于指示该路由是否可以在没有认证的情况下被访问。
*   **…rest** →这里我们将使用 spread 操作符将组件收到的所有其他参数传递给 rest 变量。

```
import React from ‘react’; 
import { Route, Redirect } from ‘react-router-dom’; export default function RouteWrapper({ 
  component: Component, 
  isPrivate, 
  …rest 
}) {}
```

既然我们已经导入了所有必要的道具，我们需要添加重定向逻辑。我们将有一个名为`signed`的变量，它将指示用户是否登录到系统(这个变量现在将有一个常量值，但是以后 **redux** 可以用来存储这个状态)。

业务逻辑将是:

1.  如果路由是私有的，并且用户没有登录，那么我们将把他送回登录页面。
2.  如果路由不是私有的，但是用户已经登录，那么我们把他发送到主页面，在我们的例子中就是仪表板页面。
3.  如果他不满足上述任何条件，那么我们从 **react-router-dom** 返回一个`ROute`，传递组件和所有其他参数。

```
import React from 'react'; 
import { Route, Redirect } from 'react-router-dom'; export default function RouteWrapper({   
  component: Component,   
  isPrivate,   
  ...rest 
}) {   
  const signed = true;    

  /**    
  * Redirect user to SignIn page if he tries to access a private      route
  * without authentication.    
  */   
  if (isPrivate && !signed) {     
    return <Redirect to="/" />;   
  }      /**    
  * Redirect user to Main page if he tries to access a non private route    
  * (SignIn or SignUp) after being authenticated.    
  */   
  if (!isPrivate && signed) {     
    return <Redirect to="/dashboard" />;   
  }    

  /**    
  * If not included on both previous cases, redirect user to the desired route.    
  */   
  return <Route {...rest} component={Component} />; 
}
```

为了完成我们的组件，我们需要添加 PropTypes 验证。第一步是安装库:

```
yarn add prop-types
```

之后，我们只需要导入并进行必要的验证。我们的组件将是这样的:

```
import React from 'react';
import PropTypes from 'prop-types';
import { Route, Redirect } from 'react-router-dom';export default function RouteWrapper({
  component: Component,
  isPrivate,
  ...rest
}) {
  const signed = true; /**
   * Redirect user to SignIn page if he tries to access a private route
   * without authentication.
   */
  if (isPrivate && !signed) {
    return <Redirect to="/" />;
  } /**
   * Redirect user to Main page if he tries to access a non private route
   * (SignIn or SignUp) after being authenticated.
   */
  if (!isPrivate && signed) {
    return <Redirect to="/dashboard" />;
  } /**
   * If not included on both previous cases, redirect user to the desired route.
   */
  return <Route {...rest} component={Component} />;
}RouteWrapper.propTypes = {
  isPrivate: PropTypes.bool,
  component: PropTypes.oneOfType([PropTypes.element, PropTypes.func])
    .isRequired,
};RouteWrapper.defaultProps = {
  isPrivate: false,
};
```

最后一步是更改我们在`./src/routes/index.js`中使用的路由组件，以便它使用我们的组件，而不是来自 **react-router-dom** 的组件:

```
import React from 'react';
// import { Switch, Route } from 'react-router-dom';
import { Switch } from 'react-router-dom';
import Route from './Route'; // rest of the code ommitted.
```

就是这样！如果你遵循所有这些步骤，你应该有非常模块化的代码，可以很容易地扩展，如果你继续使用相同的模式。

请注意，认证仍然是手动的，我们将在以后的文章中介绍这部分的自动化。

你可以在 codesandbox 上查看完整的实现！

我有另一个帖子，我继续这个应用程序添加可重复使用的布局。如果你想把它添加到你的项目中，勾选这个[链接](https://medium.com/@giovanniantonaccio/how-to-build-reusable-layouts-in-react-js-daf8adcbca79)。

希望这篇教程能对你的项目有所帮助！感谢阅读！