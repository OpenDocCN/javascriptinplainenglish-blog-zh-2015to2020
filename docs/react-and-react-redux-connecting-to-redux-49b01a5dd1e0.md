# React 和 React Redux —连接到 Redux

> 原文：<https://javascript.plainenglish.io/react-and-react-redux-connecting-to-redux-49b01a5dd1e0?source=collection_archive---------8----------------------->

Redux 是一个轻量级的状态管理工具，帮助 React 应用中的组件相互通信。这背后的简单概念是，组件的每个状态都保存在一个全局存储中。以便每个组件都可以从该存储中访问任何状态。

![](img/95f4db91032198318a1606d2682d158d.png)

Cover Image

在这篇博客中，我们来看一个 React-Redux 的简单例子。逻辑是用 React-Redux 增加和减少一个按钮上的数字。

## **目录**

*   [创建 React 应用和初始设置](#ee30)
*   [设计我们的视图](#3efb)
*   [连接 React 到 Redux](#0b3f)
*   [调度动作](#80f3)

## **1。创建 React 应用程序和初始设置**

首先，让我们创建一个新的 React 应用程序。此外，检查创建的项目是否一切正常。

```
npx create-react-app reactapp
cd reactapp
```

现在让我们安装 Redux。

```
npm install redux
npm install react-redux
npm start
```

现在打开 [http://localhost:3000/](http://localhost:3000/) 检查服务器是否启动良好。现在让我们创建我们的视图。

## 2.**设计我们的视图**

这里，我创建了一个简单的名为`Counter.js`的 React 类组件，它有两个按钮和一个文本，其中一个按钮用于递增，另一个用于递减。递增和递减的值将显示在段落标记中。

`src/Counter.js`

```
import React, { Component } from 'react'export default class Counter extends Component {
render() {
  return ( 
      <div>
          <h1>Counter</h1>

 **<button style={{ height: '40px', width: '150px' }}>Increment + </button>

          <p>4</p>

          <button style={{ height: '40px', width: '150px' }}>Decrement - </button>**
      </div>
    )
  }
}
```

创建计数器视图后，我将计数器组件包含在我的`App.js`文件中。

`src/App.js`

```
import React from 'react';
import logo from './logo.svg';
import './App.css';
import Counter from './Counter'function App() {
  return (
    <div className="App">
 **<Counter />**    </div>
  );
}export default App;
```

现在打开浏览器，导航到 [http://localhost:3000/](http://localhost:3000/) 。你一定会看到这样的东西。

![](img/73fb7b2363c89cd03300216103f4293c.png)

Counter View

## 3.**连接 React 到 Redux**

我们知道一个 Redux 应用有一个全球商店。为了将我们的组件连接到那个全球商店，我们使用了一个叫做`connect.`的东西来帮助我们从商店中获取所有的数据。此外，它还帮助我们分派行动。

`src/Counter.js`

```
import React, { Component } from 'react'
**import { connect } from 'react-redux'**export class Counter extends Component {
  render() {
    return (
      <div>
        <h1>Counter</h1> <button style={{ height: '40px', width: '150px' }}>Increment +</button> <p>**{this.props.val}**</p> <button style={{ height: '40px', width: '150px' }}>Decrement -</button> </div>
    )
  }
}**const mapStateToProps = state => ({ 
    val: state.val
})**export default **connect(mapStateToProps)**(Counter)
```

MapStateToProps 用于从存储中选择一部分数据。我们只能通过 MapStateToProps 获得商店中的所有数据。它也被称为 MapState。

我们已经将组件连接到 redux 存储。但到目前为止，我们还没有创建任何商店。所以现在让我们开始吧。为了创建一个商店，我们使用`createStore`。这需要一个参数，这是我们的减速器。这个缩减器有两个参数，初始状态和动作。顾名思义，初始状态表示任何状态的初始值。这里我们正在执行递增和递减操作。所以我们的初始状态是 0。一旦完成，通过`Provider`提供状态作为 pros 来包装`Counter`组件。

`src/App.js`

```
import React from 'react';
import logo from './logo.svg';
import './App.css';
import Counter from './Counter'
**import { createStore } from 'redux'
import { Provider } from 'react-redux'****const initialState = {
  val: 0
}****function reducer(state = initialState, action){
  return state;
}****const store = createStore(reducer);**function App() {
  return (
    <div className="App">
 **<Provider store={store}>
        <Counter />
      </Provider>**
    </div>
  );
}export default App;
```

现在你必须看到我们的初始值被打印在这些按钮之间。但是，递增和递减函数仍然不起作用，因为我们还没有为这些按钮分派任何操作。所以现在让我们开始吧。

## 4.**调度动作**

在我们的减速器内部，创建一个开关盒，如果动作是`INC`则执行递增操作，如果动作是`DEC`则执行递减操作。

`src/App.js`

```
function reducer(state = initialState, action){
 ** switch (action.type) {
    case "INC":
      return {
        val: state.val + 1
      }
    case "DEC":
      return {
        val: state.val - 1
      }
    default:
      return state;
   }**
}
```

现在，创建两个递增和递减的函数。分别在增量和减量按钮的 onClick 中调用这些函数。

在 increment 函数中，调度类型为`INC`的动作，并对动作类型为`DEC.`的 decrement 函数进行同样的操作

`src/Counter.js`

```
export class Counter extends Component {**increment = () =>{
  this.props.dispatch({type: 'INC'})
}****decrement = () =>{
  this.props.dispatch({type: 'DEC'})
}**render() {
  return (
    <div>
      <h1>Counter</h1> <button style={{ height: '40px', width: '150px' }} **onClick={this.increment}**>Increment +</button> <p>{this.props.val}</p> <button style={{ height: '40px', width: '150px' }} **onClick={this.decrement}**>Decrement -</button> </div>
  )
 }
}
```

现在递增和递减应该可以工作了。最初，这可能会令人困惑。自己用不同的例子反复练习。只有这样，你才会明白数据是如何流动的。

在我的下一篇博客中，我将解释如何用 React-Redux 执行 CRUD。保持联系。

如有任何疑问，请随时联系我。邮箱:sjlouji10@gmail.com 领英:[https://www.linkedin.com/in/sjlouji/](https://www.linkedin.com/in/sjlouji/)

完整的代码可以在我的 Github 上找到:[https://github.com/sjlouji/Connecting-React-with-Redux](https://github.com/sjlouji/Connecting-React-with-Redux)

编码快乐！