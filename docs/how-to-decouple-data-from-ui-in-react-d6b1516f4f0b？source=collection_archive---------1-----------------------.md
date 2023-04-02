# 如何在 React 中将数据与 UI 解耦

> 原文：<https://javascript.plainenglish.io/how-to-decouple-data-from-ui-in-react-d6b1516f4f0b?source=collection_archive---------1----------------------->

## 第 2 部分:对钩子、渲染道具和特设模式的进一步探索

![](img/4eee848f76b06b83000e63390cd1e60a.png)

c. 1512, Oil on canvas, Source: [programming-memes.com](https://programming-memes.com/do-you-like-spaghetti/)

在第 1 部分的[中，我介绍了一种将数据获取/管理层从 React 组件中分离出来的方法，React 组件基于数据呈现一些 UI，这将使我们从任何特定的数据库或框架中解放出来。让我们称这种**方法为**。](https://medium.com/javascript-in-plain-english/decouple-data-from-ui-with-react-hooks-6f7fe968c3e3)

## 方法 a .定制挂钩

让我们创建一个定制的钩子— `useSomeData` —它返回属性`someData`、`loading`和`error`，而不考虑数据获取/管理逻辑。以下是`useSomeData`的 3 种不同实现。

*带有获取 API 和组件状态:*

*带 Redux:*

*同阿波罗 GraphQL:*

上面的 3 个实现是**可互换的**而不需要修改这个 UI 组件:

但是，正如 [Julius Koronci](https://medium.com/u/3efad7746b00?source=post_page-----d6b1516f4f0b--------------------------------) 正确指出的，虽然数据获取/管理逻辑被解耦，但是`SomeComponent` UI 仍然耦合到`useSomeData`钩子。

换句话说，即使我们可以重用没有`SomeComponent`的`useSomeData`，**也不能重用没有** `**useSomeData**` **的** `**SomeComponent**` **。**

也许这就是渲染道具和高阶组件在加强关注点分离方面做得更好的地方(再次感谢 Julius 强调了这一点)。

## 方法 b .渲染道具

不要使用返回`someData`、`loading`和`error`的定制钩子，让我们创建一个渲染道具组件——`SomeData`——它包装一个函数(即`children`需要是一个函数)，实现数据逻辑，并将`someData`、`loading`和`error`传入函数。

您可以用 Redux、Apollo GraphQL 或您选择的任何数据提取/管理层来替换上面代码片段中的第 4 行。

**我们现在可以重用** `**SomeComponent**` **(UI 组件)而不用** `**SomeData**` **(渲染道具组件)。我们也可以重用** `**SomeData**` **而不用** `**SomeComponent**` **。**

## 方法 c .高阶组件(HOC)

让我们创建一个特设组件——`withSomeData`——它接受 React 组件作为参数，实现数据逻辑，并将`someData`、`loading`和`error`作为道具传递到包装的 React 组件中。

您可以用 Redux、Apollo GraphQL 或您选择的任何数据提取/管理层来替换上面代码片段中的第 5 行。

**我们现在可以重用** `**SomeComponent**` **(UI 组件)而不用** `**withSomeData**` **(HOC)。没有** `**SomeComponent**` **我们也可以重用** `**withSomeData**` **。**

今天我学会了。

你更喜欢哪种方法，为什么？

# 资源

*   [了解反应渲染道具和道具](https://blog.bitsrc.io/understanding-react-render-props-and-hoc-b37a9576e196)由[阿迪蒂亚·阿加瓦尔](https://medium.com/u/9c555799c00e?source=post_page-----d6b1516f4f0b--------------------------------)
*   [React Hooks:渲染道具会怎么样？](https://kentcdodds.com/blog/react-hooks-whats-going-to-happen-to-render-props)作者[肯特·c·多兹](https://medium.com/u/db72389e89d8?source=post_page-----d6b1516f4f0b--------------------------------)
*   [高阶组件 vs 渲染道具](https://www.richardkotze.com/coding/hoc-vs-render-props-react)作者[理查德·科茨](https://medium.com/u/e503a3e5ff3e?source=post_page-----d6b1516f4f0b--------------------------------)

# 阅读更多

[](https://medium.com/javascript-in-plain-english/intro-to-react-server-side-rendering-3c2af3782d08) [## React 服务器端渲染简介

### 如何在没有任何工具或框架的情况下构建一个 React SSR app？

medium.com](https://medium.com/javascript-in-plain-english/intro-to-react-server-side-rendering-3c2af3782d08) [](https://medium.com/javascript-in-plain-english/decouple-data-from-ui-with-react-hooks-6f7fe968c3e3) [## 用 React 钩子将数据从 UI 解耦

### 以及我如何用 JavaScript 函数“编程到一个接口”

medium.com](https://medium.com/javascript-in-plain-english/decouple-data-from-ui-with-react-hooks-6f7fe968c3e3) 

📫*咱们连上* [*LinkedIn*](https://www.linkedin.com/in/suhanwijaya/) *或者*[*Twitter*](https://twitter.com/suhanw)*！*