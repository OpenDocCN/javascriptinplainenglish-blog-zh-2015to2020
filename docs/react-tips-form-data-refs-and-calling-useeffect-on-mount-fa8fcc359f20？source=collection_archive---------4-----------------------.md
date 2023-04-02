# 反应提示—表单数据、引用和调用装载时的使用效果

> 原文：<https://javascript.plainenglish.io/react-tips-form-data-refs-and-calling-useeffect-on-mount-fa8fcc359f20?source=collection_archive---------4----------------------->

![](img/7a19f0ea28fbf249e20cf08aede3bba2.png)

Photo by [Kristin Brown](https://unsplash.com/@kristinbrownphotography?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 在 React 中获取表单数据

要获取表单数据，我们可以通过不受控制的输入直接获取表单元素。

或者我们可以创建受控输入并设置每个字段的状态。

然后我们可以得到场的状态。

例如，我们可以写:

```
class LoginForm extends React.Component {
  constructor(){
    this.state = {}
  } handleEmailChange(e) {
     this.setState({email: e.target.value});
  }, handlePasswordChange(e) {
    this.setState({password: e.target.value});
  }, handleLogin() {
    console.log(this.state);
  } render() {
    return (
      <form>
        <input type="text" name="email" placeholder="Email" value={this.state.email} onChange={this.handleEmailChange} />
        <input type="password" name="password" placeholder="Password" value={this.state.password} onChange={this.handlePasswordChange}/>
        <button type="button" onClick={this.handleLogin}>Login</button>
      </form>);
    }
  }
}
```

我们创建了一个带有表单的`LoginForm`组件，让我们输入电子邮件和密码。

我们将每个表单的`value`道具设置为 state。

我们将`onChange`属性设置为处理函数，用最新输入的值更新状态。

然后当我们点击登录按钮时，`handleLogin`运行并记录状态。

要获得未受控输入的值，我们可以写:

```
class LoginForm extends React.Component { constructor(props) {
    super(props);
    this.handleSubmit = this.handleSubmit.bind(this);
  } handleSubmit(e) {
    e.preventDefault();

    const formData = new FormData(e.target)
    const user = {}
    for (const entry of formData.entries()) {
      console.log(entry);
    }
  } render() {
    return (
        <div>
          <form onSubmit={this.handleSubmit}>
            <input type='text' name="phone"/>
            <input type='password' name="email"/>
            <input type="submit" value="Submit"/>
          </form>
        </div>
    );
  }
}
```

我们有一些没有`value`或`onChange`道具的输入来获取和设置状态。

这意味着它们是不受控制的输入。

当我们单击 Submit 按钮提交表单时，我们将整个表单放在`FormData`构造函数中来填充表单数据。

然后我们可以用`formData.entries()`遍历条目。

# 在 React 中访问 DOM 元素

React 中 document.getElementById()的等价物是设置引用。

我们可以使用`createRef`方法创建一个 ref 并将其分配给一个元素。

例如，我们可以写:

```
class Foo extends React.Component {
  constructor(props) {
    super(props);
    this.myRef = React.createRef();
  }
  render() {
    return <div ref={this.myRef} />;
  }
}
```

我们调用`React.createRef`来创建 ref。

然后我们将它分配给带有`ref`属性的 div。

然后，我们可以用以下方式获得 div 节点:

```
const node = this.myRef.current;
```

对于功能组件，我们可以使用`useRef`钩子。

如果我们需要访问子组件的方法，那么我们用`React.forwardRef`方法创建子组件。

例如，我们可以写:

```
const Foo = React.forwardRef((props, ref) => {
  return <div ref={ref}>foo</div> 
});const Bar = React.forwardRef((props, ref) => {
  const handleClick = () => {};
  useImperativeHandle(ref,() => ({
    handleClick
  }))
  return <div>bar</div> 
});const App = () => {
  const foo = useRef(null);
  const bar = useRef(null); return (
      <>
         <Foo ref={foo} />
         <Bar ref={bar} />
      </>
    )
}
```

我们在接受`props`和`ref`参数的回调中创建组件代码。

`props`有道具，`ref`有裁判。

方法放在我们在`userImperativeHandle`钩子的回调中返回的对象中。

我们还将`ref`分配给我们想要的元素。

然后我们在父组件`App`中创建带有`useRef`钩子的引用。

然后我们将引用分配给组件。

# 仅调用一次 React useEffect 加载函数

如果我们想调用一次`useEffect`钩子的回调函数，我们可以传入一个空数组作为它的第二个参数。

例如，我们写道:

```
const useMountEffect = (fun) => useEffect(fun, [])function Foo() {
  useMountEffect(() => console.log('hello')) 
  return <div>foo</div>;
}
```

我们创建了自己的`useMountEffect`钩子，它返回`useEffect`钩子，回调作为第一个参数，空数组作为第二个参数。

然后我们在`Foo`组件中使用它，并传入回调。

![](img/7af97b8dade16cacbd49e2282afb3a26.png)

Photo by [Davide Pietralunga](https://unsplash.com/@davide_pietralunga?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以为元件和元素指定参照。

我们用同样的方法处理类组件。

但是我们必须使用带有功能组件的`forwardRef`。

如果我们将一个空数组作为它的第二个参数传入，可以让它只调用一次。

根据输入是否受控，有多种方法可以获取表单数据值。

## **简单英语的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**