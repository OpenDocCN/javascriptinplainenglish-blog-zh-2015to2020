# 为什么在 React 中运行 setState()后总是得到“Null”

> 原文：<https://javascript.plainenglish.io/why-you-keep-getting-null-after-running-setstate-in-react-299e1835bd92?source=collection_archive---------3----------------------->

![](img/555a19e062d60309e69bb76df7761a43.png)

Photo by [Blake Connally](https://unsplash.com/@blakeconnally?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我最近开始学习 React，我已经认定它是我最喜欢的 JavaScript 库。在我的第一个 React 项目(构建一个简单的游戏)中，我在使用 this.setState 更改组件状态时遇到了一个问题。我没有选择我的玩家的武器，也没有在 DOM 中忠实地弹出获胜的玩家，而是一直得到一个巨大的“空”为什么？因为 **React 的 setState 有时候是异步的**。

以下是文档对 setState()的描述:

> 把`setState()`看作是一个*请求*而不是一个立即更新组件的命令。为了获得更好的性能，React 可能会延迟它，然后一次更新几个组件。React 不保证立即应用状态更改。
> 
> `setState()`并不总是立即更新组件。它可以批处理或推迟更新，直到以后。这使得在调用`setState()`之后立即读取`this.state`成为一个潜在的陷阱。相反，使用`componentDidUpdate`或`setState`回调(`setState(updater, callback)`)，这两种方法都保证在应用更新后触发。如果您需要根据之前的状态来设置状态，请阅读下面的`updater`参数。

因为这不能保证立即更新，所以如果您在调用 setState()后直接依赖更新后的状态，就会遇到麻烦。

```
play = () => { 
  //function setting state of computer move
  this.random(); //setting state of player move based on input value 
  this.setState({      
    playerMove: document.querySelector('.move-input').value,    
  }); //this function did not work because the player value returned 
  "null" 
  this.whoWins(); 
}
```

## 异步与同步

**同步功能**以“一次一个客户”的态度处理，直到功能完成或弹出错误才继续。**异步函数**将开始执行代码，但是让函数在后台做它的事情，而你的代码继续运行，只有当异步函数中的代码已经完成运行时才返回(通过像回调或承诺这样的方法)。异步函数对于像 API 请求这样的较长流程非常有用，因为您不希望用户在您的代码运行时无所事事，试图找到正确的数据或文件。

**那么有什么解决办法呢？**

了解到 setState 是异步的，您需要确保您的后续代码在状态完成更改之前不会运行。有多种方法可以告诉你的代码“嘿，在上面的异步函数停止运行之前，不要启动这个函数。”这些包括回调、承诺、async/await、componentDidUpdate…因为我的程序非常简单，所以我用回调解决了我的问题。

```
play = () => { 
  //function setting state of computer move 
  this.random();//setting state of player move basec on input value 
  this.setState({      
    playerMove: document.querySelector('.move-input').value,    
  },   //using a callback, this function now works!
  () => this.whowins());
}
```

编码快乐！我很乐意听到您的反馈或替代解决方案。

## 延伸阅读:

[反应文件](https://reactjs.org/docs/react-component.html#setstate)

小帕特里克·布朗的[异步 vs 同步](https://medium.com/@pjbrn26/async-vs-sync-d369a4ef95e5)

[当心:React setState 是异步的！](https://medium.com/@wereHamster/beware-react-setstate-is-asynchronous-ce87ef1a9cf3)托马斯·卡尼基

[MDN web docs 引入异步 JavaScript](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Introducing)

Flavio Copes 的 JavaScript 异步编程和回调