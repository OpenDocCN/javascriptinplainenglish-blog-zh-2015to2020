# 通过编写定制中间件来提高您的 Redux 技能

> 原文：<https://javascript.plainenglish.io/improve-your-redux-skills-by-writing-custom-middleware-32a70b9dfb25?source=collection_archive---------7----------------------->

![](img/1d150c2eb7d01101f7d604484729423b.png)

Redux 是一个状态管理库，通常与 React 一起使用。在本文中，我将教您编写定制的中间件来扩展 Redux 的功能，并深入了解如何管理全局状态。

# Redux 是什么？

如果您知道 Redux 是什么，请随意跳到下一部分。Redux 用于管理全局状态，最常见于 React 应用程序中。它通过存储一个全局的、*不可变的*状态对象来做到这一点，您可以通过**reducer**来修改它。缩减器是一个函数，它接受一个状态对象，一个**动作**(换句话说，事件)对象，并返回一个新的状态对象。动作对象描述了到底发生了什么以及状态应该如何改变。一个动作对象总是有一个`type`属性和可选的有效负载属性。为了更好地理解它，请看这个例子:

# 什么是中间件？

Redux 中的中间件用于覆盖商店的行为，最常见的是`dispatch`函数。`redux-thunk`就是这样一个中间件。它允许您调度异步操作。另一个例子是`redux-logger`，它将每个动作和状态变化记录到控制台。

编写 Redux 自定义中间件的目的是以可重用和一致的方式实现一些高级功能。它应该简化您的开发体验，并清理您的代码。

# 编写 Redux 定制中间件

当我在工作中遇到问题时，我想到了开发定制 redux 中间件的想法。我有一些特定的 API 请求，在任何特定的动作被分派的时候都会被发出。此外，另一个团队成员也有同样的问题。我决定开发一个中间件，允许你拦截某些动作并为它们提供回调。在本文中，我们将开发一个我们最终在生产中使用的中间件的稍微修改的版本。

中间件的定义有点混乱。它应该是一个接收 redux 存储并返回一个接收回调`next`的函数，回调函数返回另一个接收 redux 动作并执行您需要的中间件特性的函数。不要担心，一会儿就都清楚了。首先，考虑这个简单的中间件，它简单地记录每个调度动作的类型:

您可以注意到，在第 2 行，我们记录了动作的类型，并将动作传递给`next`。`next`如果没有更多的中间件，将操作管道化到下一个中间件或最初的分派。给`next`打电话至关重要，否则，你的行动将不会被派遣。顺序也很重要:任何在`next`之前的都将在动作被分派之前执行，任何在动作被分派之后的都将执行。

# 拦截动作

是时候实现拦截功能了。为此，我们将编写另一个函数`createInterceptorMiddleware`，它将获取一个拦截器列表并返回中间件:

现在，在处理动作时，我们按类型过滤拦截器(第 3 行),并调用每个匹配的拦截器的处理程序。在第 9–12 行，拦截器数组被定义并传递给`createInterceptorMiddleware`。您可以注意到，我们也将`action`、`dispatch`和`getState`传递给处理程序，非常像 redux-thunk。

# 支持异步处理程序

虽然我们的中间件已经很有帮助了，但是让它支持处理程序的承诺还是很有用的，非常像 redux-thunk。要实现这一点，我们只需在所有承诺都得到解决后调度操作。此外，在任何处理程序拒绝的情况下，该动作将不会被分派:

该实现将支持异步处理程序，并将等待它们进行解析。在第 6–7 行，我们检查处理程序的返回类型，如果它不是一个承诺，我们就把它变成一个。您还可以注意到，我们在第 5 行使用了`map`而不是`forEach`，因为现在我们必须存储处理程序的返回值，以便`Promise.all`可以等待它们。一旦所有的承诺都被等待，第 10 行将分派动作，第 11 行将记录错误，如果任何承诺被拒绝，则不分派动作。

# 支持调度后处理程序

我们当前的实现在分派动作之前触发处理程序*。为了增加对在分派动作后触发的处理程序的支持，我们将对处理程序本身做一点小小的改变:如果一个处理程序返回一个函数，这个函数将在分派后运行。这将为我们提供各种组合和复杂逻辑的灵活性:*

该特性通过第 12–17 行实现。由于`Promise.all`将解析所有处理程序返回值的列表，我们遍历它并调用任何函数。要使用这个特性，处理程序应该返回一个函数，如第 23-29 行所示。

# 挂上去

最后，您必须将您的中间件实际连接到商店。您可以通过将它传递给`applyMiddleware`函数来实现，该函数将返回一个增强器，该增强器需要传递给`createStore`函数:

感谢您的阅读，希望您喜欢！把这个发给你的朋友/同事，看看我关于高级 JavaScript 的其他文章:

[](https://medium.com/javascript-in-plain-english/react-usememo-and-when-you-should-use-it-e69a106bbb02) [## React.useMemo 以及何时应该使用它

### 尽管 React 经过了很好的优化，开箱即用，但了解它提供的制作工具非常重要…

medium.com](https://medium.com/javascript-in-plain-english/react-usememo-and-when-you-should-use-it-e69a106bbb02) [](https://medium.com/javascript-in-plain-english/10-javascript-interview-questions-for-2020-697b40de9480) [## 2020 年 10 个 JavaScript 面试问题

### 随着对 JS 开发人员需求的增长，你必须做好准备。这里有 10 个问题可以帮助你…

medium.com](https://medium.com/javascript-in-plain-english/10-javascript-interview-questions-for-2020-697b40de9480) [](https://medium.com/javascript-in-plain-english/7-really-good-reasons-not-to-use-typescript-166af597c466) [## 不使用 TypeScript 的 7 个非常好的理由

### 有很多理由使用 TypeScript，但我会给你 7 个不使用的理由。

medium.com](https://medium.com/javascript-in-plain-english/7-really-good-reasons-not-to-use-typescript-166af597c466) [![](img/446049aa060bbaea15a64e1a907b1030.png)](http://eepurl.com/gYiA29)