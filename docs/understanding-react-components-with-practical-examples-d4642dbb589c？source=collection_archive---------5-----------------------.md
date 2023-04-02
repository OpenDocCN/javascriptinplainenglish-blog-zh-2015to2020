# 通过实例了解 React 组件

> 原文：<https://javascript.plainenglish.io/understanding-react-components-with-practical-examples-d4642dbb589c?source=collection_archive---------5----------------------->

## 了解 React 中的组件

![](img/286d25f3bea0ae0f999ce1b8e57df4b6.png)

Photo by [Alvaro Reyes](https://unsplash.com/@alvarordesign?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

我喜欢 React 的一点是，它是一个基于组件的库。它基本上允许您将整个应用程序分成更小的代码组，这就是所谓的基于组件的方法。这对于在应用程序的不同部分重用代码也很有用。组件被认为是 React 应用程序的核心构建块。这就是为什么如果你正在学习反应，这是你必须了解的事情。

在本文中，我们将通过一些实际例子来学习 React 组件，以帮助您理解这个主题。让我们开始吧。

# 什么是 React 组件？

React 组件就像一个返回 HTML 元素或 UI 的函数。

组件是独立的和可重用的，这意味着您可以在应用程序的不同部分使用它们，而不必再次编写相同的代码。它们的作用与 JavaScript 函数相同，但是独立工作，并通过呈现函数返回 HTML。

有两种类型的组件:

1.  *功能组件、*也称为**无状态组件**，因为它们不持有或管理状态。功能组件是一种编写组件的方式，它只包含一个呈现方法，没有它们的状态。
2.  *类组件*，也称为**有状态组件，**因为持有或管理本地状态。这些组件比无状态组件更复杂。它需要你从 React 扩展。可以通过定义一个扩展组件并具有呈现功能的类来创建该类。

# 功能组件

创建 React 组件时，组件名称必须以大写字母开头。正如我说过的，函数组件返回 JSX，它将由 render 方法呈现。

如果你对 JSX 感到困惑，它就像 HTML，但有一些关键的区别。我为此写了一整篇文章。

[](https://medium.com/javascript-in-plain-english/understanding-jsx-basics-in-react-22acc6bc3136) [## 了解 React 中的 JSX 基础知识

### 通过实例了解 JSX 基础知识

medium.com](https://medium.com/javascript-in-plain-english/understanding-jsx-basics-in-react-22acc6bc3136) 

以下是一个功能组件的示例:

```
import React from 'react';
import ReactDOM from 'react-dom';// Creating our function component.
**const Component = () =>{
 return(
  <div>
   <h2>This is a React Component</h2>
  </div>
 )
}**// Rendering the component with the render method.
ReactDOM.render(
**<Component />**,
document.getElementById('root'));
```

如您所见，我们创建了一个名为`**C**omponent`的组件，首字母大写。然后，我们使用`ReactDOM.render()`呈现根元素中的组件。为了调用 render 方法中的组件，我们使用了自结束标记`<Component />`。

# 类组件

为了创建类组件，您需要对 JavaScript 中的类有基本的了解。所以一定要记住这一点。

该组件必须包含语句`extends React.Component`，该语句创建了对`React.component`的继承，并为您的组件提供了对`React.component`函数的访问。

该组件还需要一个返回 JSX 的方法`render()`。

下面是一个类组件的示例:

```
class **Javascript** extends **React.Component** {
  **render()** {
    return <h2>I love JavaScript!</h2>;
  }
}
```

现在您的 React 应用程序有了一个名为 JavaScript 的组件，它返回一个`<h2>`元素。您只需要使用方法`ReactDOM.render()`在根目录中显示它，如下所示:

```
ReactDOM.render(**<Javascript />**, document.getElementById('root'));
```

与函数组件不同，类组件有自己的状态，这是一个应该保存属性的对象。要添加这个状态对象，您需要在您的类中有一个构造函数。

构造函数是您通过包含语句`super()`来继承父组件的地方，该语句执行父组件的构造函数，并且您的组件可以访问父组件的所有函数(`React.Component`)。

这里有一个例子:

```
class Tool extends React.Component {
  **constructor()** {
    **super()**;
    **this.state = {tool: "React"};**
  }
  render() {
    return <h2>**{this.state.tool} is good.**</h2>;
    // returns: React is good.
  }
}
ReactDOM.render(**<Tool />**, document.getElementById('root'));
```

这并不是组件状态的全部。我建议您从其他资源中了解更多信息。

# 文件中的组件

React 允许您创建可在不同文件中使用的可重用组件。React 中的任何组件都可以在不同的文件中使用，你需要做的就是使用 ES6 模块来导出和导入组件。

看看下面的例子，我们从另一个文件导入了一个组件。

使用`export default`在`app.js`中导出一个汽车组件:

```
import React from 'react';
import ReactDOM from 'react-dom';

class Car extends React.Component {
  render() {
    return <h2>Hi, I am a Car!</h2>;}
}

**export default Car;**
```

使用`import`将组件从`app.js`导入到`index.js`:

```
import React from 'react';
import ReactDOM from 'react-dom';
**import Car from './App.js';**// Now you can render the component that you imported.
ReactDOM.render(**<Car />**, document.getElementById('root'));
```

# 结论

React 组件非常有用和重要，因为它们帮助您构建应用程序，将 HTML 与 JavaScript 结合起来，并轻松添加可在不同文件中执行的可重用代码。

感谢您阅读本文，希望您觉得有用。如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**

# 更多阅读

[](https://medium.com/javascript-in-plain-english/5-javascript-projects-you-should-build-as-a-front-end-developer-57318b710344) [## 作为前端开发人员应该构建的 5 个 JavaScript 项目

### 为您的投资组合提供前端 web 开发项目

medium.com](https://medium.com/javascript-in-plain-english/5-javascript-projects-you-should-build-as-a-front-end-developer-57318b710344)