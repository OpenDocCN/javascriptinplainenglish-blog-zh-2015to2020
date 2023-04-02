# next . js SSR vs SSG &渐进性

> 原文：<https://javascript.plainenglish.io/next-js-ssr-vs-ssg-incrementalism-pt-1-2-490bce12d97?source=collection_archive---------5----------------------->

## 第一部分

![](img/fad5bf1fc425b205ea4f1c713540e033.png)

Photo by [Florian Olivo](https://unsplash.com/@florianolv?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

最近，我一直在和 [Next.js](https://nextjs.org/) 一起工作。它帮助你在服务器上预渲染你的站点的能力是非常强大的。在深入 Next.js 及其预渲染方法的任何细节之前，让我们后退一步，一般性地讨论基本的客户端渲染和服务器端渲染。

之后，[点击这里](https://medium.com/javascript-in-plain-english/next-js-ssr-vs-ssg-incrementalism-pt-2-2-3928757e9dc)查看第 2 部分，在这里我们将通过示例更深入地探究 Next.js 预渲染策略。

在过去的几年里，我已经开发了几个 [Create React App](https://create-react-app.dev/) (CRA)应用，心智模型是这样的:

1.  当您运行`npm run build`时，您的 React 代码、样式等。被打包成一个(或几个)javascript 包
2.  在新构建的 JS 包旁边有一个框架 HTML 页面，JS 包含在 HTML 中
3.  将这个结构放入一个 CDN /静态托管站点，浏览器就可以请求全部内容(这很酷——不需要 web 服务器来管理站点的加载)
4.  当浏览器获得这个包时，屏幕将是空白的，直到这个包加载所有的 JS，创建 React 虚拟 DOM，将 HTML 绘制到屏幕上，并最终使页面与 JS 的其余部分交互(这是因为框架 HTML 文件还没有任何东西要显示，所以浏览器需要做所有的工作来使它可视化)
5.  ^这是*客户端渲染=* ***CSR***

最近，我一直在做一些 [Next.js](https://nextjs.org/) 项目，故事发生了很大变化。在《下一个世界》中，一切都是关于*预渲染*，这与 **CSR** 正好相反。[直从源头](https://nextjs.org/docs/basic-features/pages#pre-rendering):

> Next.js **预渲染**每一页。这意味着 Next.js 预先为每个页面生成 HTML，而不是全部由客户端 JavaScript 完成。预渲染可以提高性能和 SEO。

太棒了。在这个世界上，我们卸下了浏览器的重担。浏览器不必解包 JS 包，只是构造虚拟 DOM 并最终在屏幕上绘制一些东西，Next 将在服务器端生成一些实际的 HTML，这样当用户访问某个页面时，浏览器将接收到直接的 HTML，它可以超快地将 HTML 放到屏幕上。让我们更深入地讨论一下通用服务器端渲染…

# 服务器端渲染(SSR)

[我的下一篇文章](https://medium.com/@neightjones/next-js-ssr-vs-ssg-incrementalism-pt-2-2-3928757e9dc)将着眼于 next.js 中用于预渲染的现代技术，但让我们首先讨论传统的 SSR(SSR 是 Next 的“预渲染”的一个组件)。在 SSR 中，就像我上面提到的，服务器现在负责生成实际的 HTML 并发送给客户机。在最简单的情况下，想象一个完全静态的“关于我们”页面，也就是说，没有交互——只有一些 HTML 和 CSS。服务器可以很容易地将它发送出去，因为它已经准备好了。

但是一个从数据库中列出一堆产品的页面会怎么样呢？这就是像[手柄](https://handlebarsjs.com/)这样的模板语言的用武之地。例如，您可以创建一个具有一些特殊循环语法的 HTML *模板*，以便在请求时，服务器可以从数据库中获取所有产品，将它们一个接一个地添加到 HTML 模板中，然后将那个*水合* HTML 发送到浏览器。每个请求都会要求服务器完成这项工作，所以您总是可以在客户端获得最新的数据库结果。

预示着，如果我们可以获得 SSR 的好处，而不需要每次都去 web 服务器并让它工作，会怎么样？

# 快速回顾:CSR 与 SSR

**💻CSR 优点&缺点缺点**

1.  利:像 CRA 这样的工具生成的 javascript 包可以放在 CDN 中。这意味着它位于全球的边缘位置，因此用户可以超级快速地获得它
2.  教授:在最初的工作流程中，不需要担心网络服务器(只需要从浏览器到 CDN 再回来)…尽管如果你的应用和 AJAX 中有一些交互部分，这些当然会影响到你的后端服务器
3.  缺点:尽管能够从附近的数据中心快速获得这个包，但在处理完所有 javascript 以最终创建虚拟 DOM 并绘制到屏幕上之前，您的浏览器甚至不能显示任何内容
4.  缺点:传统上，你在这里会遇到 SEO 的问题，因为网络爬虫只会看到你的包含 JS 包的“空白”HTML 页面(最终会发展成一堆 HTML，但还不是时候)。看起来这可能会成为一个小问题，例如谷歌首先处理 javascript(？)

***☁️ SSR 优点&缺点***

1.  优点:在服务器上生成的 HTML 适合浏览器。浏览器在呈现准备显示的 HTML 时速度很快。除此之外，浏览器不需要获得一个巨大的包，而是只需要将要被浏览的 HTML
2.  利:搜索引擎优化没有问题，因为这是传统的做法(网络爬虫会直接看到来自服务器的 HTML)
3.  缺点:在纯 SSR 中，服务器为来自客户端的每个请求构建一个页面，服务器参与加载每个页面，并且服务器可能会做很多工作(如果有 10 个请求进来，它需要生成 10 个 HTML 页面来返回)
4.  反对:对于严格意义上的 SSR，CDN 级别没有缓存任何内容。服务器总是为每个请求生成 HTML

# 接下来— Next.js 预渲染策略

这只是 SSR 故事的开始。Next.js 将预渲染的想法提升到了*的下一个*级别。超越我上面描述的传统 SSR，Next 增加了另一种方法，*静态站点生成，*以及一些额外的动态层。是的，静态站点有时表现得像动态站点。

查看我的下一篇文章，了解更多关于 Next.js 中的静态生成，包括增量静态生成:

[](https://medium.com/javascript-in-plain-english/next-js-ssr-vs-ssg-incrementalism-pt-2-2-3928757e9dc) [## next . js SSR vs SSG &渐进性

### 第二部分

medium.com](https://medium.com/javascript-in-plain-english/next-js-ssr-vs-ssg-incrementalism-pt-2-2-3928757e9dc)