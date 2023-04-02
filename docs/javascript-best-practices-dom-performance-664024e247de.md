# JavaScript 最佳实践— DOM 性能

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-dom-performance-664024e247de?source=collection_archive---------9----------------------->

![](img/675342d2042aeeaa1cc31cd787d60eb2.png)

Photo by [Mazlin Massey](https://unsplash.com/@mazlinrhea?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究一些在编写 JavaScript 代码时应该遵循的最佳实践。

# 与主持人互动过多

我们不应该与任何 DOM 对象进行过多的交互。

它们渲染的很慢，我们应该减少交互的次数。

# 依赖太多

如果我们有太多的依赖，那么我们应该减少它们。

多年来，JavaScript 标准库中增加了许多东西。

因此，我们不必使用第三方依赖来做同样的事情。

例如，Lodash 数组方法可以替换为内置数组方法。

第三方 HTTP 客户端可以替换为 Fetch API。

# 糟糕的事件处理

为了提高事件处理的性能，我们应该正确地使用它们。

我们应该移除不必要的循环，并用 jQuery `unbind`或普通 JavaScript 的`removeEventListener`解除未使用事件处理程序的绑定，以移除它们。

# 无组织代码

为了使我们的生活更容易，我们应该将代码组织成连贯的块。

这样，我们以后可以找到代码。

# 使用 HTTP/2

HTTP/2 是 JavaScript 的最新版本，在原始版本的基础上提供了许多增强功能。

如果我们切换到 HTTP/2 服务器来托管我们的站点，我们将会看到速度的提高。

# 使用指针引用

我们应该在一个变量中捕获我们的 DOM 对象，这样我们就不必在以后需要的时候再从 DOM 中获取它们。

我们只需要用这个变量。

例如，我们可以写:

```
const fooEl = document.querySelector('.foo');
```

用类`foo`获取元素。

# 修剪我们的 HTML

我们可以通过减少页面上的元素来减少需要加载的条目数量。

我们拥有的越少，我们需要加载的就越少，我们的页面就会越快。

# 使用`document.getElementById()`

我们可以用`getElementById`。

通过 ID 获取东西更快，因为在一个页面上只有一个元素具有给定的 ID。

这意味着我们的浏览器不需要检查所有的节点来找到所有的条目。

例如，我们可以写:

```
const button = document.getElementById('window-minimize-button');
```

# 使用`document.querySelector()`

如果我们只想用给定的选择器获取一个元素，我们可以使用`querySelector`用给定的选择器获取 DOM 节点。

在找到具有给定选择器的第一个节点后，浏览器停止查找，这使得查找更快。

例如，我们可以写:

```
const button = document.querySelector('#window-minimize-button');
```

# 批量处理我们的 DOM 更改

我们应该批量修改 DOM，这样就不会重复多次了。

这样，我们的网页会让用户感觉更快。

# 缓冲我们的 DOM

如果我们有可滚动的 div，我们可以使用一个缓冲区从 DOM 中移除当前在视口中不可见的项目。

这将节省内存使用和 DOM 遍历。

有很多库可以帮助我们，比如 React 应用的虚拟化库。

![](img/ca3729e0c8695d1b824aeb4c19008eb7.png)

Photo by [Kayla Farmer](https://unsplash.com/@imagesbykayla?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以应用各种技术来加速 DOM 操作。

这些会对性能产生很大的影响。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**