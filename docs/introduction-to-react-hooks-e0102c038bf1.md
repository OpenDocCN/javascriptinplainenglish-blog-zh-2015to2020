# React 挂钩简介

> 原文：<https://javascript.plainenglish.io/introduction-to-react-hooks-e0102c038bf1?source=collection_archive---------0----------------------->

## 如何在 React 中使用状态挂钩

![](img/982739a7a2450d5e9b5e3c77190d0541.png)

Photo by [Fabian Grohs](https://unsplash.com/photos/dC6Pb2JdAqs) on [Unsplash](https://unsplash.com/photos/dC6Pb2JdAqs)

[**techno funnel**](https://www.youtube.com/channel/UCo-h1M-5M6Y5D4Lgut8ge4w)**带来了另一篇关于 **React 钩子**的文章，这是 React 在 16.8.0 版本中的最新加入，许多 React 开发者对这一加入感到兴奋。在这篇文章中，我们将讨论 **React 状态钩子**，它使用户能够创建没有类的状态变量。React 中有多个钩子可用，但本文主要关注`useState`。**

**![](img/afb1ba469fc923978f6afe9bb4b86d20.png)**

**在发布**钩子**之前，根据组件是基于类还是基于函数，React 组件被分为两大类。**

**类组件能够定义状态属性和生命周期方法，但是功能组件不能使用状态或访问 React 生命周期。两个组件都将`props`作为输入参数，其中包含从父组件传播的数据。由于功能组件不创建自己的状态属性，它们依赖于父组件的输入数据，这使它们成为“无状态组件”**

1.  **有状态组件(使用类创建)。**
2.  **无状态组件(使用函数创建)。**

**有关挂钩的快速教程，请参考以下视频教程:**

**[https://www.youtube.com/watch?v=383oe-B-QGA](https://www.youtube.com/watch?v=383oe-B-QGA)**

# **有状态组件**

**让我们来看一个使用类的**有状态组件**的简单实现。在下面给出的例子中，我们正在创建一个简单的组件，并使用`this.state`为其添加一些状态属性。**

**该组件定义了以下状态属性(“姓名”、“年龄”和“职务”)，这些细节作为组件 UI 的一部分呈现。代码还为我们提供了使用`this.setState`更新状态变量的能力。状态更新可用于整个组件和子组件。传递给子组件的状态被接收为`props`。**

**上面的代码创建了一个包含三个状态变量的有状态组件:`name`、`age`和`designation`。它还包含一个将 state 属性递增 1 的函数。由于状态变量已经被修改，显示`age`属性的组件将在组件中更新，并触发 UI 的相应变化。**

**[](https://reactjs.org/docs/state-and-lifecycle.html) [## 状态和生命周期-反应

### 本页介绍了 React 组件中状态和生命周期的概念。你可以找到详细的组件 API…

reactjs.org](https://reactjs.org/docs/state-and-lifecycle.html)** 

# **使用无状态组件**

**创建组件的另一种方法是将其创建为无状态组件。这些简单的组件将`props`作为它们的输入参数，并像我们之前所做的那样在 UI 中显示它们。无状态组件不能定义自己的状态变量，也不能更新收到的任何 props 值。任何更新属性的尝试都将导致错误。下面是一个简单的无状态组件的例子。**

# **使用 React 挂钩**

## ***使用函数创建有状态组件***

**既然我们已经看到了在 React 的最新版本之前组件是如何创建的，那么让我们看看如何使用状态钩子来创建它们。**

**随着**钩子**的引入，我们可以在不使用类的情况下创建有状态组件。我们可以使用函数来创建有状态的组件。因为我们在函数内部定义状态，所以我们将这些组件称为“**有状态的** **函数组件。**“我们可以使用`useState`钩子来管理功能组件内部的状态属性。**

**让我们看看使用钩子的相同代码是什么样子的。**

**上面的代码使用钩子来创建一个有状态的组件。我们使用关键字`useState`来定义组件的状态。`useState`将一个参数作为输入，定义状态变量的初始值。**

**在调用带参数的`useState`函数时，它执行以下操作:**

1.  **创建一个名为“name”的新状态变量**
2.  **将默认值指定为参数中传递的值。**
3.  **返回新创建的状态属性以及“setter”函数。**

**现在，让我们分析下面这段代码来理解`useState`钩子。**

**使用一个默认参数调用`useState`函数，该参数是一个名为“Mayank”的字符串。该函数创建一个新的状态属性，并为其分配一个默认值。此函数返回包含以下元素的数组:**

1.  **第一个元素是新创建的“state”值。**
2.  **第二个元素是同一属性的“setter 函数”。**

**然后我们使用析构并将第一个索引处的元素赋给变量`name`，setter 被赋给变量`setName`。现在，这个 name 属性充当状态变量(`this.state.name`)，而`setName`是属性设置器——它更新类似于我们在类组件(`this.setState`)中使用的值。用更新后的值作为参数调用`setName`会将该值更新为状态属性`name`。**

**上面的代码相当于:**

**使用有状态功能组件的一个优点是，您不再需要以下面的方式访问状态属性:`{this.state.name}` **。**为了访问功能组件中的状态属性，我们只需要引用`{name}`状态变量**。****

**功能组件提供了一个巨大的优势，因为我们可以摆脱复杂的类逻辑，并且我们也不需要担心在组件的任何地方添加`this`关键字。功能组件基于闭包的概念。**

**此外，它使应用程序保持一致。您不需要使用函数定义一半的组件，而将另一半的组件定义为类。使用钩子，所有的组件都可以用“函数”的形式来表示因此，这是向函数式编程和提供跨应用程序一致性迈出的一大步。**

# **使用钩子的更多好处**

## **1.类是复杂的**

**班级很难处理和管理。类通常没有被很好地缩小，并且它们也使得热重载难以处理。钩子包含了函数式编程并提供了简单性。我们不需要拥有类型为`class`的组件和类型为`function`的其他组件。**

**它提供了整个反应组件的一致性。**

## **2.随着时间的推移，类组件变得难以理解**

**随着应用程序的增长，我们向类组件中添加了大量代码，这导致了复杂性。这使得分解成相关的功能变得困难。**

**因此**类组件的规模和复杂性不断增长**。**

**有关这方面的更多详细信息，请参考 React 的官方网站:**

**[](https://reactjs.org/docs/hooks-intro.html) [## 介绍钩子-反应

### 钩子是 React 16.8 中的新增功能。它们允许您使用状态和其他 React 特性，而无需编写类。这个…

reactjs.org](https://reactjs.org/docs/hooks-intro.html) 

React’s “useState” Hooks

**作者其他文章:**

[](https://levelup.gitconnected.com/introduction-to-reacts-higher-order-components-hocs-c42182fb634) [## React 高阶组件(hoc)介绍

### 简单地说，高阶函数是要么接受一个函数作为参数，要么返回一个新的…

levelup.gitconnected.com](https://levelup.gitconnected.com/introduction-to-reacts-higher-order-components-hocs-c42182fb634) [](https://levelup.gitconnected.com/introduction-to-rxjs-3f17e1009527) [## RxJS 和反应式编程简介

### 反应式编程是 JavaScript 世界的新热点。在本文中，我们将讨论基础知识…

levelup.gitconnected.com](https://levelup.gitconnected.com/introduction-to-rxjs-3f17e1009527) [](https://levelup.gitconnected.com/creating-custom-observable-with-rxjs-379692f08f76) [## RxJS 自定义可观察对象介绍

### 在我之前的文章中，我们谈到了 RxJS 的基础知识。本库给出了许多创建可观察对象的方法。在这个…

levelup.gitconnected.com](https://levelup.gitconnected.com/creating-custom-observable-with-rxjs-379692f08f76) 

*更多内容尽在*[*plain English . io*](http://plainenglish.io/)**