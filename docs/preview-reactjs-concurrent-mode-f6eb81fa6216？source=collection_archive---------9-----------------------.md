# 预览:ReactJS 并发模式

> 原文：<https://javascript.plainenglish.io/preview-reactjs-concurrent-mode-f6eb81fa6216?source=collection_archive---------9----------------------->

## 介绍新的渲染引擎

![](img/94badc6b24fe56ae58d6e0116a140b42.png)

React 引擎正在发展，加入了一些高级渲染功能。继续阅读，了解新的*并发渲染模式*的概况。

这太令人兴奋了。

> “……组件中一定量的工作总是会导致断断续续……**并发模式通过使渲染可中断来解决这一基本限制。”**

这是来自[的 React 文档](https://reactjs.org/docs/concurrent-mode-intro.html)，它很好地深入描述了即将到来的 C *oncurrent Mode* 。现在可以使用了，作为 React 16.6+中的一个实验特性。

我在这里要做的是给出一个简明的概述，以防你没有足够的带宽浏览整篇文档。

这项技术背后的基本思想是，渲染引擎将接受来自应用程序的暗示 UI 更新的更新，并首先将它们应用到内存中。

> 这项技术背后的基本思想是，渲染引擎将接受来自应用程序的暗示 UI 更新的更新，并首先将它们应用到内存中。

目的是让绘制 UI 更流畅。

动态更新首先应用于虚拟 dom，然后写入屏幕。

与其依赖开发人员使用去抖动或节流(无论如何，它们的有效性有限)来避免不稳定的 UI 更新，*渲染引擎将承担起在数据更改和屏幕写入之间进行调解的任务。*

我认为 React 团队在这个特性上为开发者做了更多的工作。他们对如何最大化用户体验进行了大量的思考和研究，然后将其纳入框架(… *库)*本身。

用于管理这些功能的 API 仍在不断变化，您必须像这样明确地安装它:

```
npm install react@experimental react-dom@experimental
```

并发模式是对 react 工作方式的全面改变，要求根级节点通过并发引擎传递。这是通过在应用程序根上调用 createRoot 来完成的，而不仅仅是“reactDOM.render()”:

```
ReactDOM.createRoot(
  document.getElementById('root')
).render(<App />);
```

(createRoot 只有在安装了实验包的情况下才可用)

因为这是一个更大的根本性改变，现有的代码库可能与它不兼容。尤其是现在前置 UNSAFE 的生命周期方法是不兼容的。

因为这个事实，React 在我们今天使用的老式渲染引擎和并发模式之间引入了一个中间步骤。这被称为“阻塞模式”,它更加向后兼容，但是具有较少的并发特性。

从长远来看，并发模式将成为默认模式。

在中期，React 将支持以下三种模式(这直接来自[这份文件](https://reactjs.org/docs/concurrent-mode-adoption.html)):

*   **遗留模式:** `ReactDOM.render(<App />, rootNode)`。这就是 React 应用程序现在使用的。在可预见的未来，没有计划移除传统模式——但它将无法支持这些新功能。
*   **封锁模式:** `ReactDOM.createBlockingRoot(rootNode).render(<App />)`。目前还在试验阶段。这是希望获得并发模式特性子集的应用程序的第一步迁移。
*   **并发模式:** `ReactDOM.createRoot(rootNode).render(<App />)`。目前还在试验阶段。将来，当它稳定下来后，我们打算让它成为默认的反应模式。该模式启用*所有*新功能。

> [www.darkhorse.tech](http://www.darkhorse.tech)