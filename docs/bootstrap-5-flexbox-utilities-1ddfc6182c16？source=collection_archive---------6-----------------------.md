# 引导程序 5 — Flexbox 实用程序

> 原文：<https://javascript.plainenglish.io/bootstrap-5-flexbox-utilities-1ddfc6182c16?source=collection_archive---------6----------------------->

![](img/adf30be95a23f514dce18cb8ceeaf798.png)

Photo by [Gabrielle Costa](https://unsplash.com/@lbrto?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

**写这篇文章时，Bootstrap 5 处于 alpha 状态，可能会更改。**

Bootstrap 是任何 JavaScript 应用程序的流行 UI 库。

在本文中，我们将了解如何在 Bootstrap 5 中使用 flexbox 类。

# 弯曲

Bootstrap 5 为我们提供了让我们轻松使用 flexbox 的类。

例如，我们可以写:

```
<div class="d-flex p-2 bd-highlight">flexbox container</div>
```

添加`d-flex`道具来制作 div `display: flex`。

`d-flex`级的其他变体包括:

*   `.d-flex`
*   `.d-inline-flex`
*   `.d-sm-flex`
*   `.d-sm-inline-flex`
*   `.d-md-flex`
*   `.d-md-inline-flex`
*   `.d-lg-flex`
*   `.d-lg-inline-flex`
*   `.d-xl-flex`
*   `.d-xl-inline-flex`
*   `.d-xxl-flex`
*   `.d-xxl-inline-flex`

# 方向

我们可以通过添加更多的类来改变 flexbox 布局的方向。

比如我们可以写`.flex-row`水平伸缩。

并且`.flex-row-reverse`将使 flex 布局从右侧开始，而不是从左侧开始。

所以我们可以写:

```
<div class="d-flex flex-row bd-highlight mb-3">
  <div class="p-2 bd-highlight">Flex item 1</div>
  <div class="p-2 bd-highlight">Flex item 2</div>
  <div class="p-2 bd-highlight">Flex item 3</div>
</div>
<div class="d-flex flex-row-reverse bd-highlight">
  <div class="p-2 bd-highlight">Flex item 1</div>
  <div class="p-2 bd-highlight">Flex item 2</div>
  <div class="p-2 bd-highlight">Flex item 3</div>
</div>
```

来添加它们。

第一个 div 将贴在左边缘。

第二个将从右边缘开始并以相反的顺序显示。

我们可以使用`.flex-column`来设置垂直方向。

`.flex-column-reverse`从对面开始垂直方向。

我们可以加上:

```
<div class="d-flex flex-column bd-highlight mb-3">
  <div class="p-2 bd-highlight">Flex item 1</div>
  <div class="p-2 bd-highlight">Flex item 2</div>
  <div class="p-2 bd-highlight">Flex item 3</div>
</div>
<div class="d-flex flex-column-reverse bd-highlight">
  <div class="p-2 bd-highlight">Flex item 1</div>
  <div class="p-2 bd-highlight">Flex item 2</div>
  <div class="p-2 bd-highlight">Flex item 3</div>
</div>
```

还提供了以下内容:

*   `.flex-row`
*   `.flex-row-reverse`
*   `.flex-column`
*   `.flex-column-reverse`
*   `.flex-sm-row`
*   `.flex-sm-row-reverse`
*   `.flex-sm-column`
*   `.flex-sm-column-reverse`
*   `.flex-md-row`
*   `.flex-md-row-reverse`
*   `.flex-md-column`
*   `.flex-md-column-reverse`
*   `.flex-lg-row`
*   `.flex-lg-row-reverse`
*   `.flex-lg-column`
*   `.flex-lg-column-reverse`
*   `.flex-xl-row`
*   `.flex-xl-row-reverse`
*   `.flex-xl-column`
*   `.flex-xl-column-reverse`
*   `.flex-xxl-row`
*   `.flex-xxl-row-reverse`
*   `.flex-xxl-column`
*   `.flex-xxl-column-reverse`

# 对齐内容

我们可以通过使用`justify-content`类来分隔我们的内容。

后缀可以是`end`、`center`、`between`、`around`、`evenly`。

`start`是默认值。项目从左边缘开始。

`end`使项目从右边缘开始。

`center`使它们居中。

`between`让它们显示出来，中间留有空间。

`around`让它们两边都留有空间显示。

`evenly`让它们均匀散开。

例如，我们可以写:

```
<div class="d-flex justify-content-start">...</div>
<div class="d-flex justify-content-end">...</div>
<div class="d-flex justify-content-center">...</div>
<div class="d-flex justify-content-between">...</div>
<div class="d-flex justify-content-around">...</div>
<div class="d-flex justify-content-evenly">...</div>
```

来使用这些类。

其他选择包括:

*   `.justify-content-start`
*   `.justify-content-end`
*   `.justify-content-center`
*   `.justify-content-between`
*   `.justify-content-around`
*   `.justify-content-evenly`
*   `.justify-content-sm-start`
*   `.justify-content-sm-end`
*   `.justify-content-sm-center`
*   `.justify-content-sm-between`
*   `.justify-content-sm-around`
*   `.justify-content-sm-evenly`
*   `.justify-content-md-start`
*   `.justify-content-md-end`
*   `.justify-content-md-center`
*   `.justify-content-md-between`
*   `.justify-content-md-around`
*   `.justify-content-md-evenly`
*   `.justify-content-lg-start`
*   `.justify-content-lg-end`
*   `.justify-content-lg-center`
*   `.justify-content-lg-between`
*   `.justify-content-lg-around`
*   `.justify-content-lg-evenly`
*   `.justify-content-xl-start`
*   `.justify-content-xl-end`
*   `.justify-content-xl-center`
*   `.justify-content-xl-between`
*   `.justify-content-xl-around`
*   `.justify-content-xl-evenly`
*   `.justify-content-xxl-start`
*   `.justify-content-xxl-end`
*   `.justify-content-xxl-center`
*   `.justify-content-xxl-between`
*   `.justify-content-xxl-around`
*   `.justify-content-xxl-evenly`

![](img/cae880a332c38b3993f04329466f067e.png)

Photo by [Fermin Rodriguez Penelas](https://unsplash.com/@ferminrp?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用 flexbox 和 Bootstrap 5 提供的类。

项目可以按照我们的方式与它们对齐。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)和 [**找到它们订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**