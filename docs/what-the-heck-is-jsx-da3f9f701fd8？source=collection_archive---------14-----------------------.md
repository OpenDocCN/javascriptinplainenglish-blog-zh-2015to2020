# JSX 到底是什么？

> 原文：<https://javascript.plainenglish.io/what-the-heck-is-jsx-da3f9f701fd8?source=collection_archive---------14----------------------->

![](img/3aa1c9cfa0c6802f367b8b9c16dfb6a9.png)

[@baciutudor](https://unsplash.com/@baciutudor) unsplash.com

## 在 React 中学习这个最基本的概念

React 是一个 JavaScript 库，它使用一种叫做 JSX 的语法，代表 JavaScript XML。它是一种与 XML/HTML 非常相似的语法，可以与 JavaScript 代码共存。这意味着我们可以编写类似 HTML 的内容，并将其与 JavaScript 相结合。

这种语法旨在由 Babel 这样的预处理器使用，它将这种语法转换成 JavaScript 引擎可以运行的 JavaScript。

JSX 是一个简洁的类似 HTML 的结构，在我们编写 JavaScript 代码的同一个文件中。不像过去，我们可以把 HTML 放到 JavaScript 中。

让我们看一些代码，因为我们会更好地理解这样做的原因。

```
const html = <h1>Hello World</h1>
```

这看起来像 HTML 和 JavaScript 的交叉。巴别塔能够检测到这是 JSX，并将其转换为以下内容

```
const html = React.createElement('h1', null, "Hello World")
```

Babel 使用我们给它的 JSX 代码，获取标签和内容，并将它们用作 React.createElement 函数的参数。可以把 JSX 看作是调用这个函数`React.createElement`的一种简化方式。React 文档称之为 React.createElement 的“语法糖”

你可以看到 JSX 是多么容易阅读，尤其是当你开始嵌套 JSX。尽管它不是一个模板！这是必须编译成 JavaScript 的语法。

出于示例的目的，我们将假设 JSX 被转换，这有时被称为 React 渲染成显示在页面上的工作 DOM 节点。这就降低了本文的复杂性，只关注 JSX。

# 为什么用 JSX

JSX 不是由 React 创建的，它是 ECMAScript 的扩展。你可以在没有 JSX 的情况下使用 React，但下面是大多数人不这么做的原因。

1.  不太精通的编码人员可以很早上手，很容易理解和修改。设计师也更容易理解！
2.  您无需学习模板语言就可以利用 JavaScript 的强大功能。但是请记住，JSX 不是一个模板，它是一种表达 UI 组件树形结构的语法
3.  JSX 提倡内联风格的理念，这是对以前网站开发方式的转变

# JSX 规则

*   **JSX 标签的第一部分决定了 React 元素的类型。我们在一个简单的例子中看到了这一点。**
*   **大写标签表示 JSX 标签指的是 React 组件。**
*   **我们可以使用花括号**在我们的 JSX 中评估 JavaScript

```
const html = <h1> Hello {1+2} </h1>
```

如果我们要转换它并显示输出的 HTML，JavaScript 1+2 将进行计算，结果将是

```
Hello 3
```

*   我们可以嵌套这些 JSX 元素

```
const html = 
   <div> Here is a list 
      <ul> 
         <li>Item 1</li>
         <li>Item 2</li>
      </ul>
   </div>
```

React 会把这个渲染成一个物品列表！

*   您可以使用 JSX 表达式列表在页面上呈现一个列表。

这个更复杂，如果你没有得到这个不要担心。

```
const todos = ['finish doc','submit pr']
const html = 
    <ul>
      {todos.map(message =><li> {message}</li>}
    </ul>
```

如果我们给这个 JSX，让它在花括号内计算这个 JavaScript。在本例中，我们使用 map 函数创建一个 JSX 数组。我们获取 todos 数组项并包装一个`<li>`标签，输出是数组项的列表。

```
const html = 
   <ul> 
     {[<li> finish doc</li>,<li>submit pr</li>]}
   </ul>
```

然后 JavaScript 解释花括号中的 JavaScript，并呈现我们创建的带项目符号的数组项。

*   `**false**` **、** `**null**` **、** `**undefined**` **和** `**true**` **是有效的 JSX，但是它们不会被 React 渲染到页面上。**

```
<div>
<div></div>
<div>{false}</div>
<div>{null}</div>
<div>{undefined}</div>
<div>{true}</div>
```

请注意，有些错误的值会被渲染。例如，0 仍然会被渲染。

事实上，它们是有效的 JSX，并且它们不会被渲染到页面上的任何内容，这意味着我们可以创建一些情况，在这些情况下，我们可以有条件地渲染某些 JSX。

*   **根据条件，我们可以告诉 React 我们想要渲染什么特定的 JSX**

现在，假设一个首字母大写的标签是一个 React 组件，不要担心知道你是否熟悉它。React 构建从元素到组件，因为它变得越来越复杂，它们可以用 JSX 编写，如下所示。

```
<div>
   {showHeader && <Header />}
   <Content />
</div>
```

这里，如果 showHeader 变量为 true，我们希望显示 header 组件。如果 showHeader 为 false，屏幕上将看不到标题组件！

这不是 JSX 的末日。但是为了理解如何正确地使用它以及它如何正确地适合 React 代码，我们必须理解一些其他的概念。比如 React 如何把这个 JSX 变成页面上的东西。

`ReactDOM.render()`函数将我们所有的 JSX 最终转换成 DOM 节点。我们还必须理解什么是组件以及如何创建 React 组件。最后，为了充分利用 JSX，我们需要理解道具的概念。Prop 代表属性，这是 React 将数据向下传递到组件的方式。这非常有用，我们会讲到的！

# 其他文章

[](https://medium.com/javascript-in-plain-english/why-you-should-care-about-how-the-browsers-work-in-react-749bcbecc32f) [## 为什么您应该关心浏览器在 React 中的工作方式

### 了解 DOM 与 JavaScript 的关系

medium.com](https://medium.com/javascript-in-plain-english/why-you-should-care-about-how-the-browsers-work-in-react-749bcbecc32f) [](https://medium.com/javascript-in-plain-english/why-do-we-have-to-wrap-react-components-b168232dbd3a) [## 为什么我们必须包装 React 组件？

### 理解 React 应用程序中的 div 包装！

medium.com](https://medium.com/javascript-in-plain-english/why-do-we-have-to-wrap-react-components-b168232dbd3a) [](https://medium.com/javascript-in-plain-english/why-you-should-be-using-react-fragments-a5d8314a59ff) [## 为什么应该使用 React 片段

### 如何使用 React 提升 React 应用程序？碎片

medium.com](https://medium.com/javascript-in-plain-english/why-you-should-be-using-react-fragments-a5d8314a59ff) 

# 关于作者

我是一名执业医师和教育家，也是一名网站开发者。请点击此处查看我的博客和其他帖子中关于项目的更多细节。如果你想和我联系，请在这里联系:[aaron . Smith . 07 @ aberdeen . AC . uk](mailto:aaron.smith.07@aberdeen.ac.uk)或者在推特上@aaronsmithdev。