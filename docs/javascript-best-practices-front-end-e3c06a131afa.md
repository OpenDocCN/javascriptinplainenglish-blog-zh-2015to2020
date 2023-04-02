# JavaScript 最佳实践—前端

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-front-end-e3c06a131afa?source=collection_archive---------6----------------------->

![](img/c34c284ffcf61f7ccf8fdd4a015ab335.png)

Photo by [Johann Siemens](https://unsplash.com/@johannsiemens?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究一些在编写 JavaScript 代码时应该遵循的最佳实践。

# 事件委托

我们可以通过一个连接到父元素的事件监听器监听来自所有子元素的事件。

例如，我们可以写:

```
document.addEventListener('click', (event) => {
  const {
    currentTarget
  } = event;
  let {
    target
  } = event; if (currentTarget && target) {
    if (target.nodeName !== 'LI') {
      while (currentTarget.contains(target)) {
        target = target.parentNode;
      }
    }
  }
});
```

我们可以在事件侦听器中检查哪个元素发出了事件。

所有数据都在`event`对象中。

`currentTarget`具有现在传播事件的元素。

`target`拥有发出事件的原始元素。

# 去抖、节流和请求动画帧

去抖动一个函数可以防止它在给定的时间过后被再次调用。

我们可以用 Lodash 的`debounce`方法轻松做到这一点。

节流只会导致函数被调用最大次数。

洛达什也有一个`throttle`方法。

`requestAnimationFrame`让我们在运行动画时将速度调到 60fps。

# 客户端数据

我们可以使用 Fetch API 在客户端获取数据。

这是一个基于承诺的 API，允许我们在客户端应用程序上发出 HTTP 请求。

如果它不可用，我们可以为它添加一个聚合填充物。

# 何时使用客户端数据请求库

我们可以使用客户端请求库来发出可取消的请求、添加超时或获取请求进度。

有了 Axios 等第三方库，一切都变得更容易。

它们还具有转换器、拦截器和 XSRF 保护，可以应用于所有请求。

我们还可以创建添加错误处理、cookie、CORS 等功能的助手函数。

# 串联请求

GraphQL 是一种不发送 JSON 的 HTTP 请求的替代方法。

相反，它有自己的查询语言来通过 HTTP 发出请求。

这是一个值得考虑的选择，因为它有类型检查，并让我们有选择地按字段获取数据。

这意味着通话更有针对性，效率更高。

# 单元和集成测试

自动化测试让我们的生活更加轻松。

我们可以编写单元测试来测试一些表面下的功能。

集成测试测试更多的功能。

但是，目视检查仍然需要手动完成。

# 图书馆

我们需要经常更新我们的图书馆，使它们保持最新。

它们通常有安全补丁和新功能，让我们的生活更轻松。

此外，实用程序库使我们的生活更加轻松。他们有很多辅助功能来帮助我们。

例如，Lodash 和下划线有很多类似于`clone`、`each`、`chunk`等等的东西。

# 结构

框架帮助我们更快地开发。

我们可以使用像 React 和 Vue 这样的流行工具，它们有很多库，可以帮助我们更快地构建应用程序。

# 饼干

除了 cookie 之外，我们还可以使用具有唯一标识符的本地存储来确保拥有 cookie 的用户确实是应该拥有它的用户。

![](img/2a8cc62c1fad23b58fbba4b581548889.png)

Photo by [Christina Branco](https://unsplash.com/@starvingartistfoodphotography?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

有很多事情我们可能没有考虑到。

我们可以通过更多的跟踪来保护 cookies。

许多图书馆使我们的生活更容易。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**