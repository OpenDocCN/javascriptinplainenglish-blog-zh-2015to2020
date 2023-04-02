# 用普通 JavaScript 延迟加载图像

> 原文：<https://javascript.plainenglish.io/lazy-loading-images-with-vanilla-javascript-9e5e5a246df2?source=collection_archive---------8----------------------->

## 通过延迟加载图像提高性能

![](img/e4cfbd4a14afc27840284f5ed47a905a.png)

Image Created with ❤️ By Mehdi Aoussiad.

# 介绍

今天互联网上的每个网站都需要图片。无论是营销横幅、产品图像还是徽标。想象一个没有图片的网站是不可能的。但问题是，这些图像的大小很大，因此，如果我们有很多图像需要同时加载，它们会影响我们的网页的性能。但是，通过延迟加载，我们可以解决这个问题。

延迟加载是一种有助于改善页面加载时间和减小页面大小的技术。我们可以将这种技术与**交叉点观察器 API** 一起使用，这将允许我们在滚动时只加载用户屏幕上可见的图像，而不是网页上使用的所有图像。因此，这对性能和用户体验都有好处。

[](https://medium.com/javascript-in-plain-english/exploring-the-intersection-observer-api-in-javascript-d1e80d6b97c4) [## 探索 JavaScript 中的交叉点观察器 API

### 通过实例了解交叉口观察者

medium.com](https://medium.com/javascript-in-plain-english/exploring-the-intersection-observer-api-in-javascript-d1e80d6b97c4) 

在本文中，我们将使用带有普通 JavaScript 的交叉点观察器 API 制作一些延迟加载的图像。我们将简单地从 Unsplash 中获取一些图像，并使它们延迟加载。你所需要的只是一些关于 **HTML** 、 **CSS** 、 **JavaScript** 的基础知识。让我们看看这个简单的项目是什么样子的。

# 项目演示

正如你所看到的，当我们滚动页面时，只有 20%的图像在屏幕上可见，单个图像才会被加载。

# 让我们从 HTML 开始

HTML 文件将有一个容器 div，我们将把从 [Unsplash](https://unsplash.com/) 导入的所有图像放在其中。所有这些图像都有一个允许我们访问它的链接，您可以通过点击它并在一个新的选项卡中打开它(在 Unsplash 中)来获取图像的链接。例如，下面的链接是我们的一个图片链接:

```
https://images.unsplash.com/photo-1552519507-da3b142c6e3d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=750&q=80
```

将`**crop&w=750&q=80**`更改为`**crop&w=10&q=80**`会使我们的图像变得模糊，并阻止图像加载。因此，我们需要在 HTML 中这样做，然后在图像可见时在 JavaScript 中更改它。这将应用于所有图像。看看下面的例子:

The HTML Structure.

# 让我们来设计我们的元素

所以现在，我们将使用 CSS 样式化我们的元素。您可以阅读下面的代码来查看我们的样式表。

Styling Our Elements with CSS.

# JavaScript 部分

在 JavaScript 中，我们将使用交叉点观察器 API 来检查屏幕上图像的可见性。如果图像的 20%是可见的(阈值:0.2)，我们将改变它的链接，使其加载。因此，只会加载用户屏幕上的可见图像。您可以查看下面的代码了解更多细节。

Our JavaScript Code.

现在恭喜你，你已经用普通的 JavaScript 轻松地创建了一些懒惰加载的图像。

# 结论

延迟加载对于提高页面性能非常有用和重要。如今，图像对每个网站和应用程序都至关重要，为了不影响页面性能和用户体验，延迟加载图像是必须的。

感谢您阅读本文，希望您觉得有用。

## 进一步阅读

[](https://medium.com/javascript-in-plain-english/5-ways-to-improve-your-web-page-performance-1af1f57175bb) [## 提高网页性能的 5 种方法

### 提高网页性能的 5 个有用技巧

medium.com](https://medium.com/javascript-in-plain-english/5-ways-to-improve-your-web-page-performance-1af1f57175bb)