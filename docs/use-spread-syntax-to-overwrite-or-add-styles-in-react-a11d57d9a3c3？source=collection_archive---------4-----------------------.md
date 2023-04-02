# 使用扩展语法在 React 中覆盖或添加样式

> 原文：<https://javascript.plainenglish.io/use-spread-syntax-to-overwrite-or-add-styles-in-react-a11d57d9a3c3?source=collection_archive---------4----------------------->

![](img/6fa8b4d845a9e39e08ce4bbd4204df03.png)

Photo by [dylan nolte](https://unsplash.com/@dylan_nolte?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/spread?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在 React 中为内联样式使用变量是将样式模式应用于组件中多个元素的一个好方法。见下文。

```
import React from 'react'const MyComponent = () =>{ const myDivStyle = {
    color: 'green',
    textAlign: 'center',
    fontSize: 25
  } return (<div style={myDivStyle}>
      Hello World.
  </div>)
}export default MyComponent;
```

在本例中，`MyComponent`中的`div`将根据`myDivStyle`中设置的参数进行样式化。这很好，因为如果你想在你的组件中加入更多具有相同内联样式的元素，你可以简单地将`myDivStyle`传递给所有你想拥有这种样式的元素。

```
const MyComponent = () =>{ const myDivStyle = {
    color: 'green',
    textAlign: 'center',
    fontSize: 25,
    border: 'solid 1px 'green'   
}return (<div>
      <div style={myDivStyle}>Hello World.</div>
      <div style={myDivStyle}>Goodbye World.</div>
      <div style={myDivStyle}>Hello again,  World.</div>
      <div style={myDivStyle}>OK, Goodbye World (this time for real).</div>
    </div>)
}export default MyComponent;
```

现在所有这些`div`都将被应用相同的风格。但是如果您想让包含`Goodbye`的`div`字体颜色为红色而不是绿色呢？您可以创建一个新变量，它只适用于包含`Goodbye`的`div`。

```
const helloDivStyle = {
    **color: 'green',**
    textAlign: 'center',
    fontSize: 25,
    border: 'solid 1px green'}const goodbyeDivStyle = {
    **color: 'red',**
    textAlign: 'center',
    fontSize: 25,
    border: 'solid 1px green'
}
```

这是可行的，但是有一个更简单的方法。这些样式变量实际上是相同的，除了颜色。这意味着，我们不需要为了改变一个参数*而初始化另一个**。*这段代码不是很干。我们可以通过使用[扩展语法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)来使这段代码更加简洁。

![](img/d85d04669463674e0989f25219decb10.png)

Spread 语法允许我们利用三个句点来扩展 iterable，如数组或对象，并继承原始 iterable 的值。

```
var arr1 = [1, 2, 3, 4]
var arr2 = [...arr1]
console.log(arr1, arr2)> (4) [1, 2, 3, 4] (4) [1, 2, 3, 4]
```

在我们的例子中，我们可以使用 spread 操作符添加或覆盖额外的样式参数。见下文。

```
const MyComponent = () =>{const helloDivStyle = {
    color: 'green',
    textAlign: 'center',
    fontSize: 25,
    border: 'solid 1px green'
}return (<div>
      <div style={myDivStyle}>Hello World.</div>
      <div style={{...myDivStyle, color: 'red'}}>Goodbye World.</div>
      <div style={myDivStyle}>Hello again,  World.</div>
      <div style={{...myDivStyle, color: 'red'}}>OK, Goodbye World (this time for real).</div>
    </div>)
}export default MyComponent;
```

这样，我们可以向`Goodbye`元素添加变化的样式参数，而不必创建新的样式变量。但是，如果您想要替换多个样式参数，创建另一个变量可能会更有效。即使我们创建了一个新的样式变量，我们也可以使用 spread 操作符使我们的代码更加简洁，但是这次是在新的变量中。

```
const helloDivStyle = {
    **color: 'green',**
    textAlign: 'center',
    fontSize: 25,
    **border: 'solid 1px green'**
}const goodbyeDivStyle = {
    ...helloDivStyle,
    **color: 'red',**
    **border: 'solid 1px red',
    margin: 10** }
```

即使我们正在创建一个新的变量，我们也通过使用 spread 操作符来避免重复的样式赋值。在这种情况下，`goodbyeDivStyle`将拥有`helloDivStyle`提供的所有样式，并且只改变该变量之外的样式(在这种情况下，`color`、`border`和`margin`)。在这个例子中，您还可以看到 spread 操作符既可以覆盖继承的样式(`color` 和`border`)，也可以在继承的样式上添加新的样式参数(`margin`)。

[*在此将您的免费中级会员升级为付费会员*](https://matt-croak.medium.com/membership) *，每月只需 5 美元，您就可以获得数千位作家的无限量无广告故事。这是一个附属链接，你的会员资格的一部分帮助我为我创造的内容获得奖励。谢谢大家！*

# 参考

[](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax) [## 扩展语法

### Spread 语法允许在零个或多个…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax) 

# **用简单英语写的便条**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页[**plain English . io**](https://plainenglish.io/)找到所有这些内容——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**