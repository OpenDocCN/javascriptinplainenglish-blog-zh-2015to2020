# 反应组分中的错误处理

> 原文：<https://javascript.plainenglish.io/capturing-handling-errors-in-react-component-6bc48d533dd5?source=collection_archive---------6----------------------->

## 如何捕获错误并在反应中呈现回退组件

![](img/c3b3ef64f1546d95851eea93955b26ef.png)

componentDidCatch and getDerivedStateFromError in React

techno 漏斗展示了另一篇文章，我们将在其中讨论从组件捕获错误并呈现回退组件(ErrorComponent)的机制，以防组件在呈现时导致某些错误。

下面的文章将讨论以下生命周期事件:

1.  使用“**componentdcatch**”
2.  使用“**GetDeriveStateFromError**”生命周期事件。

# 了解用例场景…

componentDidMount in React, getDerivedStateFromError

1.  创建一个包含输入框的组件“雇员尾巴”
2.  一旦用户在输入框中键入空格，组件应返回一个错误，表示空格不被接受。
3.  部署一种机制，可以跟踪返回的错误
4.  遇到错误时，组件应该呈现一些“错误组件”而不是“雇员详细信息”组件。

## 开始使用员工详细信息组件

让我们首先创建一个“EmployeeDetails”组件，一旦输入框包含空格，它就会抛出一个错误。

在上面的组件中，每当我们在输入框中进行更改时，都会调用“updateName”函数。它检查文本框的当前更新值是否包含空格。如果有空格，组件将返回一个错误。由于错误不是句柄，应用程序将崩溃，并出现以下错误“名称不能包含空格”

## 添加使用“组件捕获”跟踪错误的机制

React componentDidMount, getDerivedStateFromError

为了捕捉错误，我们引入了术语“错误边界”。我们创建了一个组件，它作为一个错误边界，捕获所有子组件将调用的所有错误。让我们假设我们可以有一个名为“雇员尾巴”的组件，它可以根据某个条件抛出一个错误。为了捕获在这个组件内部引起的错误，我们需要创建一个父组件来保存这个容易出错的子组件。

现在让我们创建一个可以表示为“**错误边界**的组件

[https://gist.github.com/Mayankgupta688/2f6b1e18af3c30abc8070753e1305ffe](https://gist.github.com/Mayankgupta688/2f6b1e18af3c30abc8070753e1305ffe)

创建的“错误边界”组件现在可以呈现一些子组件。该类实现了方法“componentDidCatch ”,一旦任何子组件返回一些错误，就会调用该方法。由于在子组件内部引起的所有错误都将在这个方法中被捕获，所以组件可以被看作是一个外部组件，它不会让错误泄漏到它的边界之外。

## 回退到另一个组件“错误组件”

Error Handling in React

如果组件返回错误，我们希望组件显示“错误组件”。该组件包含一个标题，表示在使用“雇员尾巴”组件时发生了什么错误。为了在错误期间部署这种回退机制，我们可以实现**“getderivestateformerror”**生命周期事件

**理解“getDerivedStateFromError”生命周期**

一旦接收到错误，可以使用该生命周期事件来导出新的状态。一旦收到错误，将执行以下生命周期事件，它可以更新上面显示的“ErrorBoundries”组件的状态。我们可以设置“ErrorBoundries”的状态，这样它就可以表示应用程序中的错误情况。一旦状态被更新,“ErrorBoundries”组件将重新呈现，我们可以根据组件状态部署我们的“ErrorComponent”。

让我们扩展上面的 **ErrorBoundries** 组件来实现这个…

[https://gist.github.com/Mayankgupta688/ed4e7865be5fd26dd7f68296c22e8d57](https://gist.github.com/Mayankgupta688/ed4e7865be5fd26dd7f68296c22e8d57)

在上面的组件中，我们可以看到我们创建了一个状态变量“hasError”。此变量可用于跟踪子组件内部的错误。“hasError”的初始值被标记为假，因为我们在初始阶段没有任何错误。一旦在“EmployeeDetails”组件上遇到错误，就会调用“getDerivedStateFromError”函数，并将“hasError”的值设置为“true”。

因为组件正在设置新状态，所以组件将重新呈现。在 render 函数中，我们指定了表示“EmployeeDetails”或“ErrorComponent”是否可见的条件。由于“hasError”的值为 true，因此将呈现“ErrorComponent”。在这种情况下，由于错误被处理，应用程序不会中断。要访问代码，请访问以下 URL:

[https://codesandbox.io/s/error-boundries-react-kghhd](https://codesandbox.io/s/error-boundries-react-kghhd)

*   *代码需要在生产模式下运行，以显示所需的行为。上面的代码只是一个参考，它不在生产模式下运行。

*更多内容尽在*[***plain English . io***](http://plainenglish.io/)