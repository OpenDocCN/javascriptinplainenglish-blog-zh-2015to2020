# React 中的测试，第 2 部分:React 测试库

> 原文：<https://javascript.plainenglish.io/testing-in-react-part-2-react-testing-library-f32432b93c6c?source=collection_archive---------1----------------------->

![](img/bf40a55cdd50cd0482ac0dfb7fc1be59.png)

Photo by [Ivo Rainha](https://unsplash.com/@ivoafr?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/library?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

> 本文是 React 中测试系列的一部分:
> 
> [React 中的测试，第 1 部分:类型&工具](https://medium.com/javascript-in-plain-english/testing-in-react-part-1-types-tools-244107abf0c6)
> 
> **React 中的测试，第 2 部分:React 测试库**
> 
> [React 的测试，第 3 部分:Jest & Jest-Dom](https://medium.com/javascript-in-plain-english/testing-in-react-part-3-jest-jest-dom-7a8a03ae60b)
> 
> [React 中的测试，第 4 部分:酶](https://medium.com/javascript-in-plain-english/testing-in-react-part-4-enzyme-9b030ad616ae)
> 
> [React 中的测试，第 5 部分:使用 Cypress 的端到端测试](https://medium.com/@bryn.bennett/testing-in-react-part-5-end-to-end-testing-with-cypress-bd2bf8d3385f)
> 
> [React 中的测试，第 6 部分:React 测试库、Jest、Enzyme 和 Cypress 的真实测试](https://medium.com/javascript-in-plain-english/testing-in-react-part-6-real-world-testing-with-react-testing-library-jest-enzyme-and-cypress-9c73436d95d8)

React 测试库的标题是，“测试功能，*而不是*实现”。这个概念背后的思想是，实现是不断迭代和改进的，因此作为测试 UX 的手段是不可靠的。

例如，假设您正在开发一个小的 React 组件，在早期，您为其编写测试。该组件处理一定量的逻辑，并向 DOM 呈现一些内容。随着应用程序的增长，这个组件也在增长，最终将它分解成多个组件是有意义的。就用户而言，什么都没有改变——呈现给 DOM 的内容还是一样的。换句话说，您只改变了*实现*，而没有改变*功能。*

如果您的初始测试是为了测试实现而编写的，它们将会崩溃。但是通过测试功能——DOM 节点而不是呈现的组件——您的测试继续通过(当然，除非您搞砸了重构)。

**在我们开始**之前，有一个小提示:Jest 和 React 测试库经常一起使用，虽然我认为分别关注其中一个很有帮助，但是离开另一个来谈论其中一个几乎是不可能的。你可以认为 Jest 是在做实际的测试，而 React Testing Library 是在重新创建要测试的东西(无论是事件还是节点等等)。).这里提到的任何 Jest 功能将在我的下一篇文章中深入讨论。

# 测试功能

> 这个库提供的实用程序可以像用户一样方便地查询 DOM。通过标签文本查找表单元素(就像用户一样)，从文本中查找链接和按钮(就像用户一样)。它还公开了一种推荐的方法，通过一个`data-testid`来查找元素，作为文本内容和标签没有意义或不实用的元素的“出口”。

以上摘自官方文档，总结了这个库的核心。简单地说，反应测试库 1。呈现 React 组件和 2。搜索(或“查询”)一个 DOM 节点。查询之后，测试被踢回 Jest(或 Jest-Dom)，Jest 可以实际测试被查询节点的属性(即，它存在于 Dom 中或具有指定的长度)。

# 在测试环境中渲染

从语法上来说，第一步很简单。要测试的组件被导入到一个测试文件中，然后使用`render`函数进行渲染。真的很简单。真实世界的实现将把`render`封装在一个 Jest 测试中，我们将在 Jest 中介绍这个测试。现在，想象一下这个:

```
import App from './App';render(<App />);
```

# API 查询

第二步，在 DOM 中搜索一些东西，通过 API 查询来实现。最流行的两个查询是`getByText`和`getByRole`，但是还有更多。这些语句由变量(`getBy`)和查询(`Text`)组成。

## **可用变体:**

*   `getBy`
*   `getAllBy`
*   `queryBy`
*   `queryAllBy`
*   `findBy`
*   `findAllBy`

## **可用查询:**

*   `ByLabelText`
*   `ByPlaceholderText`
*   `ByText`
*   `ByAltText`
*   `ByTitle`
*   `ByDisplayValue`
*   `ByRole`
*   `ByTestId`

这些都是不言自明的，所以我不打算在这里一一介绍。不过，我会很快指出，由于`get`、`query`和`find`看起来都是表示同一事物的词，它们确实有不同的用法。

`get`将是您的首选，`query`通常用于断言元素的缺失，`find`可用于异步出现的元素。

此外，那些接受字符串作为参数的函数也可以接受正则表达式，这在查找部分匹配时很有用。您可以在[官方文档](https://testing-library.com/docs/dom-testing-library/api-queries)中阅读关于每个变体和问题的更多信息。

# 屏幕

这些查询在库提供的一个`screen`对象上被调用，这个对象类似于`document.body`,但是适用于您的测试环境。事实上，`screen`伴随着每个预先绑定到`document.body`的查询。

此外，`screen`有一个`debug`功能，它会将指定的 HTML 打印到您的终端，让您看到实际呈现给用户的内容。API 查询可以传递给`debug`函数，如下图:

```
screen.debug(screen.getByText('submit'))
```

# 事件

由于 React 的核心是对事件做出反应，React 测试库允许您测试事件的触发。您可以通过使用上面的功能来选择触发事件的节点(比如一个按钮)，然后使用`fireEvent`来重新创建要测试的事件。`fireEvent`提供了许多方便的方法，这样你就不必为像点击这样的基本操作创建新的自定义事件。

## 便利方法:

*   [完整列表](https://github.com/testing-library/dom-testing-library/blob/master/src/event-map.js)
*   `click`、`dblClick`、`drag`、`drop`、`mouseEnter`、`mouseLeave`
*   `copy`、`cut`、`paste`
*   `focus`、`blur`、`focusIn`、`focusOut`
*   `change`、`input`、`invalid`、`submit`、`reset`
*   `keyDown`、`keyPress`、`keyUp`
*   `select`
*   `scroll`

便利方法列表下的“完整列表”将提供便利事件的完整列表，以及包括默认`eventProperties`在内的详细信息。

`fireEvent`函数的结构如下:

```
fireEvent[eventName](node: HTMLElement, eventProperties: Object)
```

下面的例子(由官方文档提供)模拟了用户右击提交按钮(`button: 0`是左键，`button: 2`是右键)

```
const rightClick = { button: 2 } fireEvent.click(getByText('Submit'), rightClick)
```

最方便的可用的`eventProperties`方法包括`bubbling`和`cancelable`，但是有些会有特定于它的属性，比如`click`有一个`button`属性。

# 异步ˌ非同步(asynchronous)

最后，由于异步代码在任何给定时间都会极大地影响 DOM 上的内容，React 测试库提供了测试 DOM 中异步发生的变化的功能。

## 异步实用程序

*   `waitFor`
*   `waitForElementToBeRemoved`

其他不赞成使用的实用程序仍然可用，所以您可能会在现有代码中看到其他实用程序(就像简单的`wait`)，但这是当前应该使用的两个实用程序。

通常，这些函数用于等待某件事情发生以响应某个事件，简单地说明了 JavaScript 中的事情不一定是瞬间发生的。

正如我前面说过的，React 测试库与 Jest 一起使用，我们将在接下来讨论。一旦我们理解了这两者是如何工作的，我们就可以看到它们是如何组合成一个有效的测试套件的。

> **之前的**:[React 中的测试，第 1 部分:类型&工具](https://medium.com/javascript-in-plain-english/testing-in-react-part-1-types-tools-244107abf0c6)
> 
> **下一个**:[React 中的测试，第 3 部分:Jest & Jest-Dom](https://medium.com/javascript-in-plain-english/testing-in-react-part-3-jest-jest-dom-7a8a03ae60b)

## 资源

*   [正式文件](https://testing-library.com/docs/intro)
*   [反应测试库小抄](https://testing-library.com/docs/dom-testing-library/cheatsheet)
*   [什么是 React 测试库？](https://www.youtube.com/watch?v=JKOwJUM4_RM&feature=youtu.be)