# 你真的需要 jQuery 吗？

> 原文：<https://javascript.plainenglish.io/do-you-really-need-jquery-2bd0bbcf1b56?source=collection_archive---------5----------------------->

## jQuery 很棒，无论如何，如果它能让您编写代码的工作变得更容易，就使用它，但是下面是为什么您可能不像自己想象的那样需要它

![](img/87c5a1c41b09e777f3eb5d5d2358c6f9.png)

jQuery is not the same as JavaScript

前端 web 开发是一个快速扩展、永无止境的现象，随着定期更新，jQuery 可能最终会被淘汰。现代浏览器已经实现了大量的 BOM/DOM API，它们是新的行业标准，可以专业地使用。

# 什么是 jQuery？

jQuery 是一个 JavaScript 库，它使得 DOM 操作和事件处理更加容易。它发布于 2006 年初，那时候 DOM API 并不出色。随着 jQuery 的出现，web 开发**发生了革命性的变化**,因为它是一个易于使用、简单且易于理解的 JavaScript 库。

在某一点上，人们确实认为不用 jQuery 编写 JavaScript 是一项非常痛苦的任务，这两个术语几乎成了同义词，在现代场景中不再适用。我们必须接受这样一个事实:JavaScript 是一种语言，而 jQuery 是一种 API。随着 DOM/BOM API 越来越新的进步，我们不必为了 DOM 操作或事件处理而从头开始学习 jQuery。

与此同时，由于 React、Angular 和 Vue 等前端库的普及，直接操作 DOM 成为了反模式，因此 jQuery 的使用变得前所未有的重要。

# jQuery 和普通 JavaScript 的比较

下面是一些对比 jQuery 和普通 JavaScript 区别的代码。

## 按 ID 查找元素

## 添加类别

## 查询选择器

## 相对于视口的位置

*参考文献取自* [*此处*](http://youmightnotneedjquery.com/) *。你可以在链接网站上找到更多的例子。*

从上面的案例中，您可以看到，无论我们想使用 jQuery 做什么，我们都可以用普通的 JavaScript 来实现，有时只需更少的代码行。

> **剧透预警！**
> 
> 普通的 JS 选择器每次都比 jQuery 选择器快。

# 你可能还想用它

jQuery 是在浏览器不标准的时候几乎必不可少的工具之一，但是浏览器在很大程度上已经标准化了大多数 API，并且比过去更快地增加了支持。这并不是说 jQuery 仍然没有用武之地，但是你不应该仅仅因为它是 jQuery 就默认为每个项目都使用它。值得一提的是，jQuery 在编写复杂的代码时非常有用，如果用普通语言编写，很容易引入错误。示例:

以上两个代码都是针对就绪状态的。尽管 readyState 属性相对简单，但对于新开发人员来说可能相当复杂。

> readyState 属性返回当前文档的(加载)状态。

另一个例子是:

# 结论

jQuery 是一个很棒的 JavaScript 库。毫无疑问。然而，说一个人不学习 jQuery 就不能编写 JavaScript 是完全错误的。当您需要编写非常长的代码时，您最好使用 jQuery，因为它可能提供较短的解决方案。然而，每次需要编写 JavaScript 代码时都使用 jQuery 并不值得推荐。我见过开发人员甚至不做任何事情就将 jQuery 添加到他们的应用程序中。

普通 JavaScript 和 jQuery 一样强大。选择哪一个取决于需要完成什么。