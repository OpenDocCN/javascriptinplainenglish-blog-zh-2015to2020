# React 里的道具是什么？

> 原文：<https://javascript.plainenglish.io/what-are-props-in-react-a9c151fcd4b3?source=collection_archive---------14----------------------->

![](img/902da136532cfdf4f46e9c3b7368dee2.png)

[@grakozy](https://unsplash.com/@grakozy) unsplash.com

## 在 React 中学习这个基本概念！

在上一篇文章中，我们讨论了 React 中的渲染，并以一个简单的例子来演示 React 元素。但这并不能反映每个使用 React 的人在创建应用程序时都做了什么。在本文中，我们将讨论组件的概念和一个重要的概念，称为 props，它介绍了数据如何在 React 中流动。

React 组件允许将应用程序拆分成离散的、可重用的用户界面。一个比较准确的类比是，React 组件非常像 JavaScript 函数。

React 组件可以是函数组件，也可以是类组件。让我们先处理功能组件。

定义 React 函数组件最简单的方法是编写一个函数

```
function Welcome(props) {
  return <h1>Hello {props.name} </h1>
}
```

这几乎看起来像一个普通的 JavaScript 函数。这个函数组件接受一个 props 参数。Props 代表属性，我们将会谈到它们，但是现在，把 props 看作一个对象，它携带可以在我们的函数组件中使用的数据。该函数组件返回一些访问 props 对象关键字“name”的 JSX。

# 呈现 React 组件

在 JSX，我们可以这样表示 React 组件

```
<Welcome />
```

在我们的环境中，React 组件接受一个 props 参数。现在，当我们在 JSX 编写一个 React 组件时，我们可以定义 props 对象是什么。

```
<Welcome name='Sarah' />
```

这里我们说我们希望 props 对象有键“name”和值“Sarah”。我们称之为 JSX 属性。当我们定义这个属性时，意味着我们用一个名为 Sarah 的键和值来定义 prop。所以现在在我们的函数组件中，我们可以通过 props.name 来访问这个值！

知道了这一点，我们可以看看如何渲染这个简单的组件

```
function Welcome(props) {
  return <h1>Hello {props.name} </h1>
}const element = <Welcome name='Sarah' />ReactDOM.render(
  element, 
  document.getElementById('root')
)
```

这里我们调用了`ReactDOM.render`函数。React 识别出这是一个组件。它将“属性”名称传递给我们称为 props 的组件。React 然后处理这个函数。该函数返回 JSX，由 React 渲染并更新 DOM。然后在屏幕上显示输出。

注意！你应该总是以大写字母开始一个组件，`<div />`代表一个 HTML 标签，但是`<Div />`被解释为一个组件。

现在我们已经了解了什么是组件以及如何渲染它们。我们需要更进一步，看看我们如何构建一个像 React 这样的应用程序。我们已经讨论过这样一个事实，即组件是离散的代码片段，可以分割用户界面的各个部分。

所以组件的关键是我们可以在它们的输出中引用其他组件。当我们创建一个应用程序时，我们会创建一个名为的功能组件，我们可以引用多个组件，将应用程序拆分为独立的用户界面。

```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

这里有我们的应用程序组件，它返回具有不同属性的欢迎组件的三个迭代。然后我们调用呈现 App 组件的`ReactDOM.render()`。当我们调用这个函数时，我们实际上触发了 React 来呈现三个欢迎组件。

这种设置的美妙之处在于我们可以将用户界面分割成更小更简单的组件。请注意，我们不必在 App 组件中包含欢迎功能组件。这允许我们提取组件，使代码更具可读性。

功能组件和道具的关键是道具不应该被功能组件修改。我们称之为纯函数，不改变它的输入。然而，我们知道复杂应用程序中的事情会发生变化，React 中有一种方法可以处理这种可能性。

# 结论

在本文中，我们定义了什么是组件，以及为什么它是 React 应用程序的核心。组件的概念意味着我们可以将一个非常复杂的应用程序分解成许多小组件。

对于组件，我们还必须有一种方法将数据传输到这些组件中。这就是道具概念的由来，因为函数组件的行为很像函数，把道具想象成一个对象，我们像函数一样传递参数。

我们可以通过代表组件的 JSX 的属性来定义道具。我们看到了一个例子。这意味着我们可以用不同的数据呈现同一组件的多次迭代。

## 进一步阅读

[](https://medium.com/javascript-in-plain-english/why-you-should-care-about-how-the-browsers-work-in-react-749bcbecc32f) [## 为什么您应该关心浏览器在 React 中的工作方式

### 了解 DOM 与 JavaScript 的关系

medium.com](https://medium.com/javascript-in-plain-english/why-you-should-care-about-how-the-browsers-work-in-react-749bcbecc32f) [](https://medium.com/javascript-in-plain-english/why-do-we-have-to-wrap-react-components-b168232dbd3a) [## 为什么我们必须包装 React 组件？

### 理解 React 应用程序中的 div 包装！

medium.com](https://medium.com/javascript-in-plain-english/why-do-we-have-to-wrap-react-components-b168232dbd3a) [](https://medium.com/javascript-in-plain-english/why-you-should-be-using-react-fragments-a5d8314a59ff) [## 为什么应该使用 React 片段

### 如何使用 React 提升 React 应用程序？碎片

medium.com](https://medium.com/javascript-in-plain-english/why-you-should-be-using-react-fragments-a5d8314a59ff) 

# 关于作者

我是一名执业医师和教育家，也是一名网站开发者。请点击[这里](https://dev.to/aaronsm46722627/www.coding-medic.com)了解我在博客和其他帖子上关于项目的更多细节。如果你想和我联系，请在这里联系:[aaron . Smith . 07 @ aberdeen . AC . uk](mailto:aaron.smith.07@aberdeen.ac.uk)或在推特上@aaronsmithdev。