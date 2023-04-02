# 上下文 API:React 开发人员的必备知识

> 原文：<https://javascript.plainenglish.io/react-context-apis-in-detail-5943b54940f5?source=collection_archive---------2----------------------->

## 什么是 React 上下文，它对开发者有什么好处？

**Technofunnel** 展示了另一篇关于 **React 上下文 API**的文章，该 API 提供了一种通过组件树**传递数据的机制，而无需手动将道具传递到每一级**。我们**不需要将数据传递给每个级别**，相反，我们**在更高级别的组件**上创建一个上下文，并使**上下文数据对子组件或子组件**可用。本文着重于详细理解 React 上下文概念。

![](img/d254b323817e32bccf1e177eba6d25ec.png)

Working with React Context

# 何时使用 React 上下文

在本节中，我们将看到何时需要使用 **React 上下文 API**。React 上下文 API 使用简单且非常高效。React 应用程序可能由具有父子关系的组件层次结构组成，该层次结构可以有多个级别。

假设我们的应用程序在顶层包含一个组件，该组件包含一些数据，这些数据需要被较低层的组件使用。组件包含在树状结构中，其中一个组件是父组件，其他组件表示子组件，这些子组件可以具有表示树状结构的更多子组件。

## 让我们看一个简单的例子。

[https://gist.github.com/Mayankgupta688/152825398bd8fe975ed133a6d20c56a9](https://gist.github.com/Mayankgupta688/152825398bd8fe975ed133a6d20c56a9)

[https://codesandbox.io/s/pedantic-euclid-4i561?file=/src/App.js](https://codesandbox.io/s/pedantic-euclid-4i561?file=/src/App.js)

在上面的例子中，我们有一个顶级组件“祖父母”，它包含一些数据行“姓氏”。这个组件包含另一个组件“Parent ”,我们在“props”数据中将这个“姓氏”信息发送给它。这个“父”组件不使用这个数据，它只需要将这个数据传递给更进一步的子组件“子”，然后这个“子”组件使用这个“姓”信息。

## 上述代码的问题

在上面的场景中，仅在子级别需要数据。但是我们在整个层次结构中传播数据，使其在较低的级别(子级)可用。使用上下文，我们可以减少在整个层次结构中传递数据的开销，使其在最低级别的组件中可用。接下来，让我们详细了解如何使用 React 上下文来减少开销。

# 使用反应上下文

为了解决上述问题，我们可以使用 React 上下文 API。React 上下文 API 包括:

## 1.上下文对象

为了在应用程序中使用上下文，我们首先需要创建一个能够存储应用程序数据的上下文对象。层次结构可以使用该对象来提供或检索数据。下面的代码将使我们能够为层次结构创建一个上下文对象。

```
const FamilyContext = React.createContext({});
```

## 2.上下文提供者

```
const FamilyProvider = FamilyContext.Provider;
```

提供程序用于向该上下文对象提供值。我们可以使用这个上下文提供者对象将 at 添加到上下文中。

## 3.上下文消费者

```
const FamilyConsumer = FamilyContext.Consumer;
```

这个消费者对象用于检索上下文的值，并使该值对组件可用。使用“上下文提供者”保存在上下文中的值可以使用上下文消费者对象来检索。

让我们试着看看相同场景的完全实现的代码。

[https://gist.github.com/Mayankgupta688/bac51141facecaf2b8dedb0d2c11ec96](https://gist.github.com/Mayankgupta688/bac51141facecaf2b8dedb0d2c11ec96)

[https://codesandbox.io/s/usingcontextapis-u5rto](https://codesandbox.io/s/usingcontextapis-u5rto)

## 在上面的代码中，有以下几点需要注意:

1.  在顶部，我们创建了上下文对象(FamilyContext)、提供者对象(FamilyProvider)和消费者对象(FamilyConsumer)
2.  使用 Provider 对象将值添加到“祖父”组件的上下文中。我们使用“value”属性将所需的数据添加到提供者对象中。数据不需要在组件“父”中使用。
3.  为了使“Children”组件可以直接使用这些数据，我们使用了消费者对象(FamilyConsumer)。这将使子组件可以直接使用数据。

要在钩子中使用上下文，可以参考下面的 URL:

[](https://medium.com/javascript-in-plain-english/usecontext-in-react-hooks-aa9a60b8a461) [## 如何在 React 钩子中使用“useContext”

### 理解 React“使用上下文”挂钩

medium.com](https://medium.com/javascript-in-plain-english/usecontext-in-react-hooks-aa9a60b8a461) 

# 结束语…

在上面的例子中，数据不需要在整个层次结构中传递，数据在顶层是可用的，并且通过使用上下文对象使子节点直接可用。因此消除了在整个层次结构中使用“props”传递数据的需求。

## 更多文章，请关注我们:

[](https://medium.com/@mayank.gupta.6.88) [## 马扬克·古普塔-培养基

### 阅读马扬克·古普塔在媒介上的作品。9 年的前端技术和平均堆栈经验。正在进行…

medium.com](https://medium.com/@mayank.gupta.6.88) [](https://medium.com/better-programming/https-medium-com-mayank-gupta-6-88-21-performance-optimizations-techniques-for-react-d15fa52c2349) [## 22 反应性能优化技术

### React 编程的最佳优化技术。

medium.com](https://medium.com/better-programming/https-medium-com-mayank-gupta-6-88-21-performance-optimizations-techniques-for-react-d15fa52c2349) [](https://medium.com/better-programming/working-with-react-pure-components-166ded26ae48) [## 使用 React 纯组件

### 什么时候使用纯组件是个好主意？

medium.com](https://medium.com/better-programming/working-with-react-pure-components-166ded26ae48) [](https://medium.com/better-programming/introduction-to-react-hooks-e0102c038bf1) [## React 挂钩简介

### 如何在 React 中使用状态挂钩

medium.com](https://medium.com/better-programming/introduction-to-react-hooks-e0102c038bf1) [](https://medium.com/better-programming/introduction-to-reacts-higher-order-components-hocs-c42182fb634) [## React 的高阶组件(hoc)

### 借助用例理解 React HOCs

medium.com](https://medium.com/better-programming/introduction-to-reacts-higher-order-components-hocs-c42182fb634) [](https://medium.com/better-programming/https-medium-com-mayank-gupta-6-88-react-useeffect-hooks-in-action-2da971cfe83f) [## 反应使用效果挂钩

### 关于 React 的使用效果挂钩，你需要知道的是

medium.com](https://medium.com/better-programming/https-medium-com-mayank-gupta-6-88-react-useeffect-hooks-in-action-2da971cfe83f) [](https://medium.com/better-programming/usememo-hook-in-react-d8d0eda6598a) [## React 中的 useMemo 挂钩

### 如何用 useMemo 钩子优化性能

medium.com](https://medium.com/better-programming/usememo-hook-in-react-d8d0eda6598a) 

感谢您将我们与主题联系起来…您也可以通过我们的 YouTube 频道与我们联系:

[](https://www.youtube.com/channel/UCo-h1M-5M6Y5D4Lgut8ge4w) [## 技术漏斗

### 欢迎来到技术漏斗...这个视频关注的是 JavaScript 中承诺的概念。异步等待是新的…

www.youtube.com](https://www.youtube.com/channel/UCo-h1M-5M6Y5D4Lgut8ge4w) [](https://www.facebook.com/TechnoFunnel) [## 技术漏斗

### 印度德里的 TechnoFunnel。47 喜欢 16 谈论这个。TechnoFunnel 专注于提供 IT 服务

www.facebook.com](https://www.facebook.com/TechnoFunnel) [](https://www.facebook.com/developerfunnel/) [## 显影剂漏斗

### 英国伦敦开发商漏斗。285 个赞。寻找最新话题|面试必备话题

www.facebook.com](https://www.facebook.com/developerfunnel/) [](https://www.facebook.com/learnjavascriptbasics/) [## learn-javascript.in

### 在印度德里学习 javascript。1.9K 喜欢。该页面的目的是集中学习 JavaScript 的基础知识…

www.facebook.com](https://www.facebook.com/learnjavascriptbasics/) [](https://www.youtube.com/channel/UC26NMdgQBbY6wunk_vZwQZQ) [## 显影剂漏斗

### 这个频道将提供所有的新闻和技术更新

www.youtube.com](https://www.youtube.com/channel/UC26NMdgQBbY6wunk_vZwQZQ) 

# techno funnel # JavaScript # react #编程#开发#前端

*更多内容看*[***plain English . io***](http://plainenglish.io/)