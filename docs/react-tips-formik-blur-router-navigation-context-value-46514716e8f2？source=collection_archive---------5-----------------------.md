# 反应提示—形成模糊、路由器导航、上下文值更改

> 原文：<https://javascript.plainenglish.io/react-tips-formik-blur-router-navigation-context-value-46514716e8f2?source=collection_archive---------5----------------------->

![](img/d94e1b2161642400a1ebcf30e4a89c73.png)

Photo by [Dave Oliver](https://unsplash.com/@myfunkypixel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# JSX 的三元运算符包括 HTML

我们可以通过编写以下代码来添加三元运算符以包含 HTML

```
render () {
  return (
    <div>
      {(this.state.message === 'success')
          ? <div>successful</div> 
          : <div>not successful</div> 
      }
    </div>
  );
}
```

# 使用 useContext 的 React 挂钩时更改上下文值

我们可以将状态改变函数传递给上下文，让我们在任何地方调用它们。

这样，我们可以更改接收上下文的任何组件中的值。

例如，我们可以写:

```
const { createContext, useContext, useState } = React;const ThemeContext = createContext({});function Content() {
  const { style, color, toggleStyle, changeColor } = useContext(
    ThemeContext
  ); return (
    <div>
      <p>
        {color} {style}
      </p>
      <button onClick={toggleStyle}>Change style</button>
      <button onClick={() => changeColor('green')}>Change color</button>
    </div>
  );
}function App() {
  const [style, setStyle] = useState("light");
  const [color, setColor] = useState(true); function toggleStyle() {
    setStyle(style => (style === "light" ? "dark" : "light"));
  } function changeColor(color) {
    setColor(color);
  } return (
    <ThemeContext.Provider
      value={{ style, visible, toggleStyle, changeColor }}
    >
      <Content />
    </ThemeContext.Provider>
  );
}
```

我们创建了`ThemeContext`，在`App`组件中使用它来获取提供者，并将其包装在`Content`组件周围。

然后，我们通过将状态变量传递给`value` prop 中的对象，将我们的状态变化函数传递给上下文。

然后在`Content`组件中，我们可以调用`toggleStyle`和`toggleStyle`和`changeColor`函数来改变`App`中的状态。

我们用`useContext`钩子在`Content`中得到这些函数。

# 用酶模拟变化事件

我们可以通过为 handle change 方法创建一个 spy 来用 Enzyme 模拟一个 change 事件。

例如，我们可以写:

```
it("responds to name change", done => {
  const handleChangeSpy = sinon.spy(Form.prototype, "handleChange");
  const event = { target: { name: "name", value: "spam" }};
  const wrap = mount(
    <Form />
  );
  wrap.ref('name').simulate('change', event);
  expect(handleChangeSpy.calledOnce).to.equal(true);
})
```

我们为我们的变更事件监听器创建一个间谍。

然后我们设置我们的事件对象。

然后我们安装我们想要测试的`Form`组件。

然后我们用`simulate`方法触发其上的变更事件。

然后我们查了一下那个间谍是不是叫过一次。

# 如何将自定义 onChange 和 onBlur 与 React Formik 一起使用

我们可以通过编写以下代码来创建自己的模糊处理程序:

```
<Field
    component={MyInput}
    name="email"
    type="email"
    onBlur={e => {
        handleBlur(e)
        let someValue = e.currentTarget.value
        //...
    }}
/>
```

我们调用内置的`handleBlur`函数。

然后我们做其他的事情。

# 如何使用电子感应路由器

我们可以在带有`HashRouter`的电子应用中使用 React Router，

例如，我们可以写:

```
<HashRouter
  basename={basename}
  getUserConfirmation={() => {}}
  hashType='slash'
>
  <App />
</HashRouter>
```

`basename`是一个字符串，包含所有位置的基本 URL。

`getUserConfirmation`是让我们用来确认导航的功能。

`hashType`是一个字符串，包含用于`window.location.hash`的编码。

可能的值是`'slash'`，也就是`#/`。

`'noslash'`是`#`。

而`'hashbang'`美国`#!/`。

# 无法使用 React 路由器读取未定义的属性“push”

我们可以使用`withRouter`高阶组件使`history`对象可用。

例如，我们可以写:

```
class App extends React.Component {
  constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this);
  } handleClick(value) {
    this.props.history.push('/dashboard');
  } render() {
    return (
      <div>
        <Route
          exact
          path="/"
          render={() => (
            <div>
              <h1>Welcome</h1>
            </div>
          )}
       />
       <Route
         path="/dashboard"
         render={() => (
           <div>
             <h1>Dashboard</h1>
           </div>
         )}
      />
      <button onClick={this.handleClick} >go to dashboard</button>
    );
  }
}export default withRouter(App);
```

我们有几条路线和一个`handleClick`方法让我们去仪表板路线。

我们有`this.props.history.push`方法，因为我们用`App`调用了`withRouter`高阶分量。

![](img/3d0e1076e0255c89e264fe4df85532e2.png)

Photo by [Jack Catalano](https://unsplash.com/@jrcatalano?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用`withRouter`编程导航到路线。

我们可以将任何东西传递给上下文，这样我们就可以在任何地方使用它们。

Formik 的模糊处理程序可以替换成我们自己的函数。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果是这样，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**