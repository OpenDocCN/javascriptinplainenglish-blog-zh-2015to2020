# React 提示—超级、调用子方法、反跳和内联样式

> 原文：<https://javascript.plainenglish.io/react-tips-super-call-child-methods-debounce-and-inlint-styles-55a98466152e?source=collection_archive---------4----------------------->

![](img/d13f9ba69a18b8a4325b50ca2b62df9f.png)

Photo by [Sheri Hooley](https://unsplash.com/@sherihoo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 使用 ES6 类时 React 中“super()”和“super(props)”的区别？

两者之间唯一的区别是当我们需要在构造函数中访问属性时。

如果我们需要在构造函数中获取它们，那么我们必须从参数中获取道具，并将其传递给`super`。

例如，我们写道:

```
class MyComponent extends React.Component {
  constructor(props) {
    super(props)
    console.log(this.props);
  }

  render(){
    //...
  }
}
```

如果没有，我们可以跳过它:

```
class MyComponent extends React.Component {
  constructor(props) {
    super()
  }

  render(){
    //...
  }
}
```

# 内联风格最佳实践

我们可以通过传入带有`styles`或`className`属性的对象来放置内联样式。

例如，我们可以写:

```
<li 
 className={classnames({ 'list-item': true, 'is-complete': item.complete })} />
```

我们使用了`classnames`包来动态设置`li`元素的类名。

此外，我们可以写:

```
<li style={Object.assign({}, fooStyles, barStyles)}>
```

我们只是通过传入一个具有所有样式属性的对象来设置样式。

键有属性，值就是值。

# 任何时候调用 setState 都会调用 render 吗？

每次调用`render`时都会调用`setState`。

这就是为什么我们不能在`render`方法中使用`setState`的原因。

否则，我们将得到一个无限的渲染循环。

如果我们不想在每次调用`setState`时都调用它，我们应该使用`shouldComponentUpdate`钩子。

我们可以比较状态和属性，并返回一个布尔表达式，其中包含通知 React 组件何时应该呈现的条件。

`showComponentUpdate`瓦斯如下签名:

```
shouldComponentUpdate(nextProps, nextState)
```

# 在 React 中执行去抖

我们可以通过使用令人敬畏的去抖承诺包来谴责 React，

例如，我们可以写:

```
const searchAPI = text => fetch(`/search?text=${encodeURIComponent(text)}`);const searchAPIDebounced = AwesomeDebouncePromise(searchAPI, 500);class SearchInputAndResults extends React.Component {
  state = {
    text: '',
    results: null,
  }; search = async text => {
    this.setState({ text, results: null });
    const result = await searchAPIDebounced(text);
    this.setState({ result });
  };
  //...
}
```

我们使用`AwesomeDebouncePromise`功能将`searchAPI`功能延迟 500 毫秒。

然后在`sesrch`方法中，我们用包返回的承诺，用`setState`来设置数据。

我们也可以将它与函数组件一起使用。

例如，我们可以写:

```
const searchAPI = text => fetch(`/search?text=${encodeURIComponent(text)}`);const searchAPIDebounced = AwesomeDebouncePromise(searchAPI, 500);

const App = () => {
  const [results, setResults] = useState([]);
  const [text, setText] = useState([''); search = async text => {
    const result = await searchAPIDebounced(text);
    setResults(result);
  }; return (
    <div>
      <input value={setText} onChange={e => setText(e.target.value)} />
      <div>
        {results && (
          <div>
            <ul>
              {results.map(r => (
                <li key={r.name}>{r.name}</li>
              ))}
            </ul>
          </div>
        )}
      </div>
    </div>
  );
};
```

我们使用相同的函数和`useState`钩子来设置完成后的状态。

这些都在`search`功能中完成。

# 从父方法调用子方法

我们可以通过将引用转发给子方法来调用父方法的子方法。

然后我们就可以从 ref 中得到子方法了。

例如，我们可以写:

```
const Child = forwardRef((props, ref) => {
  useImperativeHandle(ref, () => ({
    hello() {
      console.log("hello Child");
    }
  })); return <h1>Hi</h1>;
});const Parent = () => {
  const childRef = useRef(); return (
    <div>
      <Child ref={childRef} />
      <button onClick={() => childRef.current.hello()}>Click</button>
    </div>
  );
};
```

我们用一个返回组件的回调函数在孩子上调用`forwardRef`。

钩子获取 ref 并返回一个包含我们想要调用的方法的对象。

然后在父节点中，我们将引用传递给`ref`属性。

然后我们调用`current.hello`来调用子中的方法。

如果我们使用类组件，那么就更简单了。

例如，我们可以写:

```
const { Component } = React;class Parent extends Component {
  constructor(props) {
    super(props);
    this.child = React.createRef();
  } onClick = () => {
    this.child.current.hello();
  }; render() {
    return (
      <div>
        <Child ref={this.child} />
        <button onClick={this.onClick}>Click</button>
      </div>
    );
  }
}class Child extends Component {
  hello() {
    conole.log('hello child');
  } render() {
    return <h1>Hello</h1>;
  }
}
```

我们用从`Parent`调用的`hello`方法创建了一个`Child`。

然后我们只要给它分配一个 ref，用`current.hello`调用它。

![](img/b872eebba5610a81ba8f080182110cc5.png)

Photo by [Quinn Buffing](https://unsplash.com/@qbuffing?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以从父组件调用子组件功能。

如果我们需要访问构造函数中的 props，那么我们需要将它们传递给`super`。

我们可以谴责第三方库的事件处理程序。

# 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**