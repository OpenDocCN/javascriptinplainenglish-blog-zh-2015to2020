# 反应提示—单选按钮、呈现 HTML 和初始化状态

> 原文：<https://javascript.plainenglish.io/react-tips-radio-buttons-render-html-and-initialize-states-197e7e8a8c30?source=collection_archive---------7----------------------->

![](img/9c5e21999d978fad922eaecfb41f8d72.png)

Photo by [Bernd Dittrich](https://unsplash.com/@hdbernd?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 用 React 插入 HTML

我们可以在 React 代码中插入 HTML。

为此，我们使用了`dangerouslySetInnerHTNL`道具。

例如，我们可以写:

```
render() {
  return (
    <div dangerouslySetInnerHTML={{ __html: someHtml }}></div>
    );
}
```

我们传入一个带有`__html`属性的对象来呈现原始 HTML。

`someHtml`是一个包含 HTML 内容的字符串。

# 从 React 组件中的 Props 初始化状态

我们可以通过在构造函数中设置`this.state`来初始化 React 组件中 props 的状态。

例如，我们可以写:

```
class App extends React.Component { constructor(props) {
    super(props); this.state = {
      x: props.initialX
    };
  }
  // ...
}
```

我们从`props`参数中获取道具，并将其设置为`this.state`的属性。

# 按 Enter 键后调用更改事件侦听器

要在按下 enter 键后调用一个`onChange`事件处理程序，我们可以监听 keydown 事件。

例如，我们可以写:

```
class App extends React.Component {
  handleKeyDown = (e) => {
    if (e.key === 'Enter') {
      console.log('entre pressed');
    }
  } render() {
    return <input type="text" onKeyDown={this.handleKeyDown} />
  }
}
```

我们通过将事件对象的`key`属性与一个字符串进行比较来检查它。

在函数组件中，我们可以编写:

```
const App = () => {
  const handleKeyDown = (event) => {
    if (event.key === 'Enter') {
      console.log('enter pressed')
    }
  } return <input type="text" onKeyDown={handleKeyDown} />
}
```

`handleKeyDown`功能相同。

# 指定一个端口以运行基于 create-react-app 的项目

我们可以通过设置`PORT`环境变量来改变 create-react-app 项目监听的端口。

例如，我们可以添加:

```
"start": "PORT=3006 react-scripts start"
```

进入`package.json`的`scripts`部分。

在 Linux 或 Mac OS 中。

在 Windows 中，我们可以放置:

```
"start": "set PORT=3006 && react-scripts start"
```

进入`package.json`的`scripts`部分。

# 在 React 中使用单选按钮

我们可以通过在单选按钮上设置值`onChange`事件监听器来使用带有 React 的单选按钮。

例如，我们可以写:

```
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {};
  } handleChange = e => {
    const { name, value } = e.target; this.setState({
      [name]: value
    });
  }; render() {
    return (
      <div>
        orange
        <input
          id="orange"
          value="orange"
          name="fruit"
          type="radio"
          onChange={this.handleChange}
        />
        grape
        <input
          id="grape"
          value="grape"
          name="fruit"
          type="radio"
          onChange={this.handleChange}
        />
        apple
        <input
          id="apple"
          value="apple"
          name="fruit"
          type="radio"
          onChange={this.handleChange}
        />
      </div>
    );
  }
}
```

我们创建了多个具有相同`name`属性的单选按钮。

这样，我们可以从所有按钮中选择一个。

在`handleChange`方法中，我们调用`setState`，将`e.target.name`作为键，将`e.target.value`作为值。

这用单选按钮的`name`设置了状态，即`fruit`，其值将是`value`属性的值。

对于函数组件，我们编写:

```
const useInput = (initialValue) => {
  const [value, setValue] = useState(initialValue); function handleChange(e){
    setValue(e.target.value);
  } return [value, handleChange];
}function App() {
  const [fruit, setFruit] = useInput(""); return (
    <form>
      <div>
        <input type="radio" id='apple' value='apple'
        checked={fruit === 'apple'} onChange={setFruit}/>
        <label>apple</label>
      </div>
      <div>
        <input type="radio" id='orange' value='orange'
        checked={fruit === 'orange'} onChange={setFruit}/>
        <label>orange</label>
       </div>
       <div>
        <input type="radio" id='grape' value='grape'
        checked={fruit === 'grape'} onChange={setFruit}/>
        <label>grape</label>
       </div>
     </form>
  )
}
```

我们以同样的方式创建了单选按钮，但是我们创建了自己的`useInput`钩子来使变更处理逻辑可重用。

在钩子中，我们使用了`useState`方法来设置状态。

然后我们返回`value`，它总是有最新的值，和`handleChange`，它有我们的事件处理函数。

然后我们在我们的`App`组件中使用了`useInput`钩子的返回值。

我们传递了`setFruit`处理函数，它作为数组的第二个条目返回给`onChange`属性。

然后，当我们单击按钮时，该值将被设置。

我们也有`checked`属性来用正确的`checked` 值呈现按钮。

![](img/66be1ad7bc7801887fd0be664301aa0f.png)

Photo by [ALE SAT](https://unsplash.com/@aletransportes?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用传递给`onChange` prop 的处理函数来设置单选按钮的值。

我们必须将它们的`name`属性设置为相同。

为了呈现原始 HTML，我们将一个对象传递到`dangerouslySetInnerHTML` prop 中。

同样，我们可以用道具初始化状态。

运行我们的 create-react-app 项目的端口可以改变。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**