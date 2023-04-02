# React 中状态和道具有什么区别？

> 原文：<https://javascript.plainenglish.io/what-are-the-differences-between-state-and-props-in-react-74f21e39b172?source=collection_archive---------9----------------------->

## 你如何使用道具和状态，它们分别是什么？

![](img/a0f7e2e4450e8c3d5658af5a4208aea3.png)

Photo by [Elena Mozhvilo](https://unsplash.com/@miracleday?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

react 最令人难以置信的一点是它能够管理您的数据并充分地重新呈现您的应用程序。当你的数据发生变化，但有两种主要的反应方式来思考数据，有道具和它们的状态。理解什么时候应该使用道具，什么时候应该使用状态可能会有点混乱。

我将详细分析什么是道具，什么是状态，以及为什么你应该在你的应用程序中使用道具和状态。

# 反应道具

所以首先我们要从道具开始。你可以把道具想象成一个函数的参数，当你在 react 里面创建一个组件，你想要渲染它，你要把它传递给你想要赋予它的道具。

例如，假设我们有一个计数器应用程序，你最有可能传递给这个计数器的是初始计数，也就是你的计数应该开始的地方。所以，你要在道具里给你的计数器组件一个初始计数，我们用道具的原因是因为道具有点像你传递的东西。

这就像一个函数，你想把你的组件初始化成什么或者你想让你的组件呈现什么？例如，在这个计数器应用程序示例中，我们希望初始计数为零，因此，我们将在 props 中传递它。

## 举个例子，

你能想到的其他需要道具的地方是，假设你想向用户显示一个有标题和副标题的东西。你也要把它储存在道具里，因为你想要什么？你的功能或者你要带的组件将会是道具。

在我们的例子中，组件存储标题和副标题。因此，我们通过道具来传递它们，然后我们的应用程序知道如果这些道具在某个时候发生了变化，例如，我们的应用程序中的其他东西发生了变化，这些道具会为我们重新呈现该组件，因为我们的道具现在不同了。

# 反应状态

另一方面，相当多的不同状态是组件内部的某个东西。道具和状态的最大区别在于，你传递给组件的道具和状态是在组件内部处理的。在我们的计数器应用程序的例子中，道具是在组件外部处理的。更新计数在状态内处理。因此，当我们在 props 中传递初始计数时，我们只是将状态设置为初始计数。然后在我们处理计数器的组件中。

我们设法更新我们的计数器，根据用户的操作来增加或减少它，我们为此使用了 state。

状态和道具差别很大。状态在组件中处理，您可以在组件内部更新它。相比之下，props 在组件之外处理，也必须在组件之外更新。

## 主要差异

关于状态和道具的另一件事是，当你改变应用程序中的状态时，它会重新呈现应用程序的那个部分。但是道具你不能改变它们，你需要改变外面的组件。

就像我们讨论过的，它很可能会被存储在你的应用程序中的其他地方，作为道具传递下去。

回到我们的副标题和组件示例，很可能这个组件根本没有任何状态，因为它所做的只是呈现一些文本，而这些文本永远不会改变。

该组件中没有任何东西可以改变，因此它不需要任何状态，它只是将这两个道具显示出来，这就是该组件所做的一切。

这很好，非常简单。不过，对于计数器应用程序示例，我们正在更新计数，因此我们需要状态来存储我们将要更新的内容，这正是状态派上用场的地方，当您需要重新呈现和更新应用程序时，状态就在那里。基于用户所做的事情，如果你想改变你的应用程序中的一些东西，你需要把它存储在一个状态中，这样它就能正确地重新呈现一个改变了的状态。

另一方面，当您想要显示组件内部的一些信息而不需要对其进行硬编码时，Props 非常有用。本质上它是函数的变量。你也可以这样想，当你创建一个带有构造函数的类时，你传递给这个类的构造函数的东西，将会是你在 react 中的一个组件的道具。

## 举个例子，

例如，我们将把标题和描述传递下去，因为我们想要呈现它们，而不只是想要一个呈现相同标题和描述的组件。我们希望标题和描述有所不同，所以我们将使用道具来制作动态效果。根据我们传递给那个组件的内容，但是对于我们的计数器应用程序，我们有一些计数需要在我们的状态中更新，所以我们将使用状态来确保我们可以根据用户输入不断地更新它。

国家的另一个例子是有益的。

也就是说，在一个表单中，如果你有一个类似 checkbox 的输入元素，选择一个需要用户更新的框。因此，我们将使用状态来存储他们正在将该值更新为什么，以及他们正在将该值更改为什么，等等。

许多人感到困惑，不知道何时应该使用状态 vs 道具的地方是他们没有正确地考虑哪个将由组件来处理。

# 要点

这些是国家和道具之间的主要区别。

1.如果你处理的是组件内部的信息，而不是组件外部的信息，比如一个父母，那么你就要使用状态。

2.例如，如果您像处理父母一样在组件之外处理信息，那么您需要通过 props 传递它。

3.此外，如果您的信息是静态的，不会改变，例如，在组件内部，您永远不需要更新显示部分的标题。然后你想用道具，因为道具是给那些要从父代传下来的东西用的，不会在组件内部发生变化。

但是对于计数器，正如我提到的，我们在组件中更新它，所以我们需要使用状态，这是状态和道具之间唯一的区别。

希望你能从中学到一些新的东西。你也可以看看我的其他文章。

[](https://medium.com/better-programming/why-i-find-javascripts-destructuring-so-useful-7be41d9ba609) [## 为什么我觉得 JavaScript 的析构如此有用

### 数组和对象析构

medium.com](https://medium.com/better-programming/why-i-find-javascripts-destructuring-so-useful-7be41d9ba609) [](https://medium.com/javascript-in-plain-english/should-i-use-map-or-foreach-in-javascript-af9da3b4adc3) [## JavaScript 中应该用 map()还是 forEach()。

### 了解何时使用哪种类型的数组方法

medium.com](https://medium.com/javascript-in-plain-english/should-i-use-map-or-foreach-in-javascript-af9da3b4adc3) 

祝你愉快😉