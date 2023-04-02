# React 提示—修复常见错误、多个类和上下文 API

> 原文：<https://javascript.plainenglish.io/react-tips-fix-common-errors-multiple-classes-and-context-api-f9fb834960b4?source=collection_archive---------15----------------------->

![](img/f09fac3d1f81ea0d2f4855f8122232c0.png)

Photo by [Juan Encalada](https://unsplash.com/@juanencalada?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 向 React 组件添加多个类

我们可以使用`classnames`包向 React 组件添加多个类。

例如，在`render`方法或函数组件中，我们可以写:

```
const liClasses = classNames({
  'main-class': true,
  'active': props.active
});return (<li className={liClasses}>{props.name}</li>);
```

我们也可以使用模板文字来做同样的事情。

例如，我们可以写:

```
<input className={`form-control rounded ${this.state.valid ? '' : 'error'}`} />
```

# 修复“不变冲突:对象作为反应子对象无效”错误

我们可以通过确保在包装组件的开始和结束标记之间有字符串或组件来修复错误。

例如，如果我们要呈现一个字符串，我们可以写:

```
return (
  <Item href={routeString}>
    {breadcrumbElement}
  </Item>
)
```

其中`breadcrumbElement`是反应元素或成分。

我们也可以用字符串替换元素或组件:

```
return (
  <Item href={routeString}>
    hello world
  </Item>
)
```

如果我们有一个数组，我们可以写:

```
const photosList = photos.map((photo, i) => {
  return (
    <div>
       <img src={photo.url} alt={photos.description} />
    </div>
  );
});return (
  {photosList}
);
```

这些都在类组件或函数的`render`方法内部。

我们有一个表达式，通过调用返回 img 元素的回调函数`map`,将`photos`数组呈现为图像。

然后我们在我们的`return`语句中返回它。

# 修复了“一个组件正在将一个文本类型的非受控输入更改为受控”错误

我们可以通过用状态设置输入的`value`属性来修复错误。

为了设置状态，我们向`onChange`属性传递一个事件处理程序，以便在输入值改变时设置状态。

例如，我们可以写:

```
<input 
  className="input" 
  type="text" 
  value={this.state.name || ""} 
  name="name" 
  placeholder="name" 
  onChange={this.onChange}
/>
```

那么在我们的`onChange`方法中，我们可以写:

```
onChange(event){
  const { name, value } = event.target;
  this.setState(prevState => {
    prevState.fields[name] =  value;
    return {
      fields: prevState.fields
    };
  });
};
```

我们从`event.target`中获得`name`和`value`属性。

然后我们用回调调用`setState`，将新值与现有的状态对象合并。

# 两个反应组件通信

React 组件进行通信的场景有几个。

最常见的就是亲子沟通。

例如，我们可以写:

```
const Child = ({ onClick }) => (
  <div onClick={() => onClick(42)}>
    Click me
  </div>
);class Parent extends React.Component {
  onClick(value){
    console.log(value);
  }; render() {
    return (
      <Child onClick={this.onClick}/>
    )
  }
}
```

我们将`Parent`中的`onClick`方法传递给`Child`组件。

然后我们从参数`props`中得到道具的`onClick`。

然后我们将它传递给`onClick`回调函数，在那里我们用一个值调用它。

然后`console.log`将运行。

如果我们想要在具有其他关系的组件之间进行通信，我们可以使用上下文 API。

例如，我们可以写:

```
const AppContext = React.createContext(null)class App extends React.Component {
  render() {
    return (
      <AppContext.Provider value={{ language: "en" }}>
        <div>
          <Foo>
            <Bar>
              <Baz />
            </Bar>
          </Foo>
        </div>
      </AppContext.Provider>
    )
  }
};const Baz = () => (
  <AppContext.Consumer>
    {({language}) => <div>{language}</div>}
  </AppContext.Consumer>
);
```

我们调用`React.createContext`来创建上下文。

然后我们可以使用`AppContext.Provider`将数据传递给`AppContext.Provider`中的任何组件。

`value`道具拥有我们可以在其他地方访问的数据。

由于我们的`Baz`组件在我们的上下文提供者内部，我们可以使用`AppContext.Consumer`来获取数据并呈现它。

我们在里面有一个回调函数来从我们传递给`value`的对象中获取`language`。

![](img/645b95e5d43a5fa7c9b5fcdd1bcb1dbb.png)

Photo by [Cristina Anne Costello](https://unsplash.com/@lightupphotos?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用模板字符串或`classnames`包向一个组件添加多个类。

我们可以直接在父组件和子组件之间进行通信。

或者我们可以使用上下文 API 在任何组件之间进行通信。

各种错误可以通过快速修复来解决。

# 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**