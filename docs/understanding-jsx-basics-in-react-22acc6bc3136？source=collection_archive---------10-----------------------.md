# 了解 React 中的 JSX 基础知识

> 原文：<https://javascript.plainenglish.io/understanding-jsx-basics-in-react-22acc6bc3136?source=collection_archive---------10----------------------->

## 通过实例了解 JSX 基础知识

![](img/68ad50620978d1d2cea2ea9ccfb976b9.png)

Photo by [Ferenc Almasi](https://unsplash.com/@flowforfrank?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

React 是一个用户界面库，它将 HTML 和 JavaScript 功能结合起来，创建了自己的标记语言， **JSX** 。这有几个好处。它允许您在 HTML 中使用 JavaScript 的全部编程能力，并有助于保持代码的可读性。

在这篇文章中，我们将学习一些你在 React 中需要知道的 JSX 基础知识。让我们开始吧。

# 创建一个简单的 JSX 元素

我们在 JavaScript 文件中编写 React JSX。它类似于你已经学过的 HTML。但是，您需要知道一些关键的区别。因为 JSX 不是有效的 JavaScript，JSX 代码必须编译成 JavaScript。巴别塔是这一过程的流行工具。如果你使用“创建反应应用”，你不必担心这一点，因为它已经被添加到引擎盖下。

为了让我们都在同一页上，我们从这个非常基本的 React 代码开始:

```
import React from "react";
import ReactDOM from "react-dom";

const Component = () => {
  **return <div>Hello React</div>;**
};

ReactDOM.render(<Component />, document.getElementById("root"));
```

如您所见，函数`Component`返回一些 UI，一个 div 元素。这就是我们所说的 JSX。它允许您在 JavaScript 中返回组件内的任何 HTML 元素。

例如，因为 JSX 是 JavaScript 的语法扩展，你可以直接在 JSX 中写 JavaScript。为此，您只需在花括号中包含您希望被视为 JavaScript 的代码:`{code}`。

这里有一个例子:

```
import React from "react";
import ReactDOM from "react-dom";

const name = "Mehdi";

const Index = () => {
  return <div>Hello **{ name }**!</div>;  //Returns: Hello Mehdi!
};

ReactDOM.render(<Index />, document.getElementById("root"));
```

你也可以调用那些花括号里的函数。所以，无论何时你想在 JSX 内部写 JavaScript，你都必须像我们上面做的那样加上那些花括号。

# 创建一个复杂的 JSX 元素

关于嵌套 JSX，需要知道的一件重要事情是，它必须返回单个元素。这一个父元素将包装所有其他级别的嵌套元素。因此，如果在 JSX 中有多个元素，就必须在一个 div 中返回它们。

这里有一个例子:

***有效 JSX:***

```
return(
<div>
  <p>Paragraph One</p>
  <p>Paragraph Two</p>
  <p>Paragraph Three</p>
</div>
);
```

***无效 JSX:***

```
return(
  <p>Paragraph One</p>
  <p>Paragraph Two</p>
  <p>Paragraph Three</p>
);
```

# 将 HTML 元素呈现给 DOM

有了 React，我们可以使用 React 的渲染 API React DOM 将这个 JSX 直接渲染到 HTML DOM。

ReactDOM 提供了一个简单的方法来将 React 元素呈现给 DOM，如下所示:

```
const Component = () => {
  return <div>Hello React</div>**;**
};

**ReactDOM.render(<Component />, document.getElementById("root"));**
```

如您所见，我们已经在组件中包含了 JSX。所以如果我们想要渲染 JSX，我们可以渲染这个组件。

ReactDOM 提供了一个将 React 元素呈现给 DOM 的简单方法，如下所示:`**ReactDOM.render(componentToRender, targetNode)**`，其中第一个参数是要呈现的 React 元素或组件，第二个参数是要呈现组件的 DOM 节点。

# 在 JSX 定义一个 HTML 类

我们可以像在 HTML 中一样在 JSX 中添加类，但是有一些关键的区别。

JSX 的一个关键区别是你不能再用单词`class`来定义 HTML 类。这是因为`class`是 JavaScript 中的保留字。相反，JSX 使用`className`。

这里有一个例子:

```
return (
<div>
 <h1 **className="mydiv"**>Hello</h1>
</div>
);
```

事实上，JSX 中所有 HTML 属性和事件引用的命名约定都变成了 camelCase。例如，JSX 的一个点击事件变成了“`onClick`，而不是`onclick`。

# JSX 的自闭标签

JSX 与 HTML 的另一个重要区别在于自结束标签的概念。在 HTML 中，换行符可以写成`<br>`或者`<br />`，但是不应该写成`<br></br>`，因为它不包含任何内容。

在 JSX，规则有点不同。任何 JSX 元素都可以用自结束标记来编写，并且每个元素都必须是结束的。例如，为了成为有效的 JSX，换行符标签必须总是被写成:`<br />`。这同样适用于图像标签和“hr”标签。

下面是 JSX 的自结束标签的一个例子:

```
return(
 <div>
  <h2>Welcome to React!</h2> **<br />**
  <p>Be sure to close all tags!</p>
  **<hr />** </div>
);
```

# 结论

正如你所看到的，JSX 是一个在 JavaScript 中编写可读 HTML 的方便工具。我认为这是了解 JSX 和 React 的良好开端。然而，我鼓励你从官方[文档](https://reactjs.org/docs/getting-started.html)中了解更多。

感谢您阅读本文，希望您觉得有用。

## 进一步阅读

[](https://medium.com/javascript-in-plain-english/5-amazing-front-end-development-tools-that-you-should-know-7372dc377d7) [## 你应该知道的 5 个惊人的前端开发工具

### 每个开发人员都应该知道的有用的前端开发工具

medium.com](https://medium.com/javascript-in-plain-english/5-amazing-front-end-development-tools-that-you-should-know-7372dc377d7)