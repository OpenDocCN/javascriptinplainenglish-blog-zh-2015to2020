# React:非常简单的组件通信

> 原文：<https://javascript.plainenglish.io/react-dead-simple-component-communication-4582c0cb18c1?source=collection_archive---------7----------------------->

## 尽快:尽可能简单

![](img/9b267150595dbf168eeee1a41d6c8868.png)

在 React 中，有一种非常简单的方法来建立孩子与父母之间的交流:

```
class Parent extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
     title: "Foo"
    }
    this.onTitleChange = function(){
      this.setState({"title":"bar"})
    }
  }  
  render() {
    return (
      <div>
        <h1>The Title: {this.state.title}</h1>
        <Child onTitleChange={this.onTitleChange.bind(this)}></Child>
      </div>
    )
  }
}
class Child extends React.Component {
  constructor(props){
    super(props);
  }
  render() {
    return (
      <button onClick={this.props.onTitleChange}>Go</button>
    )
  }
}
ReactDOM.render(<Parent />, document.querySelector("#app"))
```

工作提琴[这里](https://jsfiddle.net/mtyson/21fm7c6t/)。

所以它所做的是获取一个在父组件中定义的函数`onTitleChange`，并把它作为一个道具传递给子组件。

这是一个函数道具，或者换句话说，一个普通的 JavaScript 回调函数。

香草很好，因为香草很简单。

你必须用父级中的`bind(this)`来传入函数，这样当子级执行回调时，就会发生在父级的作用域内。

在我们接受这一点之前，应该注意的是，从 react 引擎的角度来看，这种方法会导致回调属性看起来像是重新呈现的。

这可能会影响性能。可能还不够担心，但是让我们看看更好的方法。

## 使用组件级箭头函数

获得正确范围的更好方法是在组件上使用箭头功能:

```
class Parent extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
     title: "Foo"
    }
  }
  onTitleChange = () => {
    this.setState({"title":"bar"})
  }  
  render() {
    return (
      <div>
        <h1>The Title: {this.state.title}</h1>
        <Child onTitleChange={this.onTitleChange}></Child>
      </div>
    )
  }
}
class Child extends React.Component {
  constructor(props){
    super(props);
  }
  render() {
    return (
      <button onClick={this.props.onTitleChange}>Go</button>
    )
  }
}
ReactDOM.render(<Parent />, document.querySelector("#app"))
```

所以在这里我们已经移除了`bind()`调用，只使用了 arrows 函数自动使用‘词法’作用域的事实。这意味着`this`将解析到父组件。

重要的是，我们在组件本身上定义函数，而不是在 render 方法中内联。(后一种方法与在 render 方法中调用 bind()具有相同的性能影响)。

所以这是一个处理亲子沟通的简单好方法，可以避免任何陷阱。

如果您确实想使用绑定方法，可以在构造函数中设置该函数，然后在 render 中使用它。

## 发回参数

通常我们会希望在函数调用时发送一些参数。没问题，将子调用更改为 lambda 包装的调用:

```
class Parent extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
     title: "Foo"
    }
  }
  onTitleChange = (newTitle) => {
    this.setState({"title":newTitle})
  }

  render() {
    return (
      <div>
        <h1>The Title: {this.state.title}</h1>
        <Child onTitleChange={this.onTitleChange}></Child>
      </div>
    )
  }
}class Child extends React.Component {
  constructor(props){
    super(props);
  }
  render() {
    return (
      <button onClick={() => this.props.onTitleChange("baz")}>Go</button>
    )
  }
}
ReactDOM.render(<Parent />, document.querySelector("#app"))
```

[拨弄这里。](https://jsfiddle.net/mtyson/21fm7c6t/18/)

## 功能组件

这是功能型的。

```
import React, { useState } from "react";
import ReactDOM from "react-dom";
function FChild(props) {
 return <button 
          onClick={() => props.onTitleChange("Baz")}>Go</button>; 
}
function Parent() {
  let [title, setTitle] = useState("Foo");
  const onTitleChange = newTitle => setTitle(newTitle);
  return (
    <div>
      <h1>The Title: {title}</h1>
      <FChild onTitleChange={onTitleChange} />
    </div>
  );
}
const rootElement = document.getElementById("root");
ReactDOM.render(<Parent />, rootElement);
```

顺便提一下，您可以看到相对于基于类的简化语法的好处:props 语法相当简洁。

[工作沙盒。](https://codesandbox.io/s/react-hooks-usestate-jf66b)

现在，我们介绍了`useState`钩。这个钩子只是让一个功能组件处理状态。底线是，其余的机制是完全相同的。

## 路由器

最后，既然它被如此广泛地使用，让我提一下，你可以直接通过一个路由器传递这样的函数属性，正如这里描述的。

```
<Route path=’/foopath’ render={(props) => <Child {…props} />} />
```

然后，您可以像以前一样继续操作(从父级开始):

```
<Route appProps={{ onTitleChange }} />
```