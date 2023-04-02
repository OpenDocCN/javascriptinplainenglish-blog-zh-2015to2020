# 高级 Vuejs 组件模式

> 原文：<https://javascript.plainenglish.io/advanced-vuejs-component-patterns-3ab1e1f0ed07?source=collection_archive---------3----------------------->

## 可扩展 Vuejs 应用程序的 5 种模式

组件模式是定义组件的职责和行为的方法和最佳实践。遵循这些模式将提高应用程序的可维护性、性能和可伸缩性。

在这一部分中，我们将介绍在实现新组件时添加到工具箱中的 5 种模式。

*   功能组件
*   动态组件
*   异步组件
*   无渲染组件
*   高阶组件

![](img/46f09d2f43b3cefe243a98fb72a8b63e.png)

Photo by [Fran .](https://unsplash.com/@fran_?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 功能组件

与您的设想不同，这些组件并没有做太多的事情。它们由渲染函数组成，没有状态(无状态)，没有生命周期挂钩，甚至没有 Vue 实例，因此它们便宜而快速。

## 何时使用它们

当您想要提升应用程序的性能时，它们是一个不错的选择。功能组件往往尺寸更小，因此增加了可测试性和可重用性。

它们通常用于编写表示性的(*哑的*)组件，比如按钮、图像、链接和头像。

## 如何实现它们

通过向组件添加`functional`属性，可以很容易地实现功能组件，这可以通过使用模板或渲染函数来完成。

## 模板

类似于常规的 vue 组件，我们可以使用带有`functional`属性的`template`标签。

由于是无状态的，API 是不同的，例如，你必须使用`{{props.msg}}`而不是`{{msg}}`。

## 渲染功能

当使用渲染函数时，你需要做的就是定义`true`的函数属性如下

# 动态组件

动态组件允许你利用`<component :is="attribute">`基于`attribute`安装和卸载组件

这意味着你不再有几个`v-if`和一个杂乱的模板，而是获得了开箱即用的相同功能。此外，您还可以在交换组件时保持您的状态。

## 何时使用它们

到目前为止，选项卡是最流行的用例，因为对于每个选项卡，您都希望根据所选的选项卡交换几个组件。

网站构建者通常非常依赖这种模式，因为最终客户(用户)是决定在运行时呈现哪些组件的人。

## 如何实施它们

实现动态组件非常简单。使用内置的`<component :is=”componentName”>`，它允许我们基于`componentName`的评估动态地呈现一个组件。

此外，如果我们想在不同组件之间切换时保持状态，那么我们可以使用`<keep-alive>`来保持这个组件状态不消失。

# 异步组件

异步组件是异步加载的组件。它们通过减小初始包的大小来优化加载时间，从而有助于提高应用程序的性能。

一旦您的应用程序增长，您将认识到您不必立即将所有组件交付给您的用户，因为有些用户可能不使用某些组件。

通过使用 Webpack 和 ES6，您可以轻松优化向客户交付哪些组件。

## 何时使用它们

异步组件通常用于路由/页面组件，从而推迟这些组件的加载，直到客户请求它们。

在更大的组件中，涉及复杂的状态操作来切换一些组件(*思考表单向导*)延迟这些组件的加载也有助于减少初始包的大小。

## 如何实施它们

您所需要做的就是返回一个与您的组件一起解析的`promise`。使用 Webpack 和 ES6，我们可以只在安装了`my-async-component`的用户时才加载它。

# 高阶组件

高阶组件是将一个组件作为参数，并在添加一些功能后返回另一个组件的组件。

通过在几个组件之间共享逻辑，HOC 允许您减少代码重复。

尽管这种模式不像 react 社区中那样常见，因为存在作用域槽和混合。可以编写高阶元件，如 [vuex-connect](https://github.com/ktsn/vuex-connect) 。

## 何时使用它们

我个人还没有使用过它们，但是我可以看到回退加载组件是 HOC 有用的一个用例。

## 如何实现它们

以下是由[Bogna“bog nix”knychaa](https://medium.com/u/507e445516c?source=post_page-----3ab1e1f0ed07--------------------------------)撰写的关于 HOC 的深度指南

[](https://medium.com/bethink-pl/higher-order-components-in-vue-js-a79951ac9176) [## Vue.js 中的高阶组件

### 如 React 文档中所述，高阶组件是一个函数，它将组件和…

medium.com](https://medium.com/bethink-pl/higher-order-components-in-vue-js-a79951ac9176) [](https://github.com/jackmellis/vue-hoc/blob/master/packages/vue-hoc/README.md) [## jackmellis/vue-hoc

### 受 https://github.com/vuejs/vue/issues/6201 姐妹项目的启发，创建更高阶的 Vue 组件:vue-compose…

github.com](https://github.com/jackmellis/vue-hoc/blob/master/packages/vue-hoc/README.md) 

# 无渲染组件

这些组件本身不呈现任何内容。它们的存在是为了封装特定的功能并允许消费者提供不同的模板。

无渲染组件通过向上提供数据/接口，严重依赖于作用域插槽。

## 何时使用它们

假设我们想要实现一个可切换的按钮，但同时我们不想把它限制在一个模板上。这使得切换组件成为无渲染的良好候选。在这个意义上，我们将公开状态，并使用作用域槽向上公开方法。

## 如何实施它们

首先，我们向消费组件公开状态`on`和一些方法，如`setOff`和`setOn`。

稍后，我们用模板填充该槽，并在无渲染组件中重用预定义的状态和方法

# 摘要

我们已经介绍了一些流行的组件模式、它们的用法和实现。关于组件模式的推理将帮助您更好地设计您的应用程序，从而从长远来看节省您的时间。

这些模式并不相互排斥，您经常可以将多个模式混合在一起，这样您就可以拥有一个异步功能组件或一个无渲染的动态组件。

这些是我问自己的一些有帮助的问题，以决定使用哪种组件模式

*   这个组件是有状态的吗？它需要有状态吗？
*   您认为这个组件的“逻辑”有可能被重用吗？
*   您需要在初始加载时加载该组件吗？
*   我的模板上有多个`v-if`吗？

感谢阅读，我错过了你使用的一些模式吗？请在评论中告诉我。如果您想阅读更多关于组件模式的内容，这是一个很好的起点。

 [## 组件声明| Vue 模式

### SFC(clicked - 0)引用:基本上，vue 组件遵循单向数据流，即 props down()和 events up…

learn-vuejs.github.io](https://learn-vuejs.github.io/vue-patterns/patterns/#component-declaration)