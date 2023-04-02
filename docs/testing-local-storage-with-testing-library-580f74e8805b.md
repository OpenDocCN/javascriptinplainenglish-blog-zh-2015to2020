# 使用测试库测试本地存储

> 原文：<https://javascript.plainenglish.io/testing-local-storage-with-testing-library-580f74e8805b?source=collection_archive---------0----------------------->

## 如何使用测试库测试同步和异步情况下本地存储的变化。

![](img/1d1d61b61e47593858171536696e19a2.png)

Photo by [Erda Estremera](https://unsplash.com/@erdaest?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

嗨，大家好，最近我有点挣扎，试图用测试库为本地存储产生一些测试。我在网上找到的例子很少，或者对于普通读者或知识很少或没有知识的人来说有点混乱。

经过反复研究，我想出了一个我认为可以接受的解决方案。在深入研究代码之前，让我们回顾一下基础知识。

## 本地存储是什么？

本地存储是一种允许访问存储对象的属性，该存储对象允许保存在会话之间持续存在且没有到期日期的键/值对。

storage 对象公开了 get、set 和 remove Item 方法，这些方法允许从本地存储中读取、添加和移除项。

## 怎么考？

这里主要是通过嘲讽上面提到的方法。

我们可以通过使用 Object.defineProperty()来修改本地存储方法并向它们传递模拟函数。

Mocking local storage

如您所见，我们用 jest.fn()替换了 getItem 和 setItem 的实现。现在我们可以自由地访问 window.localStorage.getItem 并对其进行断言。

*你可能想知道这个可写的东西有什么用，对吗？*

writable 属性允许通过使用赋值运算符随意将值重新分配给该属性。这意味着，如果我们决定要在 defineProperty 方法之外访问 localStorage.setItem 并修改其内容，我们可以这样做。

有了这些信息，让我们在 React 组件中排队，并使用测试库测试它。

## 该组件

Local storage component

上面，您可以看到一个利用 useLocalStorage 定制钩子的功能组件。(查看 https://usehooks.com/useLocalStorage/[了解更多信息和定制挂钩)。](https://usehooks.com/useLocalStorage/)

该组件有一个接收存储在本地存储上的名称的输入和一个更新本地存储的 onChange 事件。为了增加乐趣和难度，我们添加了接收端点的 fetch 函数，并使用 axios 发出 get 请求，将响应数据保存到本地存储中。

## 测试

我们将涵盖以下测试案例:

1.  在呈现时，应该调用本地存储 getItem。
2.  在输入文本改变时，我们应该用新的文本调用本地存储 setItem。
3.  单击按钮时，我们应该向端点发出 get 请求，并将响应存储在本地存储上。

首先，让我们模拟我们的依赖和设置或测试

Mocking dependencies and setting tests up

正如你在上面的要点中看到的，我们模仿 axios get 方法返回一个承诺，并利用 jest beforeEach 函数在每个测试用例之前重新定义我们的本地存储模拟。

完成设置后，测试应该很简单。

## 在渲染时，应该调用本地存储 getItem。

Assert getItem call

这个测试用例是三个中最简单的。我们所要做的就是呈现我们的组件，并断言我们模拟的 getItem 被调用。

## 在输入文本改变时，我们应该用新的文本调用本地存储 setItem

Assert setItem is called on input change

这个测试用例需要与我们的组件进行交互，以便能够验证是否调用了 setItem。我们通过呈现组件并提取其 queryByPlaceholderText 方法来实现这一点，并利用它来访问我们的输入。有了输入组件，我们现在可以使用测试库 fireEvent 来更改它的值。

剩下的就简单了，我们断言我们的 setItem 被调用了一次，然后，我们检查这个调用是否使用了预期的键/值对。

## 单击按钮时，我们应该向端点发出 get 请求，并将响应存储在本地存储上。

这个测试更棘手，因为它处理异步内容，幸运的是我们已经模拟了 axios get 方法总是返回给定的字符串。

Assert network request is made and setItem is called

我们需要做的第一件事是启动我们的异步请求。这一部分可以通过让我们的按钮 a 触发一个点击事件来完成。现在，我们不希望在确认异步请求已经结束之前就完成断言。既然我们知道完成请求后，我们的输入值会发生变化，那么我们可以使用 waitForElement 和 getByDisplayValue 来等待被嘲讽的值出现。

之后，我们可以成功地做我们的断言，并且一切正常。

现在你可以检查我们完整的测试文件

Complete local storage test

你也可以在 Codesandbox 上使用它

Play around on Codesandbox

## 结论

正如你可以看到的测试，本地存储可以毫不费力，或者它可以是一个耳朵。这完全取决于你如何对待它。

我希望这篇文章有助于最终消除您对本地存储测试以及为此使用测试库的疑虑。

如果你有任何问题，你可以在推特上找到我，地址是@danieljcafonso

我希望你喜欢并关注下一期指南。