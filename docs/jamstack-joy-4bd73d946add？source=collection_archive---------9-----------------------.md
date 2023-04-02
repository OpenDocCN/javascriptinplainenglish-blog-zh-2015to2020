# 如何以每年 10 美元的价格托管一个 CMS 驱动的网站

> 原文：<https://javascript.plainenglish.io/jamstack-joy-4bd73d946add?source=collection_archive---------9----------------------->

## 詹姆斯塔克乔伊！

![](img/6f3af188e62d1ce47a2e2498252e162d.png)

See what I did there? — Photo by [Viktor Forgacs](https://unsplash.com/@sonance?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 如何以每年 10 英镑的价格托管一个 CMS 驱动的网站

在 Covid 之后，我一直在考虑慈善事业。特别是我们作为编码人员如何利用我们的技能来帮助我们的当地社区。这个想法是，热情的程序员可以免费提供少量的时间给当地的个人/企业，以建立现代的、反应迅速的网站。

快速搜索一下，发现不少运动队、商店和个体企业都有约会或无回应的网站。

我免费提供我的服务，但即使如此，关于托管费用的讨论可能会很尴尬。我以前和 AWS、Azure 一起托管过。就虚拟主机而言，两者都相当实惠。我发现在流量相当低的 AWS 上运行一个 EC2 实例每月要花费我 10 英镑，外加每年 10 英镑的域名费用。

问题是一年 130 英镑对于一个运动队或个人来说兼职是一大笔钱。尤其是当你不确定你将获得多少流量，或者它将真正推动多少业务。

## 静态网站怎么样？

好问题。这是我下一个想法。有很多服务可以让你免费托管一个静态网站(想到了 Firebase)，这意味着你只需要支付 10 英镑的 DNS 费用。

这里的问题是，没有多少企业只是想要一个静态的网站。他们通常希望能够提供更新，或更换人员，或联系页面。他们不希望定期的维护费用让开发商为他们更新网站。

我在这里考虑的另一个潜在选项是一个静态站点，它从 Twitter 中提取内容，从而让用户通过 Twitter 用户界面输入内容，并让内容自动进入他们的站点。我认为这不是很有性能，例如对于人事页面来说不实用。

## 我们为什么需要服务器？

这是一个更好的问题，也是一个今天不常被问到的问题。我们正在谈论建立的网站是我将要称之为——以一种矛盾的方式——一个可以更新的静态网站。

像这样的博客网站本质上是一个静态网站。不需要实时更新。你只需要每隔几天添加/更新一个页面。为什么要运行服务器来完成这个任务呢？它又贵又慢。

## 那么，为什么没有人创建一个无服务器的 CMS 呢？

我在这个问题上被困了一段时间。直到我意识到他们有。它叫做 [Netlify CMS](https://www.netlifycms.org/) 。

这个想法很简单:

1.  你有你的源代码托管在某个地方。这很容易做到，而且是免费的。GitHub 将是最明显的例子。
2.  Netlify CMS 是一个由你托管的开源静态用户界面，它通过 API 做所有的事情。它通过 GitHub 进行认证，本质上是使用 GitHub 文件系统作为 CMS 后端。如果你在 CMS 中添加了一个项目，它会把它推送到 GitHub，如果你删除了这个项目，它会把它从 GitHub 中删除。
3.  然后你就有了一项服务(Netlify 也以非常简单的方式提供这项服务，也是完全免费的),它可以监视你的 GitHub repo，根据任何变化重建你的网站，然后部署/托管它。

不需要服务器。你的网站可以免费托管，加载速度也快如闪电。

静态页面是在部署时生成的，不像服务器在每次访问页面时都生成静态页面。

您需要使用一个框架来生成这样的静态资产。周围有很多。我用的是 [Gatsby.js](https://www.gatsbyjs.org/) 很容易和这个流程集成。我相信同样的事情也可以用其他工具实现，比如 [Nuxt.js](https://nuxtjs.org/) 、 [Next.js](https://nextjs.org/) 和 [Sapper](https://sapper.svelte.dev/) 。

## 有什么坏处？

老实说，我不确定有没有。有些事情你不应该用这个解决方案去尝试和处理。你不能运行像这样需要实时更新的网站，因为每次 CMS 更新后，你必须等待几分钟进行重建，然后才能看到实时网站的变化。你不希望多个改变同时触发构建——我不确定 Netlify 会如何处理，它是否会节流或者是否会永远去抖。但这仍然是一个很好的起点。您只需要为实时方面添加 API 调用。

## 什么是 Jamstack？

在此之前，我已经多次听到这个术语 [Jamstack](https://jamstack.org/) ，但是我现在要举起我的双手，承认我直到此时才完全理解它。我认为我发现的想法本质上是 Jamstack。Jamstack 我会总结成一句话:

> 你的 UI 应该是一小组静态资产。

我想不出什么情况下这不是最好的。任何涉及到服务器的东西都会在到达用户之前引入一个初始延迟(外加额外的运行成本和维护)，而且也没有必要。

我一直误以为服务器端呈现是一个理想的特性，但现在我认为 SSR 只是在运行时呈现，可以在构建时呈现。

仅在需要时调用您的服务器。

祝一切顺利，
尼克

更多阅读资料/资源如果您想复制此设置:

[https://www.netlifycms.org/docs/gatsby/](https://www.netlifycms.org/docs/gatsby/)

[](https://www.gatsbyjs.org/docs/sourcing-from-netlify-cms/) [## 从 Netlify CMS 采购

### 在本指南中，您将使用 Netlify CMS 建立一个具有内容管理的网站。Netlify CMS 是一个开源的，单一的…

www.gatsbyjs.org](https://www.gatsbyjs.org/docs/sourcing-from-netlify-cms/) 

## 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**