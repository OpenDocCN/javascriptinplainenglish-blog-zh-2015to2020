# 使用 React 路由器添加路由转换效果

> 原文：<https://javascript.plainenglish.io/adding-route-transition-effects-with-react-router-8b5dd8214dde?source=collection_archive---------7----------------------->

![](img/fbd745d0657873abf691f89bd204503c.png)

Photo by [Diego Jimenez](https://unsplash.com/@diegojimenez?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建前端视图的库。它有一个庞大的图书馆生态系统与之合作。此外，我们可以用它来增强现有的应用程序。

要构建单页面应用程序，我们必须有某种方法将 URL 映射到 React 组件来显示。

在本文中，我们将看看如何添加转换效果来反应路由器路由转换。

# 添加路线过渡效果

要使用 React 添加过渡效果，我们可以使用`react-transition-group`库来轻松完成。

我们可以通过运行以下命令来安装它:

```
npm i react-transition-group
```

此外，为了使开发更加方便，我们还可以通过运行以下命令来添加 TypeScript 类型定义:

```
npm install @types/react-transition-group
```

当路线改变时，我们可以添加一个简单的渐变过渡效果，如下所示:

`styles.css`:

```
.fade-enter {
  opacity: 0;
}
.fade-enter-active {
  opacity: 1;
  transition: opacity 800ms;
}
.fade-exit {
  opacity: 1;
}
.fade-exit-active {
  opacity: 0;
  transition: opacity 800ms;
}
```

`index.js`:

```
import React from "react";
import ReactDOM from "react-dom";
import { TransitionGroup, CSSTransition } from "react-transition-group";
import {
  useLocation,
  useParams,
  BrowserRouter as Router,
  Switch,
  Route,
  Link
} from "react-router-dom";
import "./styles.css";function RGB() {
  let { r, g, b } = useParams();
  return (
    <div
      style={{
        background: `rgb(${r}, ${g}, ${b})`
      }}
    >
      rgb({r}, {g}, {b})
    </div>
  );
}function Foo() {
  return <h3>Foo</h3>;
}function AnimationApp() {
  const location = useLocation();
  return (
    <div>
      <ul>
        <li>
          <Link to="/foo">Foo</Link>
        </li>
        <li>
          <Link to="/rgb/33/150/243">Blue</Link>
        </li>
        <li>
          <Link to="/rgb/240/98/146">Pink</Link>
        </li>
      </ul> <div>
        <TransitionGroup>
          <CSSTransition key={location.key} classNames="fade" timeout={300}>
            <Switch location={location}>
              <Router>
                <Route path="/rgb/:r/:g/:b" children={<RGB />} />
                <Route path="/foo" children={<Foo />} />
              </Router>
            </Switch>
          </CSSTransition>
        </TransitionGroup>
      </div>
    </div>
  );
}function App() {
  return (
    <Router>
      <Switch>
        <Route path="*">
          <AnimationApp />
        </Route>
      </Switch>
    </Router>
  );
}const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

在上面的代码中，我们有过渡效果的类。

`fade-`之后的部分类名是标准的。我们可以将`fade`改为其他名称，只要它与我们为`CSSTransition`组件设置的`classNames`属性值相同。

在`styles.css`中，我们用`.fade-enter`指定动画开始时的样式。

然后我们指定用`.fade-enter-active`显示路线内容时要显示的样式。

一旦路线的组件退出，将应用`.fade-exit`类中的样式。然后，在 route 的组件离开 DOM 之后，将应用`.fade-exit-active`类中的样式。

对于每个类型动作，不以`-active`结尾的类应用于以`-active`结尾的类。这 4 个类一起创建了一个渐变效果，不透明度从 0 开始，然后过渡显示为完全不透明。然后过渡效果又消失了。

在`Route`组件中，我们根据 URL 显示`RGB`组件或`Foo`组件。

在`AnimationApp`的`CSSTransition`组件中，我们有`timeout`属性来指示转换进入或离开 DOM 所需的毫秒数。

`react-transitions-group`使用`key`道具来确定哪个子组件已经进入或退出。

在`App`中，我们有`Router`、`Switch`和`Route`组件。我们只是用它来映射到`AnimationApp`的所有路线，它有实际想要映射到的路线。

我们需要这样做，因为 React 路由器无法从根路由中获取`location`对象。所以:

```
const location = useLocation();
```

如果在`App`中会给我们一个错误。

在`RGB`组件中，我们用`useParams`获取路线参数，然后用它用路线参数中的 RGB 值设置背景颜色。

![](img/eb8d96977aa1d190806f34b3ce7c1a93.png)

Photo by [Justin Lawrence](https://unsplash.com/@thisisjlaw?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

当我们在路线之间转换时，我们可以使用`react-transition-group`库来添加转换效果。

像任何其他组件过渡一样，我们使用`TransitionGroup`和`CSSTransition`组件来创建 CSS 的过渡效果。

一旦我们指定了`classNames`属性的值，那么我们就可以使用 CSS 代码中的类前缀来指定转换。

我们可以为`enter`、`enter-active`、`exit`和`exit-active`类指定 CSS 样式，如果需要的话还可以指定其他样式。

不以`-active`结尾的类应用于以`-active`结尾的类。

## **简明英语团队的笔记**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**我们还推出了一个 YouTube，希望你能通过 [**订阅我们的简明英语频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) 来支持我们**

**一如既往,“简明英语”希望帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****