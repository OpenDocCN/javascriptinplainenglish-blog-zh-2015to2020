# React 路由器鲜为人知的特性

> 原文：<https://javascript.plainenglish.io/lesser-known-features-of-react-router-28319fd30965?source=collection_archive---------6----------------------->

## #2 内存路由器

![](img/3a8369d8959acff1220c345b4fd74f23.png)

React-router 是 React 中使用最多的路由库之一。它方便了 web (React.js)和 mobile (React Native)的路由和导航，因此我们可以依靠一个单一的解决方案来满足 React 生态系统中的所有导航需求。然而，我们中的大多数人，尤其是 React 的初学者，经常坚持 React-Router 的基础知识，而不知道它提供的一些其他功能。本文介绍了 React 路由器的一些较少讨论的特性。对这些有一个公平的理解可能会在你的 React 开发者之旅中派上用场。

# 01.提示

当用户在我们的 web 应用程序中填写长/多步表单时，他/她可能会意外关闭浏览器选项卡。这可能导致用户不得不重新开始漫长、单调的表单填写。如果我们可以在用户将要关闭标签页时警告他们，会怎么样？有了 React-Router 的`Prompt`组件就没那么难了。

Usage of the Prompt Component of React-Router

在上面的代码片段中，只有当表单没有完成并且用户关闭选项卡时，`Prompt`才会出现。这可以防止我们的用户意外关闭网页并丢失未保存的进度。

# 02.内存路由器

您是否曾经遇到过我们希望在非浏览器环境中保留路由历史的情况(例如，React Native)？也有一些情况下，我们只需要模拟导航，比如在测试中。`MemoryRouter`是 React-Router 在这种情况下的解决方案。它接受一组路由，您可以通过编程来处理有关路由器的所有事情，这样您就可以完全控制您定义的路由器。

MemoryRouter of React Router in a testing environment

上面的代码片段展示了如何在测试中使用`MemoryRouter`。这里，我们检查`Home`组件是否为 `/`路线渲染。在这种情况下，我们只需要模拟导航，而`MemoryRouter`就是这样的工具。

# 03.静态路由器

在 React 服务器端渲染中，React 应用程序请求的内容首先在服务器上编译，然后发送到客户端。在这种情况下，我们既不能使用`BrowserRouter`也不能使用`HashRouter`，因为它们只在浏览器环境下工作。因此，我们需要在服务器端用相应的内容来映射路线。React-Router 的`StaticRouter`可以为我们解决这个问题。

Using StaticRouter in Node.js/Express.js server

静态路由器接受一个`route` prop，使用它来确定要提供的相应内容。还有一个`context`属性，可用于将附加数据传递给呈现组件。React-Router 仅在呈现的组件内发生重定向时将一个`url`属性插入到`context`对象中。除此之外，你可以完全控制`context`对象，这样你就可以给它添加你自己的属性。这就是我们如何在 React 中的服务器端渲染中处理路由。

# 04.生成路径

有时，您希望根据特定条件在几个不同的 URL 之间切换同一个链接的目的地。React-Router 的`generatePath`功能使基于不同标准的不同 URL 之间的切换变得简单。

generatePath example

在上面的代码中，你可以看到随着`isPosts`的改变，`url`变量的值在`users/:id/comments`和`users/:id/posts ,` 之间切换。考虑到状态变量`isPosts`的值，`generatePath`产生相应的 URL。

在本文中，我们讨论了 React-Router 库的一些不太常见的特性。我期待着阅读您深思熟虑的建议和反馈。

保持安全，感谢阅读💖

**资源:**

[](https://reacttraining.com/react-router/web/guides/quick-start) [## React 路由器:React 的声明式路由

### 学习一次，路由到任何地方

reacttraining.com](https://reacttraining.com/react-router/web/guides/quick-start) 

## 简单英语的 JavaScript

通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达爱意吧！**