# Bootstrap 5 — Flexbox 利润、订购和包装

> 原文：<https://javascript.plainenglish.io/bootstrap-5-flexbox-margins-ordering-and-wrapping-d5cb869e257f?source=collection_archive---------5----------------------->

![](img/05c01f7a2f32eb25c6dec98fc64b3ea1.png)

Photo by [Gary Sandoz](https://unsplash.com/@gala_san?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

**写这篇文章时，Bootstrap 5 处于 alpha 状态，可能会更改。**

Bootstrap 是任何 JavaScript 应用程序的流行 UI 库。

在本文中，我们将了解如何使用 Bootstrap 5 添加边距、排序和包装。

# 自动边距

我们可以用`.mr-auto`添加右边距，用`.ml-auto`添加左边距。

例如，我们可以写:

```
<div class="d-flex bd-highlight mb-3">
  <div class="p-2 bd-highlight">Flex item</div>
  <div class="p-2 bd-highlight">Flex item</div>
  <div class="p-2 bd-highlight">Flex item</div>
</div>

<div class="d-flex bd-highlight mb-3">
  <div class="mr-auto p-2 bd-highlight">Flex item</div>
  <div class="p-2 bd-highlight">Flex item</div>
  <div class="p-2 bd-highlight">Flex item</div>
</div>

<div class="d-flex bd-highlight mb-3">
  <div class="p-2 bd-highlight">Flex item</div>
  <div class="p-2 bd-highlight">Flex item</div>
  <div class="ml-auto p-2 bd-highlight">Flex item</div>
</div>
```

第一个 div 将所有内容向左对齐。

第二个 div 的第一个 div 有右边距，因此它后面的两个 div 在右边。

第三个 div 有最后一个添加了`ml-auto`类的孩子，所以最后一个 div 在右边，其余的在左边。

# 带对齐项目

我们也可以垂直对齐项目。

Bootstrap 5 附带了`mb-auto`和`mt-auto`类来进行比对。

例如，我们可以写:

```
<div class="d-flex align-items-start flex-column bd-highlight mb-3" style="height: 200px;">
  <div class="mb-auto p-2 bd-highlight">item</div>
  <div class="p-2 bd-highlight">item</div>
  <div class="p-2 bd-highlight">item</div>
</div>

<div class="d-flex align-items-end flex-column bd-highlight mb-3" style="height: 200px;">
  <div class="p-2 bd-highlight">item</div>
  <div class="p-2 bd-highlight">item</div>
  <div class="mt-auto p-2 bd-highlight">item</div>
</div>
```

进行校准。

我们使用`mb-auto`类将跟随它的 div 推到底部。

我们使用`mt-auto`类在底部 div 和它上面的 2 之间添加空间。

# 包装

我们可以用不同的类包装项目。

它们包括`flex-nowrap`和`flex-wrap`类。

`.flex-nowrap`禁用浏览器默认的换行。

`.flex-wrap`启用包装。

我们可以用`.flex-wrap-reverse`反向包装物品。

例如，我们可以写:

```
<div class="d-flex flex-nowrap">
  ...
</div>
<div class="d-flex flex-wrap">
  ...
</div>
<div class="d-flex flex-wrap-reverse">
  ...
</div>
```

来添加它们。

第一个 div 不会包装孩子。

第二个 div 包装孩子。

最后把它们反过来包装。

这些响应变化也是可用的:

*   `.flex-nowrap`
*   `.flex-wrap`
*   `.flex-wrap-reverse`
*   `.flex-sm-nowrap`
*   `.flex-sm-wrap`
*   `.flex-sm-wrap-reverse`
*   `.flex-md-nowrap`
*   `.flex-md-wrap`
*   `.flex-md-wrap-reverse`
*   `.flex-lg-nowrap`
*   `.flex-lg-wrap`
*   `.flex-lg-wrap-reverse`
*   `.flex-xl-nowrap`
*   `.flex-xl-wrap`
*   `.flex-xl-wrap-reverse`
*   `.flex-xxl-nowrap`
*   `.flex-xxl-wrap`
*   `.flex-xxl-wrap-reverse`

# 命令

项目的视觉顺序可以改变。

Bootstrap 5 附带了`order`类来做这件事。

例如，我们可以写:

```
<div class="d-flex flex-nowrap bd-highlight">
  <div class="order-3 p-2 bd-highlight">First</div>
  <div class="order-2 p-2 bd-highlight">Second</div>
  <div class="order-1 p-2 bd-highlight">Third</div>
</div>
```

改变顺序。

我们也可以写:

*   `.order-0`
*   `.order-1`
*   `.order-2`
*   `.order-3`
*   `.order-4`
*   `.order-5`
*   `.order-sm-0`
*   `.order-sm-1`
*   `.order-sm-2`
*   `.order-sm-3`
*   `.order-sm-4`
*   `.order-sm-5`
*   `.order-md-0`
*   `.order-md-1`
*   `.order-md-2`
*   `.order-md-3`
*   `.order-md-4`
*   `.order-md-5`
*   `.order-lg-0`
*   `.order-lg-1`
*   `.order-lg-2`
*   `.order-lg-3`
*   `.order-lg-4`
*   `.order-lg-5`
*   `.order-xl-0`
*   `.order-xl-1`
*   `.order-xl-2`
*   `.order-xl-3`
*   `.order-xl-4`
*   `.order-xl-5`
*   `.order-xxl-0`
*   `.order-xxl-1`
*   `.order-xxl-2`
*   `.order-xxl-3`
*   `.order-xxl-4`
*   `.order-xxl-5`

这些类也可用于使项目成为第一个或最后一个项目:

*   `.order-first`
*   `.order-last`
*   `.order-sm-first`
*   `.order-sm-last`
*   `.order-md-first`
*   `.order-md-last`
*   `.order-lg-first`
*   `.order-lg-last`
*   `.order-xl-first`
*   `.order-xl-last`
*   `.order-xxl-first`
*   `.order-xxl-last`

![](img/885ea35273a30f8a17e5c2e0e97b115a.png)

Photo by [Sankhadeep Barman](https://unsplash.com/@sankhadeep_5300?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用 Bootstrap 5 类添加边距和对齐项目。

还有用于元素排序的类。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**