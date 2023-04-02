# 平滑运算符:React 中的并发模式

> 原文：<https://javascript.plainenglish.io/smooth-operator-concurrent-mode-in-react-bf303de4c161?source=collection_archive---------13----------------------->

![](img/a87a76f6b7db77488ca5f25b59055c27.png)

Photo by [Clark Young](https://unsplash.com/@cbyoung?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/two-rivers?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## 警告:在撰写本文时，并发模式是*而不是*尚未稳定，不应用于生产。

并发模式是今年 2 月发布的一个实验性特性，随着 React 16.13.0 的发布。在深入研究该特性修复的问题之前，我们先来讨论一下 React 目前是如何工作的。

![](img/cfdbc32d38db890760c243ad1c4fa5be.png)

Photo by [Nicole De Khors](https://burst.shopify.com/@ndekhors?utm_campaign=photo_credit&utm_content=Free+Stock+Photo+of+Pedestrian+Stop+Light+%E2%80%94+HD+Images&utm_medium=referral&utm_source=credit) from [Burst](https://burst.shopify.com/light?utm_campaign=photo_credit&utm_content=Free+Stock+Photo+of+Pedestrian+Stop+Light+%E2%80%94+HD+Images&utm_medium=referral&utm_source=credit)

# 分块渲染

正如您可能知道的，如果您在 React 中花过时间进行开发，任何状态更改都会触发 DOM 和虚拟 DOM 之间的比较，如果发现两者之间有任何差异，就会触发组件的重新呈现。

这个功能很棒，也很有帮助……大多数时候都是如此。通常，如果状态中的某些内容已经更新，并且状态中的数据显示在 UI 中，我们也会希望更新 UI。

但是，当这些变化发生得如此之快，以至于一个重新渲染无法在另一个被触发之前完成时，会发生什么呢？或者，当用户的网络或设备刚刚过时，而且速度有点慢时，情况又会如何呢？你为你的用户得到了一个次优的体验——这就是所发生的。

React 使用了一种叫做“阻塞渲染”的东西，这简单地意味着，在渲染过程完成之前，所有其他渲染都被阻塞。在上面提到的快速连续和过时的设备场景中，阻止渲染会造成某种视觉上的断续。这种视觉上的停顿是 DOM 试图跟上用户正在做的事情。

考虑这样一个场景，您的用户正在筛选数千条记录，使用的输入随着用户键入内容进行筛选。对于每封打出的信，这是一个过程:

1.  调用更改处理程序时
2.  输入在状态中更新
3.  已过滤项目的列表在状态中更新
4.  用新的过滤项目列表更新虚拟 DOM
5.  比较虚拟 DOM 和 DOM，找出差异，重新呈现组件

不过，通常在用户键入另一封信之前，这些都无法完成。随着用户不断键入，每个后续的重新渲染都被阻止，直到当前的渲染完成。

# 并发模式下的可中断渲染

![](img/2247443eac27fecfd48977aa9264d58d.png)

Photo by [Brennan Tolman](https://www.pexels.com/@brennan-tolman-250017?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/aerial-photo-of-four-cars-on-round-about-at-daytime-771925/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

并发模式是 React 对这种复杂性的回答。正如 React 文档所述:

> 并发模式是一组新功能，有助于 React 应用程序保持响应，并根据用户的设备功能和网络速度进行适当调整。

它通过引入可中断呈现来做到这一点。有了可中断呈现，浏览器不再被阻止可视地进行更新(例如，更新文本输入)*，因为它发生了*。它不再需要等待更改处理程序和输入状态更新以及过滤项状态更新和重新呈现的过程，只需进行显示一个附加字母的更改。尽管应用程序继续运行它的进程，但是只有当呈现完成时，DOM 才会为用户更新。

简而言之，应用程序以同样的方式运行，但是用户体验得到了显著的改善。

# 有意装载顺序

与可中断呈现一样，并发模式也引入了有意加载序列。有意加载序列是一个内置工具，允许应用程序在导航到不同页面时，停留在当前页面上，直到下一个页面“就绪”，即，直到页面被加载和数据被获取。在并发模式之外，这在技术上是可能实现的，但这并不容易。在并发模式下，它是内置的。

![](img/0a0637daa18927716540fa3ac0d109a7.png)

Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/caution?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 小心行事

我想重申的是，并发模式在撰写本文时(2020 年 4 月)是纯实验性的，在它稳定之前不应用于生产。现在，你可以用 [React 的文档](https://reactjs.org/docs/concurrent-mode-intro.html)更深入地研究它，并开始在开发中测试它。您还可以检查同一个链接，看看它何时变得稳定，可以投入生产。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物: [**AI in Plain English**](https://medium.com/ai-in-plain-english) ，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****