# 编写更好的 React 代码的 5 个最佳实践

> 原文：<https://javascript.plainenglish.io/5-best-practices-for-writing-better-react-code-f6120d94c350?source=collection_archive---------1----------------------->

## 每个 React 开发人员都应该知道的 5 个最佳实践

![](img/abed881754cb76637f3b31b8ed6d0454.png)

Photo by [Caspar Camille Rubin](https://unsplash.com/@casparrubin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个著名的用于构建用户界面的 JavaScript 库。如果你开始用 React 开发你的前端，这篇文章会对你有帮助。我将讨论 5 个最佳实践来保持你的代码组织良好。

## **1。使用功能组件**

我们知道，在 React 中有两种类型的组件，即类组件和功能组件。类组件是全状态的，具有内部状态和生命周期方法。功能组件是无状态的。但是使用 [*反应钩子*](https://medium.com/dev-genius/introduction-to-react-hooks-e49738432f54) *，*它可以充当状态满的组件。功能组件接受 props 作为参数并返回一个 React 元素。功能组件的优点是代码少，比基于类的函数简单，无状态，没有“this”绑定，易于理解。

```
import React from ‘react’;export default function App() {
 return(
  <h2>Hello world</h2>
 );
}
```

## **2。使用小型组件并分离您的功能**

React 应用程序是用一组组件构建的。当你编写小组件时，你可以很容易地阅读和理解这个组件的功能(活动)是什么。代码的可重用性也增加了。当你写一个代码在一个组件中完成几个功能(活动)时，这将很难理解。

例如，App 组件包含书籍和文件两个列表。下面的代码打印整本书列表和文件列表。

Figure 1\. (App.js — line no 10–14 and 16–20 are duplicated)

我们可以使用`*ItemsList*` 组件来显示图书列表和文件。因此，代码的可重用性增加了，并且很明显功能(活动)被分成了两个部分。

Figure 2\. (App.js — ItemsList component is reused. )

Figure 3\. (ItemsList.js)

## **3。使用 JavaScript 析构删除冗余**

*析构赋值语法是一个 JavaScript 表达式，它可以将数组中的值或对象中的属性解包到不同的变量中。* [*来源*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

我们将参数作为道具从父组件传递给子组件。例如，在下面的代码中，父组件(App)将其状态值作为属性发送给子组件(Sum)。在这里，`*props.values*` 四次都是多余的。

Figure 4\. (App.js)

我们可以使用 JavaScript 析构来删除`*props.values.*b`的冗余，方法是:

`**const {value1, value2, value3, value4} = props.values;**`

```
function **Sum**(props){ 
 const {**value1**, **value2**, **value3**, **value4**} = **props.values**;
 return <h2>sum: {**value1** + **value2** + **value3** + **value4**}</h2>;
}
```

## **4。使用 prop-types 库(如果您没有使用 TypeScript/Flow)**

prop-types 是一个用于检查道具类型的库。它验证道具的类型。它有助于防止在将错误的数据类型属性传递给组件时出现错误。

Figure 5\. (DisplayName.js)

## **5。使用 React 开发者工具**

当你开发一个 React 项目时，React 开发者工具非常有用。它查看组件层次，组件的状态，道具和孩子，它也有助于调试。

**参考**

[](http://reactjs.org/docs) [## 开始行动-做出反应

### 用于构建用户界面的 JavaScript 库

reactjs.org](http://reactjs.org/docs)