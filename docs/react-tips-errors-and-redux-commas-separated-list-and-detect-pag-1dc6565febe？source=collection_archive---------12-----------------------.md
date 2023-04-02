# 反应提示-错误和冗余，逗号分隔列表，并检测页面导航

> 原文：<https://javascript.plainenglish.io/react-tips-errors-and-redux-commas-separated-list-and-detect-pag-1dc6565febe?source=collection_archive---------12----------------------->

![](img/78afe521634592cfe631df4aeed50dfd.png)

Photo by [Bonnie Kittle](https://unsplash.com/@bonniekdesign?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 用 React 路由器检测用户离开页面

React 路由器有一个`Prompt`组件，让我们用一条消息阻止导航。

例如，我们可以写:

```
import { Prompt } from 'react-router'const App  = () => (
  <React.Fragment>
    <Prompt
      when={shouldBlockNavigation}
      message='Are you sure you want to leave?'
    />
    {/* more components */}
  </React.Fragment>
)
```

我们有`Prompt`,它接受一个带有布尔表达式的`when`提示，指示我们何时想要阻止导航。

`message`是用户试图离开时显示的消息。

它将阻止任何事情，但不刷新或关闭页面。

为了阻止刷新或关闭，我们必须根据导航是否被阻止，将`onbeforeunload`属性设置为不同的值。

例如，我们可以写:

```
componentDidUpdate = () => {
  if (shouldBlockNavigation) {
    window.onbeforeunload = () => true
  } else {
    window.onbeforeunload = undefined
  }
}
```

如果在刷新或关闭浏览器标签时导航应该被阻止，那么我们将它`window.onbeforeunload`设置为一个返回`true`的函数。

否则，我们将其设置为`undefined`。

# 避免 React 中的额外包装

我们可以使用一个片段来避免在组件之外包装一个额外的 div。

例如，我们可以写:

```
render() {
  return (
    <React.Fragment>
      <ChildA />
      <ChildB />
      <ChildC />
    </React.Fragment>
  );
}
```

或者:

```
render() {
  return (
    <>
      <ChildA />
      <ChildB />
      <ChildC />
    </>
  );
}
```

# 处理 react-redux 中的提取错误

我们可以使用 redux-thunk 创建一个异步动作，调用`dispatch`在出错时调度同步动作。

例如，我们可以写:

```
export function getData() {
  return dispatch => {
    dispatch({type: FETCH }); httpClient.get('/data')
      .then(res => {
        dispatch({ type: FETCH_SUCCESS, data: res.body });
      })
      .catch(error => {
        dispatch({type: FAILED });             
        dispatch({type: ADD_ERROR, error });      
      });
  };
}
```

我们有一个 thunk，它获取数据并调用`dispatch`来分派各种动作。

在我们的`errors`减速器中，我们可以写:

```
function errors(state = [], action) {
  switch (action.type) {
    case ADD_ERROR:
      return [...state, action.error];
    case REMOVE_ERROR:
      return state.filter((error, i) => i !== action.index);
    default:
      return state;
  }
}
```

我们有一个`errors`减速器，它有一个`ADD_ERROR`箱用来增加误差，另一个箱用来消除给定的误差。

然后，我们可以通过编写以下代码将`errors`状态映射到 props:

```
export default connect(
  state => ({
    errors: state.errors,
  })
)(App);
```

其中`App`是反应元件。

# 告诉运行时在浏览器中运行的 React 版本

我们可以获取`React.version`属性来获取运行在浏览器中的 React 版本。

还有:

```
npm view react version
npm view react-native version
```

以获得 React 版本。

`require(‘React’).version`也管用。

# 用 React 路由器删除浏览器中的/#/

要用 React Roytee 从 URL 中删除散列符号，我们可以使用`BrowserRouter`作为路由根标签。

例如，我们可以写:

```
import BrowserRouter from 'react-router/BrowserRouter'ReactDOM.render (( 
  <BrowserRouter>
   {//...}
  <BrowserRouter> 
), document.body);
```

我们还需要重写 URL 以重定向到`index.html`，这样我们在加载或刷新页面时就不会出错:

```
# Setting up apache options
AddDefaultCharset utf-8
Options +FollowSymlinks -MultiViews -Indexes
RewriteEngine on# Defining the rewrite rules
RewriteCond %{SCRIPT_FILENAME} !-d
RewriteCond %{SCRIPT_FILENAME} !-fRewriteRule ^.*$ ./index.html
```

这个配置是针对 Apache 服务器的。

# 使用 map 和 reduce 渲染由逗号分隔的 React 组件

我们可以通过将条目映射到组件来呈现带有`map`和`join`的 React 组件。

然后我们可以使用`reduce`在组件之间放置逗号。

例如，我们可以写:

```
class CommaList extends React.Component {
  render() {
     <div>
        {this.props.data
          .map(t => <span>{t}</span>)
          .reduce((combined, curr) => [combined, ', ', curr])}
     </div>
  }
}
```

我们得到了`data`道具，这是一个数组。

然后我们调用`map`将它们映射到组件。

然后我们调用`reduce`在已经用逗号连接的组件之间添加逗号，这些组件存储在`combined` 中，新组件存储在`curr`中。

![](img/8526f5891e26bacab03a8c7044be9f71.png)

Photo by [Katherine McAdoo](https://unsplash.com/@ohaikatherine?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以在 React 路由器中检测用户离开页面的情况。

我们可以用`reduce`将逗号分隔的组件组合成一个数组。

此外，当 Redux 出现错误时，我们可以调度操作。

# 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**