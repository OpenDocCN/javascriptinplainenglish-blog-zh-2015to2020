# 自定义 React 挂钩以在浏览器窗口间共享状态

> 原文：<https://javascript.plainenglish.io/a-react-hook-to-share-state-between-browser-windows-a672470f66ff?source=collection_archive---------0----------------------->

![](img/7b8902d04bbf4a749cdb5636bf45e0ec.png)

Photo by [Juan Davila](https://unsplash.com/@juanster?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/t/nature?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

有时在一个前端应用程序中，我们需要在浏览器上打开的同一个域的多个标签之间进行通信。这个用例的一个很好的例子是一个电子商务网站。假设您打开多个选项卡来查看不同的产品，并且在查看时将这些产品添加到购物车中。你所期望的用途是看到你的购物车在任何标签页上更新！

这是我创建的一个简单的演示示例。在两个选项卡上打开这个[示例应用程序](https://mostafa-drz.github.io/react-cross-windows-state/),并将商品添加到您的购物车中！正如你所看到的，应用程序的状态是在浏览器的选项卡之间共享的，但是我们如何做到这一点呢？

## 一点背景:跨窗口交流

如果你想在同一个域的多个窗口之间进行通信，有不同的方法。进行这种通信的一些常见方式是[窗口 PostMessage API](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage) 、[广播通道 API](https://developer.mozilla.org/en-US/docs/Web/API/Broadcast_Channel_API) 以及通过本地存储进行通信。我不会在这篇文章中讨论前两种交流方式，但是你可以在网上和文档中找到这两种方式的很多例子。我将重点介绍使用 Localstorage 进行通信，因为我们也希望保持我们的状态，这样下次访问页面时，我们仍然可以访问状态。

## 局部存储器

[本地存储](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)是浏览器上可用的存储之一。通过使用它提供的一个非常简单的 API，您可以简单地添加项目和读取存储。这里有一个简单的例子:

```
localStorage.setItem('name','mostafa')console.log(localStorage.getItem('name'))
// mostafa
```

正如你所看到的，这个 API 非常容易使用，你可以传递一个键/值给`localStorage.setItem`，然后用这个键调用`localStorage.getItem`来获取这个值。
但是，关于本地存储，还有一点我们将在我们的使用案例中使用。您可以监听 localStorage 中的所有更改:

```
window.addEventListner('storage',(e)=>{// Something has changed on localStorage 
})
```

在`storage`上收到的事件有几个属性，其中两个是我们需要的:`key`和`newValue`。基本上，我们想要做的是在每个选项卡上设置 React 状态，我们也将它保存在`localStorag`中，这样其他选项卡就可以监听这个变化并更新它们自己的状态。

## 最终解决方案之前的一个小例子

这是一个小的 React 组件，我们希望在这个应用程序的不同选项卡之间共享状态:

```
import React, { useState, useEffect } from "react";function HelloStorage() {
  const [name, setName] = useState(""); useEffect(() => {
    localStorage.setItem("name", name);
  }, [name]); useEffect(() => {
    const onReceiveMessage = (e) => {
      const { key, newValue } = e;
      if (key === "name") {
        setName(newValue);
      }
    };
    window.addEventListener("storage", onReceiveMessage);
    return () => {
      window.removeEventListener("storage", onReceiveMessage);
    };
  }, []);const handleChange = (e) => {
    setName(e.target.value);
  };
  return <input value={name} onChange={handleChange} />;
}
```

在上面的代码中，您可以看到有两个`useEffect`，第一个监听我们状态的变化，如果有变化，它在 localStorage 上设置状态。
还有第二个`useEfffect`，它为`storage`添加了一个事件监听器，每次存储上有变化时，它检查变化的键，如果是我们的状态，那么它更新组件内部的状态。(基本上，这意味着另一个选项卡更新了此状态)

出现的第一个问题是我们会陷入一个循环吗？🤔如果我们随着每个本地存储的改变而更新状态，那么对于改变状态的选项卡来说，这似乎是一个无限循环(更新状态—>更新本地存储—>更新状态)！答案是否定的！我们不会陷入循环。因为`storage`事件不会在事件的原点激发事件。换句话说，改变状态的选项卡没有得到`storage`事件，所以唷👏

如果您照原样运行这段代码，您会看到一个问题，因为每次您打开一个新的选项卡时，新的状态都是空的，您猜怎么着！它将状态设置为跨选项卡的空字符串。我们不会在这个小样本代码中解决这个问题，但是在我们的最终解决方案中，我们会解决这个问题。

## 自定义的 React 挂钩，用于在浏览器选项卡之间共享状态

在本节中，我假设您熟悉 React 挂钩以及如何构建定制的 React 挂钩。如果你想了解更多，我推荐你看看 React 文档上的[钩子部分。](https://reactjs.org/docs/hooks-intro.html)

首先，让我把最终版本放在这里，然后我们一起看一下:

```
function useCrossTabState(stateKey,defaultValue){
  const [state,setState] = useState(defaultValue)
  const isNewSession = useRef(true)useEffect(()=>{
    if(isNewSession.current){
      const currentState = localStorage.getItem(stateKey)
      if(currentState){
        setState(JSON.parse(currentState))
      }else{
         setState(defaultValue)
      }
      isNewSession.current=false
      return
    }
    try{
      localStorage.setItem(stateKey,JSON.stringify(state))
    }catch(error){}
  },[state,stateKey,defaultValue])useEffect(()=>{
    const onReceieveMessage = (e) => {
     const {key,newValue} = e
    if(key===stateKey){
       setState(JSON.parse(newValue))
    } 
    }
    window.addEventListener('storage',onReceieveMessage)
    return () => window.removeEventListener('storage',onReceieveMessage)
  },[stateKey,setState])return [state,setState]
}
```

我喜欢 React hooks 的一点是，你可以很容易地构建一些定制的钩子，在你的应用程序中使用，以获得更模块化和更干净的代码。

这个定制钩子接受两个输入:`stateKey`和一个`defaultValue`。基本上，`stateKey`是我们调用`localStorage.setItem`时想要使用的键，如果`localStorage`上还没有设置值，那么`defaultValue`是状态的默认值。

我们现在跳过`isNewSession`模块，先来看看`useEffect`:

```
useEffect(()=>{
    try{
      localStorage.setItem(stateKey,JSON.stringify(state))
    }catch(error){}
  },[state,stateKey])
```

在这一部分中，与上一个示例相同，我们在每次状态改变时都在 localStorage 上设置状态。
现在我们需要监听`storage`变化，如果变化与我们的状态有关，我们需要更新我们的状态:

```
useEffect(()=>{
    const onReceieveMessage = (e) => {
     const {key,newValue} = e
    if(key===stateKey){
       setState(JSON.parse(newValue))
    } 
    }
    window.addEventListener('storage',onReceieveMessage)
    return () => window.removeEventListener('storage',onReceieveMessage)
  },[stateKey,setState])
```

到目前为止，这类似于我们在第一个例子中所做的，除了它更灵活，我们可以在应用程序的不同状态下使用它。

现在来说说`isNewSession` block 是做什么的？如果你还记得我提到的第一个例子，这种方法的问题是，如果你打开一个新标签，新标签有默认状态(在我们的例子中是一个空字符串)，它用默认状态更新存储，它会覆盖标签共享的状态。
为了解决这个问题，我们需要检查这是否是一个新的会话，组件是否正在被渲染，如果是这样，我们就跳过设置`localStorage`的状态。

```
if(isNewSession.current){
      const currentState = localStorage.getItem(stateKey)
      if(currentState){
        setState(JSON.parse(currentState))
      }else{
         setState(defaultValue)
      }
      isNewSession.current=false
      return
    }
```

现在我们可以使用我们的自定义钩子重写我们的第一个例子:

```
import React from "react";
import { useCrossTabState } from "./hooks";function App() {
  const [name, setName] = useCrossTabState("name", "");
  const handleChange = (e) => {
    setName(e.target.value);
  };
  return <input value={name} onChange={handleChange} />;
}export default App;
```

请记住，状态键在域中应该是唯一的，以避免任何覆盖。

你可以在我的 GitHub 上看到使用这个钩子的另一个例子:[https://github.com/mostafa-drz/react-cross-windows-state](https://github.com/mostafa-drz/react-cross-windows-state)

我希望这个定制钩子能帮助你的项目，并且你喜欢这篇文章。请不要犹豫，与我分享你的意见，你会如何改善这个自定义挂钩，或者如果你有任何想法。🍻

[***莫斯塔法***](https://www.linkedin.com/in/mostafa-darehzereshki/)