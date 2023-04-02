# React useRef 可以用的比你想象的多

> 原文：<https://javascript.plainenglish.io/react-useref-can-be-used-for-more-than-you-think-d4cfe7d90797?source=collection_archive---------1----------------------->

![](img/f1126c881068da048074f408327c0471.png)

Photo by [Tatiana Rodriguez](https://unsplash.com/@tata186?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

您可能已经使用了 *useRef* 钩子来访问 DOM 节点。

如果你在 ***ref*** 上搜索文章，这是你能找到的最常见的例子

```
import React, { Component, createRef } from "react";                                               class CustomTextInput extends Component {                              textInput = createRef();                                                      focusTextInput = () => this.textInput.current.focus();                                                  render() {                          
  return (                    
    <>                                 
   <input type="text" ref={this.textInput} />                               <button onClick={this.focusTextInput}>Focus the text input</button>                             </>                         
  );                 
        }                    
}
```

上面的例子展示了如果你有一个类组件，你将如何使用 refs。

如果您正在使用功能组件，这是您将采取的方法来实现同样的事情，

```
function TextInputWithFocusButton() {
  const inputEl = useRef(null);
  const onButtonClick = () => {
    // `current` points to the mounted text input element
    inputEl.current.focus();
  };
  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus the input</button>
    </>
  );
}
```

useRef 钩子的另一个常见用法是当你想获取表单中的输入时，就像这样，

```
function Nameform {

  const inputEl = useRef(null);  

  const handleSubmit=(event)=> {
    alert('A name was submitted: ' + inputEl.current.value);                 event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={handleSubmit}>
        <label>
          Name:
          <input type="text" ref={inputEl} />        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

> 但是，refs 的相同属性不仅可以用于存储 DOM 引用。

我将提供两个代码示例，演示如何使用*引用*，这可能有助于更好地理解*引用*。

## 示例 1

想象一下这样一个场景，当安装了一个特定的组件时，我们需要以特定的时间间隔向控制台记录一些东西。让我先在一个类组件中这样做，

```
**class** App **extends** React.Component{**constructor**(props){super();this.interval=null;this.state=true;}componentDidMount(){this.interval=setInterval(()**=>**{console.log("This is a log");},2000)}handleCancel=()**=>**{clearInterval(this.interval);}handleToggle=()**=>**{this.setState(()**=>**{return !this.state});}render() {console.log("App rendered");return <><h1>Hello</h1><button onClick={this.handleCancel}>Cancel Timer</button><button onClick={this.handleToggle}>Toggle State</button></>;}}
```

我们将计时器存储在一个 ***实例变量*** 中。我们也可以在状态中存储计时器，但这会导致额外的渲染。我们不想那样。

现在，如果我们想在一个功能组件中这样做，你将如何存储定时器？在功能组件中，我们没有实例变量。

> useRef 来救援

```
**function** App() {console.log("App rendered");**const** [state, toggle] = useState(true);**const** intervalRef = useRef();useEffect(() **=>** { **const** id = setInterval(() **=>** { console.log("This is a log") },2000); intervalRef.current = id; return () **=>** { clearInterval(intervalRef.current); };},[]);function handleClick(){ clearInterval(intervalRef.current);}return ( <div> <button onClick={handleClick}>Cancel Timer</button> <button onClick={()**=>**{ toggle(!state)}}>Toggle</button> </div>
)
```

这里， *intervalRef* 的行为就像一个实例变量。

如果你在想为什么我们不得不使用一个引用而不仅仅是一个局部变量，记住没有函数组件的*实例*，所以在下一次重新渲染时，先前与函数相关的数据会丢失。

尽管如此，我还是要说，你做这样的事可能不会受到惩罚，

```
let interval=null;
function App(){ // ...

 function handleClick(){ clearInterval(interval); }}
```

也就是说，将计时器引用存储在功能组件外部声明的变量中。

但是，这显然不是一个好的解决方案，不推荐使用。因为，首先，如果你不止一次的渲染同一个组件，这将会中断。

`*useRef*` *返回一个可变 ref 对象，其* `*.current*` *属性初始化为传递的参数(* `*initialValue*` *)。返回的对象将在组件的整个生命周期内保持不变—****React Docs****。*

## 示例 2

考虑这样一个场景，除了第一次(初始)渲染，你需要在每次渲染后对组件执行一些副作用。

在一个类组件中，这就像使用 **componentDidUpdate** 生命周期方法一样简单，因为在组件挂载时不会调用它。

但是，在一个功能组件中，我们有一个 useEffect 来应用副作用，做一些类似于，

```
 useEffect(() => {
   // do something
  })
```

这将在每次渲染时执行，不会起作用(因为效果将在初始渲染后执行)。

因此， *useRef* 再一次出手相救！

我们可以这样做，

```
const hasMounted= useRef(false)useEffect(() => {
    if (hasMounted.current) {
      // do something
    } else hasMounted.current = true
  }
```

为什么会这样？我想我在前面的例子中已经提到过:)

![](img/38df599f1bbb35e346581a27e292b5d4.png)

感谢您的阅读。