# React 中的服务器端渲染—简介

> 原文：<https://javascript.plainenglish.io/server-side-rendering-in-react-an-introduction-55f4c7fa274b?source=collection_archive---------8----------------------->

## 简单解释一下什么是同构/通用应用程序，以及服务器端呈现在这方面的作用。

![](img/d8b0a66dac6e2ea8cf35a2336ea8b6b0.png)

服务器端渲染(SSR)是我很久以来一直想描述的一个主题。我认为这在 SPA(单页 App)应用广泛的时代是一个极好的解决方案。因为这个主题相当广泛，我想详细地呈现它，我认为将故事**分成几个部分**是有用的。今天，在这个故事的第一部分，我将试着向你展示什么是同构/通用应用。我还将阐述什么是服务器端渲染，它最显著的优势是什么。在接下来的部分，我将描述如何把它整合到你的 React 应用中。

# 服务器端渲染和同构/通用应用

一般来说，“同构”的 app 就是我们可以同时在很多平台上渲染的那种。通常，对于网络应用来说，它既在**客户端**(网络浏览器)上，也在**服务器**上。我们可以通过许多不同的方式实现应用程序的同构——早在 SPA 应用程序出现之前，我们就已经在创建这样的应用程序了。任何首先由 PHP/Ruby/Java/C#语言呈现并且使用 JavaScript 实现所有客户端交互的应用程序都可以被称为“同构的”

世界在前进，今天，使用 JavaScript，我们可以实现同构的应用程序，这些应用程序也是通用的。你可以问:什么意思？嗯，这样的应用程序，除了在客户端和服务器上呈现之外，还**使用几乎相同的代码**。值得一提的是，这些名称经常互换使用。这意味着，通过说“同构应用”或“通用应用”，我们通常会想到通用应用。

如果我们在 [React](https://reactjs.org) 的上下文中谈论通用应用程序，当我们创建通用 React 应用程序时，我们通常构建**一个单一的、相互的组件树**。然后，在第一个请求期间，我们在服务器上渲染它([node . js](https://nodejs.org/en/)/[express . js](https://expressjs.com))并发送给浏览器。

**服务器端渲染**是上述方法的服务器部分。在本系列的后续文章中，我将向您展示如何基于 Node.js(例如，express.js)配置 webserver，如何使用它来呈现 React app，以及如何使其全部工作。

# 通用应用程序的优点和缺点

最后我觉得还是值得提几个通用 app 的利弊。

请查看以下最重要的优点:

*   **服务器和客户端共享代码**——你会看到我们应用的所有 React 组件在两种环境下都是一样的
*   **性能(？)和更好的 UX**——由于第一个请求返回由数据预先填充的 HTML 代码，用户不必等待 JavaScript 客户端代码加载，也没有必要显示“AJAX spinners”
*   更好的搜索引擎优化(SEO)——如果我们先在服务器上渲染，就不用担心网络爬虫无法读取我们的页面并正确索引

通用应用程序的缺点:

*   **特定于环境的代码的潜在问题** —例如:在服务器上，没有定义`window`对象(幸运的是，一些库可以解决这样的问题)

# 摘要

今天到此为止。我希望我能够让您对通用 JavaScript 应用程序的主题感兴趣。在下面的文章中，您将看到**自己实现这种方法是多么容易！**

**附言**本帖是关于使用 React 进行服务器端渲染的系列文章的一部分。请参见下面的系列文章列表:

*   [React 中的服务器端渲染—简介](https://medium.com/@bartomiejdybowski/server-side-rendering-in-react-an-introduction-55f4c7fa274b)
*   [React-express . js 中的服务器端渲染](https://medium.com/@bartomiejdybowski/server-side-rendering-in-react-expressjs-8a87af0edba4)
*   [React-Redux 中的服务器端渲染](https://medium.com/@bartomiejdybowski/server-side-rendering-in-react-redux-8d6209fbfed)
*   [React-React-router 中的服务器端渲染](https://medium.com/@bartomiejdybowski/server-side-rendering-in-react-redux-ab0af31c9c4b)
*   React 中的服务器端渲染—处理真实数据