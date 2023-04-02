# React 中的继承是如何工作的？

> 原文：<https://javascript.plainenglish.io/inheritance-in-react-1e301ef3a618?source=collection_archive---------7----------------------->

![](img/130485653fd08bf05c27c56c314f8a08.png)

Photo by [Esther Jiao](https://unsplash.com/@estherrj?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

如果没有继承，现代代码在很大程度上是站不住脚的，这“[允许程序员创建基于现有类](https://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)#:~:text=Inheritance%20allows%20programmers%20to%20create,via%20public%20classes%20and%20interfaces.)的类”。它是面向对象编程的基石。尽管 React 在许多方面利用了传统面向对象的实践，但 JSX 及其整体结构与传统的合成有所不同。例如，我们不会通过调用传统的构造函数来创建一个类的新实例:

```
new Component();
```

而是创建类的 JSX 表示，作为 React 组件:

```
<Component />
```

# 问题是

那些通过继承解决的问题仍然存在于 React 应用中，但由于我们编写 React 代码的方式不同，变得更加棘手。例如，我最近正在构建一个容器，它可以呈现几种不同图表中的任何一种。这些图表之间有许多共同的属性，但也有一些重要的差异。

看起来像是继承的经典案例，对吧？我们创建了一个`Chart`类，其中的`ChartA`、`ChartB`和`ChartC`都可以继承。

沿着这条路走下去之后，才知道*就是不行*。从我们调用函数、处理道具和渲染 JSX 的方式，所有这些都是我们代码中众所周知的难题。你会接近，但不完全是*那里*——至少，不会以任何方式在反应中感到自然。

![](img/d72118d1ab2e525be72242714d82c6c5.png)

Photo by [Karla Hernandez](https://unsplash.com/@karlahrnndz?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/problem?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 解决方案

React 文档建议单独使用合成来代替继承。

> React 有一个强大的组合模型，我们建议使用组合而不是继承来重用组件之间的代码。

这些文档特别强调了“专门化”——当一个组件是通用组件的“特例”时。这感觉就像我们上面看到的图表问题。尽管共享的图表属性只会作为`ChartA`、`ChartB`或`ChartC`的一部分呈现，而不仅仅是`Chart`，这三个图表在某种程度上是`Chart`的“特例”。

## 不要做什么

我的第一反应是构建一个父组件，或者甚至是容器，将所有共享属性传递给专用图表。这很自然地感觉像是继承的表现——本质上是通过`props`继承类属性。然而，在这条道路上出现了几个问题。

首先，事实是共享属性比唯一属性多得多，一些唯一属性嵌套在共享属性中。这使得凌乱的物体蔓延和大，笨拙的道具。

第二，存在范围的问题——主要是，感觉它们应该在`Charts`中被一般性地定义，但是依赖于在子元素中定义的那些独特的属性，因此必须限定在最里面的子元素的范围内。

第三，有一层逻辑确实是完全通用的，但由于组成和结构，最终在`ChartA`、`ChartB`和`ChartC`中被重复。这违反了 D.R.Y，一个明确的危险信号，表明我们在做错事。

## 正确的东西

那么，我们做什么呢？我们把上面的拿过来，然后翻转过来。React 文档建议构建您的组件，以便“一个更‘特定’的组件呈现一个更‘一般’的组件，并用 props 对其进行配置”。

这感觉是违反直觉的，就像通用是从“特殊”继承的一样。`ChartA`、`ChartB`和`ChartC`都呈现一个共享的`Chart`组件。但是(没有人感到惊讶)，React 的团队对这种结构完全正确，这种组合可以满足任何继承和专门化的需要。

继承的核心是共享属性和实现的概念——换句话说，一个类的属性在子类中被“重用”。在 React 中，我们还创建组件，这些组件的属性和实现可以在整个代码库中重用和共享。我们把这些可重用的组件放在哪里呢？在其他容器或组件的内部，这些容器或组件通常为重用组件提供关于其行为方式的上下文。例如，一个按钮从一个`Login`容器中接收它的`label`和`onClick`。

在我们的图表示例中，`ChartA`、`ChartB`和`ChartC`都共享`Chart`的属性，即它们“继承”(共享属性)的组件/类，方法是将`Chart`作为子组件，并将唯一的(子类)属性作为`props`传入。

您可以在 React [文档](https://reactjs.org/docs/composition-vs-inheritance.html)中阅读更多关于组合与继承的内容。