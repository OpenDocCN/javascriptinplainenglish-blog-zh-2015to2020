# 应对无限滚动和延迟加载

> 原文：<https://javascript.plainenglish.io/react-infinite-scrolling-and-lazy-loading-171909f92b54?source=collection_archive---------1----------------------->

无限滚动是许多网站用来增强用户体验和性能的概念。在无限滚动中，站点加载一些数据，随着用户继续滚动，越来越多的数据被加载。这一概念节省了时间，因为数据是分部分加载的，并且提高了性能，因为不需要一次渲染所有组件。

**—惰性加载是一种针对在线内容的优化技术，
惰性加载的概念不是像批量加载那样一次性加载整个网页并呈现给用户，而是帮助只加载所需的部分，并延迟剩余部分，直到用户需要它。**

**![](img/c3ca6b079a99cfde2872acdd29abd510.png)**

**Photo by [Eduardo Dutra](https://www.pexels.com/@edwardutra?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/person-in-front-of-laptop-on-brown-wooden-table-2115217/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)**

**在**无限滚动**和**惰性加载**的帮助下，我们可以提高 React 应用的性能和体验。想象一下，您有大约 10，000 个数据对象，每个数据对象必须有一个单独的组件。如果您同时渲染所有组件，React 应用程序一次渲染所有组件将花费大量时间，并且会产生性能问题和对用户体验的负面影响。**

**这就是无限滚动和延迟加载派上用场的地方。使用无限滚动，我们可以限制最初要呈现的组件，随着我们继续滚动，其余的组件可以逐渐呈现。现在，我们可以做一些事情来进一步提高我们的性能和用户体验。例如，该组件还显示带有动态 URL 的图像。从 URL 呈现图像的每个组件都需要时间。与此同时，我们可以显示所有组件的静态图像，而延迟加载在后台加载图像。为此，我们将创建两个组件——列表和图像。列表将实现**无限滚动**，图像将**延迟加载**到我们的列表组件中，以增强我们的应用程序。下面提供了实现的要点示例。**

**让我们开始在应用程序中实现无限滚动和延迟加载。我们将创建一个调用开源 API 的 React 应用程序。API 有一个查询参数页面。每个页面都有一个整数值，并返回一个包含 30 个对象的数组。每个对象都有一个名称和一个图像字段。该应用程序将显示每个人的姓名和图像，以及建筑师和工程师的硬编码值。实现如下所示。**

**为了实现**无限滚动**，我们将使用**滚动事件监听器**。Scroll Event Listener 使我们能够访问窗口和文档高度，即足以计算页面结尾的滚动的当前位置。现在，应用程序将从初始化为值 1 的**页面**开始，当用户到达页面的末尾时，每次**页面**的值将增加+1。现在，一旦页面加载完毕，就会使用 **UseEffect(与 componentDidMount 相同)**进行 API 调用，并为页面 1 提取数据。当用户到达页面末尾时，滚动事件监听器将允许我们使用函数 **handleScroll** 来了解滚动的位置，页面值将递增，数据将被提取用于**后续页面**并显示给用户。**

> **无限滚动将为我们节省大量时间，让 API 一次调用整个页面并一次呈现所有数据，从而提高我们的性能和用户体验。**

**现在，为了实现延迟加载，我们将使用悬念和延迟，这是 React 的新增功能。所以，让我们多了解一下悬疑和懒惰。数据获取的暂停是一个新特性，它允许你使用`<Suspense>`到**声明性地“等待”其他任何东西，包括数据。**本页主要关注数据获取用例，但也可以等待图像、脚本或其他异步工作。悬念让你的组件在渲染之前“等待”一些东西。在我们的应用程序中，我们将使用悬念来等待图像被呈现，同时，我们将使用 fallback 属性向用户显示所有组件的静态通用图像，并且一旦图像被加载，我们的 fallback 组件将被卸载，图像组件将被装载。**

> **延迟加载将会提升我们的用户体验。我们将显示一个普通的图像，而不是什么都不显示，直到图像加载完毕，一旦图像加载完毕，它将显示给用户，而不需要在 UI 端做任何改变。**

**图像组件看起来像这样-**

**所以让我们破解密码-**

*   **因为我们使用了钩子，所以我们使用了 useState 和 useEffect 组件。悬疑是 React 最新加入的创造懒加载的功能。你可以在这里了解更多相关信息——https://reactjs.org/docs/code-splitting.html**
*   **现在，我们正在创建一个简单的图像组件，它将使用 React.lazy 进行延迟加载**
*   **现在，我们将使用 3 种状态—
    **列表项** —从服务器获取的数据数组
    **isfetching** —当我们到达页面末尾时将被设置为 true
    **page** 布尔型—当 isfetching 为 true 并且最初被设置为 1 时，从服务器获取数据的页码增加+1。**
*   ****useEffect()** —就像 componentDidMount 将运行一次，从服务器获取页面 1 的数据，并添加事件监听器 scroll，将其绑定到函数 handleScroll。**
*   ****handleScroll()** —在 handleScroll 中，我们使用一个简单的公式**math . ceil**(window . inner height…..)来确定用户何时进入滚动或页面的结尾。当用户在现代浏览器中使用缩放功能并将 isFetching 设置为 true 时，Math.ceil 非常方便。**
*   **现在，我们使用 useEffect 来确定 **isFetching** 中的任何变化，一旦 isFetching 发生变化并且为真，我们就调用函数 fetchMoreData。**
*   **并且 **fetchMoreData** 调用具有 setTimeOut 函数的 fetchData，仅仅显示过渡，仅此而已。fetchData 将页面增加+1，然后相应地调用 API 并将数据推入 listItems 数组。**
*   **除了悬念之外，返回方法很简单。暂停中的回退将被初始渲染，并且一旦 ImageComponent 被加载，回退将被卸载并且 **ImageComponent** 将被渲染。**

**完整的 Github 代码—[https://Github . com/anupamchaudhary 1117/React-infint-Scroll-And-lazy-Loading](https://github.com/ANUPAMCHAUDHARY1117/React-Infinte-Scroll-And-lazy-Loading)**

# ****简明英语笔记****

**你知道我们推出了一个 YouTube 频道吗？我们制作的每个视频都旨在教给你一些新的东西。点击此处 查看我们，并确保订阅该频道😎**