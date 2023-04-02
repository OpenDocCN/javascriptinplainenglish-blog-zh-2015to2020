# VueJS 动态组件概述—您需要了解的内容

> 原文：<https://javascript.plainenglish.io/an-overview-of-vuejs-dynamic-components-what-you-need-to-know-37b59c0ed7f4?source=collection_archive---------4----------------------->

![](img/d0d6b2daf1b7e30ad913bd5605a4cb06.png)

VueJS 动态组件可以非常方便地让您的代码更具可读性和适应性。他们可以将几个条件组件(使用 v-if、v-else-if、v-else 切换的组件)简化为一行代码。

简而言之，它们允许您以最简单的方式在不同的组件之间切换。

在本文结束时，您将…

*   了解什么是动态组件
*   认识到何时使用它们
*   知道如何在 VueJS 中实现它们
*   了解高级动态组件主题

好了，介绍够了。让我们开始吧。

# 什么是动态组件

动态组件是在运行时在组件之间动态切换的方式。虽然这是一个循环的定义，但这个名字是不言自明的。

为了更好地理解，让我们看一些示例用例。

*   导航[选项卡式组件](https://learnvue.co/2019/12/building-reusable-components-in-vuejs-tabs/)而不必路由到新页面
*   在一个组件元素中管理不同类型的弹出窗口
*   根据用户是否登录显示不同的内容。

所有这些都需要独特的组件，但是动态组件提供了一种快捷方式，而不是一个接一个地包含所有的组件。

使用动态组件有几个好处。

从开发人员的角度来看，它使您的代码更具可重用性。如果你所要做的就是使用一个元素，而不是为所有事情都添加一个 v-if，那么它的可伸缩性和可读性更好。

从用户端来看，使用动态组件可以使页面更具交互性，并节省页面负载，从而带来更快的用户体验。

> ***这是双赢。开发更容易，您的用户也会受益。***

现在我们知道了什么是动态组件，以及为什么它们有用，让我们学习如何使用它们。

# 太好了。现在我们如何使用它们？

使用动态组件非常简单。我们只需要知道两件事:

## 1.组成元素

在 Vue 中，component 元素允许我们声明动态组件。它的用法和其他元素一样，我们可以在类似`<component></component>`的模板中使用它

当我们开始使用组件元素的一个独特属性时，奇迹就发生了。

## 2.v-bind:is 属性

这是实际上允许动态组件工作的属性。本质上，它告诉组件元素它是什么。v-bind:is 或 just :is 是简写，指向一个注册的组件。

您可以向它传递两种类型的参数:

1.  组件的名称
2.  组件的选项对象

## 那么我们如何利用这一点呢？

简单。

首先，我们必须使用 import 语句包含我们想要使用的组件。我们还必须在 options 方法中将它们声明为 VueJS 属性的组件。

```
<script> import ComponentA from '@/components/A.vue' import ComponentA from '@/components/A.vue' export default { components: { ComponentA, ComponentB } }</script>
```

接下来，我们必须声明一个对应于当前组件的变量。它看起来会像这样。

```
data () { return { comp: ComponentA // or can be 'ComponentA' }}
```

最后，让我们将组件元素添加到模板中。更糟糕的是。我添加了在两个组件之间切换的小按钮。代码如下所示。

```
<template> <div> <button @click='comp = ComponentA'> Component A </button> <button @click='comp = ComponentB'> Component A </button> <component :is='comp' /> </div></template>
```

# 走远一点

您可以做更多的事情来让动态组件变得令人惊叹。我个人最喜欢的是添加 Vue 转换以使它们无缝工作，或者实现 Vue 保持活动组件以帮助保存状态。

但是，即使在最简单的情况下，VueJS 动态组件也可以使您的代码更具可维护性和适应性。

希望这篇文章有所帮助。如果您有任何关于 VueJS 的问题，请联系 matt@learnvue.co

如果你有兴趣了解更多关于 Vue 3 的知识，请下载我的免费 Vue 3 备忘单，里面有一些基本知识，比如组合 API、Vue 3 模板语法和事件处理。

*原载于 2020 年 1 月 4 日 https://learnvue.co**的* [*。*](https://learnvue.co/2020/01/an-overview-of-vuejs-dynamic-components/)