# 一个你会上瘾的懒加载实用函数

> 原文：<https://javascript.plainenglish.io/a-lazy-load-util-function-you-will-get-addicted-to-e682ab410ac8?source=collection_archive---------9----------------------->

## 一个现代的功能，收集了最新的 JS 功能，让每个人都开心

![](img/e05d9e7c76a3db21cdc448fe289a6ddc.png)

Photo by [Christian Gertenbach](https://unsplash.com/@kc_gertenbach?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/child-play?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 网络性能所隐含的生态系统是巨大的

这不是秘密。Web 性能是每个网站最重要的目标之一，无论它们属于哪种类型:电子商务、机构、信息……事实上，这是满足用户的几个配置文件的共同目标:

*   产品负责人:一个网页必须尽可能快地加载，这样用户才能开始互动并成为顾客。
*   **SEO 经理**:轻量级页面有助于在谷歌、必应等搜索引擎中获得更好的可见度
*   **用户**:网页加载时间太长？用户不会等待，而是去拜访市场上的其他竞争者。
*   支持服务:他们真的不想收到用户的电子邮件，报告网站/应用程序因为太重而无法使用。
*   首席技术官:在首席执行官看来，如果网页太慢，他们会赔钱，这就是他的责任。
*   首席执行官:当竞争对手的首席执行官取笑他说他的网站有多慢时，我认为他不高兴。
*   **Web performance actors** :他们提供工具来衡量网页/应用的性能。
*   **和前端开发者**:他是前几个剖面之间的基石。他必须尽力让每个人都开心。

Web 性能在公司战略中是如此重要，以至于他们经常将它作为一个 KPI(关键性能指标),从层级的顶端一直到底端(你，前端开发人员)。

# 让我们让每个人都开心，包括你

## 延迟加载:减少加载时间的武器

降低网页权重的最好武器之一就是只在页面的水线处加载用户需要的内容。当用户开始通过滚动、点击、写东西等方式在网页中进行交互时，其他资源必须是动态惰性加载的

> 滚动延迟加载有点复杂，因为您需要知道目标元素何时在屏幕上可见，以便加载相关资源。

## 带异步/等待的动态导入

很容易从 click、keyup、doubleclick…中延迟加载资源，因为触发函数是 click 事件函数本身。我们只需要使用两个特性:

[](https://webpack.js.org/guides/code-splitting/) [## 代码分割| webpack

### 本指南扩展了入门和输出管理中提供的示例。请确保您至少…

webpack.js.org](https://webpack.js.org/guides/code-splitting/) [](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function) [## 异步功能

### 异步函数是用 async 关键字声明的函数。异步函数是异步函数的实例…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function) 

多亏了 Webpack 和它的代码分割特性，我们可以很容易地通过点击来导入资源。事件更好，通过在导入函数中添加注释`webpackChunkName`为输出文件设置一个名称。

Lazy-load a resource from a click

> 我有一个梦想:element . addevent listener(' visible ')

载入一个资源，当它在用户界面上变得可见时，没有魔法事件，这将是不可思议的，你不这样认为吗？

## 交叉点观察器 API

通过在 HTML 元素上添加一个滚动事件，可以知道该元素何时可见。但是我们必须计算每个滚动事件的位置，这对浏览器来说是很昂贵的。

我们有另外一个工具可以做到这一点，我敢肯定你已经知道了:这就是交叉点观察器 API

[](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API) [## 交叉点观察器 API

### 交叉点观察器 API 提供了一种异步观察目标元素交叉点变化的方法…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API) 

当目标元素在用户屏幕上可见时，交叉点观察器 API 将帮助我们提供一个触发事件。

如果你的网站，应用程序，应该在旧的浏览器上工作，考虑添加相关的 polyfill:

[](https://github.com/w3c/IntersectionObserver/tree/master/polyfill) [## W3C/intersect observer

### 路口观察者。通过在 GitHub 上创建一个帐户来为 W3C/intersect observer 开发做贡献。

github.com](https://github.com/w3c/IntersectionObserver/tree/master/polyfill) 

> 您是否已经考虑过延迟加载 polyfill，以避免在已经支持该功能的浏览器中加载它？

所以现在我们已经拥有了构建一个延迟加载实用函数的所有要素

## 延迟加载实用程序函数

功能相当简单。它的目标是为我们提供一个事件，让我们知道何时动态导入资源。

onElementVisible util function

当你添加这个函数到你的 util 文件时，懒人加载资源就变得轻而易举了！我保证你会上瘾的！

让我们看几个如何使用它的例子。

## 延迟加载图像

Lazy-load images example

## 惰性加载库

让我们假设产品所有者想要在产品页面中添加交叉销售滑块部分。作为前端开发人员，您选择或已经使用库 Swiper。这个库很重，你想让它慢加载。

Lazy-load library Swiper

## 延迟加载 JS 框架

是的，如果你在页面的一部分使用，你也可以动态地加载一个 JS 框架。让我们假设您添加了一个用 JS 框架(VueJS，React…)构建的时事通讯登录功能:

**带 VueJS**

Vue lazy-load

**同反应过来的**

React lazy-load

## 更深入的提示

为了更有效地使用延迟加载功能，我给你一些提示或建议。

*   **显示回退内容**向您的用户提示将要显示的内容
*   **为目标 DOM 元素设置预期尺寸**以避免裁剪
*   **在你最喜欢的浏览器中查看网络部分的输出文件**,看看你从初始网页加载中节省了多少字节
*   **使用覆盖率浏览器工具**来识别可以延迟加载的大量资源

# 结论

在产品负责人与前端开发人员和 UX/UI 团队的互动中，应该始终考虑 Web 性能。这个想法是提供尽可能平滑的用户界面，并向用户隐藏功能的复杂性。这就是为什么我认为 util 这样的功能会帮助你达到这个目标。