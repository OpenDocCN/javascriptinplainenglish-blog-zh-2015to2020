# next . js SSR vs SSG &渐进性

> 原文：<https://javascript.plainenglish.io/next-js-ssr-vs-ssg-incrementalism-pt-2-2-3928757e9dc?source=collection_archive---------1----------------------->

## 第二部分

![](img/cc382fc591ee6811dc9f782409863a5a.png)

Photo by [Scott Graham](https://unsplash.com/@sctgrhm?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在我的上一篇文章中，我回顾了客户端渲染(CSR)和服务器端渲染(SSR)的一些基础知识。如果你不熟悉，请点击这里查看:

[](https://medium.com/javascript-in-plain-english/next-js-ssr-vs-ssg-incrementalism-pt-1-2-490bce12d97) [## next . js SSR vs SSG &渐进性

### 第一部分

medium.com](https://medium.com/javascript-in-plain-english/next-js-ssr-vs-ssg-incrementalism-pt-1-2-490bce12d97) 

[Create React App](https://create-react-app.dev/) 是 CSR app 的典范， [Next.js](https://nextjs.org/) 是 SSR 世界的引子。如前所述，Next.js 通过大量选项将服务器上的所有渲染提升到了一个新的水平，因此这篇文章将通过用例解释它的一些功能。

🗒快速提示:你可以谷歌一下“CRA SSR ”,有很多事情可以做。我只是在谈论基本的开箱即用功能。

# Next.js 中的预渲染

Next.js 侧重于预渲染，这是一个比 SSR 更通用的术语。[文档](https://nextjs.org/docs/basic-features/pages#pre-rendering)描述预渲染包括两件事:

1.  服务器端渲染(SSR)，就像上一篇文章谈到的那样
2.  静态发电(SSG =静态站点发电)

SSG 类似于 SSR，服务器负责预渲染 HTML(没有 CSR！)，但是**它发生在构建时，**即一个现成的 Next.js 项目的`npm run build`。一般来说，你可以静态地构建对所有客户都一样的页面(营销页面、博客文章页面等)。更多关于用例的内容将会出现)。

为什么构建时生成的东西很棒？

1.  当你运行一个`build`命令时，这是在实际访问你的站点的人的工作流程之外，所以如果花一点时间也没问题
2.  一旦你的 SSG 页面建好了，它们就可以放在 CDN 里了。这意味着 HTML 位于世界各地的边缘位置，浏览器为用户请求和呈现这些内容的速度非常快
3.  您的 web 服务器将不必为从 CDN 提供的静态生成的页面做任何工作，因此您不需要担心它在这种情况下的性能

这些优势融合了 CSR 和 SSR 的优势，就像我在上一篇文章中描述的那样——即服务器负责创建实际的 HTML，但我们仍然可以将其放在 CDN 中。Next.js 文档的主题实际上变成了“如果你能用 SSG 做用例，就去做吧。

Next.js 文档很棒，但是有几个概念让我对 SSG 有了更好的选择。下面的用例试图澄清一些事情。

# 密码

代码样本可以在这个回购这里找到[https://github.com/neightjones/next-ssr-ssg-blog](https://github.com/neightjones/next-ssr-ssg-blog)

# 用例 1:简单的营销页面

这个例子很清楚。假设我们有一个“关于我们”页面，由一个没有数据获取或任何东西的组件组成——只是一个静态页面。查看报告中的[关于我们的页面。](https://github.com/neightjones/next-ssr-ssg-blog/blob/master/pages/AboutUs.js)

我们当然希望它在构建时静态生成，因为这很简单。每个用户都会看到同样的东西，所以可以直接从 CDN 获得服务。幸运的是，SSG 是 Next.js 的默认版本。当您运行`npm run build`时，它将变成一个基本的 HTML 页面，一切都准备好了。

# 用例 2:博客文章列表

现在我们已经超越了案例 1，更棘手的案例都将涉及某种动态内容。这可能意味着两件事:

1.  页面上的某些内容会随着时间的推移而改变(例如，您向您的博客添加了新的博客帖子，所以现在列出这些内容的页面需要显示新的帖子)
2.  由于新的内容，已经存在的页面集合会扩大(例如，现在你添加了一个新的博客帖子，你可能会有一个额外的页面，像`/posts/<new_post_id>`

让我们首先关注这个组件，列出我们所有的博客文章。这是在 Next.js 中使用服务器端渲染的情况吗？毕竟，随着博客文章的增加，我刷新我的浏览器，我想看到所有最新的文章。有了 SSR，我对列表页面的请求每次都会被构建在服务器上，然后服务器总是会从数据库(或者文件系统，但是我的博客文章是被存储的)中获取最新的内容。SSR 会给你你想要的结果，但是 Next.js 可以做得更好。

进入 ***带数据静态生成。*** 下一步。js 有一个内置的方法来完成 SSG，即在**构建**时依赖外部数据，所以您可以预构建所有需要的东西并将其放入 CDN 中。你需要做的就是在你的一个页面组件上实现一个名为`getStaticProps`的`async`函数。

我不会重新发明轮子，但这里有一个简单的例子。[查看此处单据](https://nextjs.org/docs/basic-features/pages#static-generation-with-data)可了解更多详细信息。[此处链接至回购组件。](https://github.com/neightjones/next-ssr-ssg-blog/blob/master/pages/BlogList.js)

由于`getStaticProps`已经实现，Next 将知道在构建时运行`getBlogs`(并且在服务器上)，这将创建显示博客文章列表的最终 HTML。这个页面可以位于 CDN 中，所以在浏览器中访问和渲染会非常快。

我们可能也希望每个博客帖子有一个页面，就像`/posts/<some_post_id>`一样。Next.js 还使得在构建时静态生成这些**路径&页面**变得容易。因此，如果你有 3 篇 id 为 1、2 和 3 的博客文章，你可以使用 SSG 预渲染 3 个适当的博客页面，路径都设置为 go ( `/posts/1`、`/posts/2`和`/posts/3`)。要做到这一点，您需要结合使用`getStaticPaths`和`getStaticProps`。不需要重新创建文档，[所以在这里检查一下](https://nextjs.org/docs/basic-features/pages#scenario-2-your-page-paths-depend-on-external-data)。

由于数据的静态生成，我们现在可以在构建时构建所有的博客文章页面和路径，这很棒。如果你写了一篇新的博文会怎么样？所有内容都是在构建时生成的，所以如果你刷新浏览器，你就看不到新的帖子了。在这种情况下，最好的办法是重建网站，这样就可以获得最新的内容。这可能看起来有点烦人，但是内容不会太频繁地改变*和*，并且您可以自动化您的博客的构建/部署过程。

重建和部署的需求当然回避了这个问题——在什么情况下内容变化太快或者变得太大而不值得一直重建你的站点？另一个要考虑的因素是哪些用户可以访问各种内容？对于博客的例子，我假设它是完全公开的。但是，如果有一些私人的，每个用户的数据，我们不希望预先渲染用户数据的一切。下一个用例展示了 Next.js 中的几个 SSG 技巧，以处理更多的复杂性。

# 用例 3:成长中的电子商务网站

这个例子基于 Vercel(next . js 的开发者)的这篇博客文章。我会总结一下，但我也建议浏览他们的完整帖子。

上面，我们用数据的静态生成处理了你的博客站点的预渲染策略。现在，你有一个快速增长的电子商务网站。本周将增加数千种产品，现有的产品列表会经常变化(定价、库存等。).

让我们首先关注列出产品的页面(例如`/products`)，以及单个产品页面(例如`/products/1`)。如果我们尝试应用静态数据生成技术会发生什么？

1.  随着产品被添加到网站，我们将遇到一个问题。我们将不得不频繁地重建网站，这将花费很长时间，因为我们将静态地生成数千个产品页面。这是不可行的。
2.  同样，随着产品的变化，我们也会遇到问题。同样，我们将不得不重建网站以向客户反映产品的变化。也不可行。

我们希望确保客户看到最新的数据，以便他们能够购买产品。**我们最终应该使用 SSR 来确保每个请求的数据都是最新的吗？？**

事实证明，很可能不是！Next.js 有一个叫做**增量静态生成**的概念，它帮助像我们的电子商务商店这样的动态网站留在静态生成的世界里(Next.js 竭尽全力帮助我们使用 SSG！).随着我越来越多地使用 Next.js，我发现这是一些最酷的东西。

***案例 1:在静态生成的构建生命周期中添加产品页面***

假设我们已经静态生成了包含 100 种产品的电子商务站点。但现在 1000 款新品将于本周上市。我们可以在对`getStaticPaths`的调用中使用一个名为`fallback`的键([我在上面也链接过这个](https://nextjs.org/docs/basic-features/pages#scenario-2-your-page-paths-depend-on-external-data)，但是这是在使用 SSG 并在构建时自动生成所有路径和产品页面的情况下)。

正常情况下，`fallback`会是`false`。这正如您所料——任何在构建时没有生成的路径都将返回 404。这很好，因为如果有人去`/products/<non-existent product_id>`，404 是正确的。但是，如果`fallback`设置为`true`，您可以解锁一些特殊行为。[查看官方](https://nextjs.org/docs/basic-features/data-fetching#fallback-true) `[fallback: true](https://nextjs.org/docs/basic-features/data-fetching#fallback-true)` [文档](https://nextjs.org/docs/basic-features/data-fetching#fallback-true)。

使用`true`时，404 **不会因未知路径而返回**。相反，您将从 Next 得到指示，您应该显示一个“回退”页面(本质上是一个加载页面)。同时，服务器将**缓慢地**为用户生成合适的页面。工作流程如下:

1.  我在网站上添加了一个新的 id 为`5000`的产品
2.  一个用户试图进入页面`/products/5000`，但是它还不存在(当然不存在，因为我刚刚添加了产品)
3.  浏览器将为用户显示一个回退/加载页面(没有错误或类似的情况)
4.  服务器为产品 5000 生成 HTML 页面，然后将 5000 添加到它的已知路径集中(现在产品 5000 的页面可以缓存在 CDN 中)
5.  用户现在将看到产品页面，而不是回退页面
6.  在此部署的生命周期内，对于此产品的所有未来请求，产品 5000 都已准备就绪，只需轻松完成一次工作！

***案例 2:在静态生成构建的生命周期中更新产品页面***

这种情况在 Next.js 中更加简单。这种情况被称为增量静态 **Re** generation，因为这是关于在初始构建之后定期重新生成页面。

您所需要做的就是在对`getStaticProps`的调用中添加一个名为`revalidate`的键。例如，`revalidate: 60` (60 秒)会有以下工作流程:

1.  最初，构建了产品 X 的页面。如果它是延迟构建的，用户将在时间 0 看到这个新的产品页面
2.  假设 1 秒钟后，产品的价格发生了变化
3.  现在，大约 30 秒后，另一个用户转到产品 x 的页面。尽管如此，他们仍然会看到旧的*价格信息，因为每 60 秒才重新生成一次*
4.  整整一分钟后，另一个用户请求该页面，Next 知道重新生成产品 X 页面，这将引入新的定价(虽然对于这个用户，他们可能仍然会再次获得旧页面，但他们已经通知 Next 生成新页面)

现在我们的电子商务网站是静态生成的，但是我们有动态元素，包括添加新产品和更新产品！这是个好消息——Next 在这些场景中为我们提供了两个世界的最佳选择。

*编辑:我在本系列中提到过，静态生成的一大好处是您的 web 服务器不在考虑范围内，所以您不必担心它会过载。请注意，对于这种增量静态生成的情况，虽然 CDN 层在提供生成的内容方面至关重要，但在这种情况下，web 服务器会缓慢地创建产品页面，并不时地重新生成页面。*

***关于电子商务网站*** 的“购物车”说明

请注意，上面讨论的所有内容都只涉及每个人都可以看到的公共信息。但是购物车只与特定的用户相关。因此，您最多可以呈现页面的某些部分，但最终，需要在请求时找到关于用户购物车中有什么的数据。你可以在这里使用 SSR，但是[同一个 Vercel 帖子](https://vercel.com/blog/nextjs-server-side-rendering-vs-static-generation#shopping-cart-page-static-generation-without-data,-combined-with-client-side-fetching)推荐**无数据+客户端抓取的静态生成。**

这意味着 Next 将静态生成购物车页面的通用部分(布局、样式等。)，可以缓存在 CDN 中。然后，由浏览器在请求时向服务器请求购物车细节。由于性能的原因，这比 SSR 更受欢迎——如果用户导航到他们的购物车，他们会很快看到*的一些*内容，并加载实际购物车细节的 UI。使用 SSR，在构建整个购物车之前，页面上不会有任何内容。重要的是，在这种情况下，这都是私人用户信息，所以我们可以忽略任何 SEO 优化，而是为用户快速看到*东西*进行优化。

在 Next.js 中，你可以为每个页面选择你想要的预渲染和数据获取策略，所以你可以有一个混合的方法。也看看 [Vercel](https://vercel.com/) 吧。他们是 Next.js 的创造者，他们让部署 Next 应用变得非常容易。