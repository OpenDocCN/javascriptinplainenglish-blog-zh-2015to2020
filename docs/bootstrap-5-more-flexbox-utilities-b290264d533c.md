# 引导数据库 5-更多 Flexbox 实用程序

> 原文：<https://javascript.plainenglish.io/bootstrap-5-more-flexbox-utilities-b290264d533c?source=collection_archive---------13----------------------->

![](img/3f6f3a210982acb0cbd57ba7dc123a6a.png)

Photo by [Conor Brown](https://unsplash.com/@commonboxturtle?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

**写入时，Bootstrap 5 处于 alpha 状态，可能会发生变化。**

Bootstrap 是任何 JavaScript 应用程序的流行用户界面库。

在本文中，我们将了解如何在 Bootstrap 5 中使用 flexbox 类。

# 对齐项目

我们可以使用`align-items`类来改变伸缩项的对齐方式。

这些项目将与这些类垂直对齐。

选项包括`start`、`end`、`center`、`baseline`或`stretch`。

例如，我们可以写:

```
<div class="d-flex align-items-start">...</div>
<div class="d-flex align-items-end">...</div>
<div class="d-flex align-items-center">...</div>
<div class="d-flex align-items-baseline">...</div>
<div class="d-flex align-items-stretch">...</div>
```

`align-items-start`将把 div 里面的物品放在最上面。

`align-items-end`将物品放在底部。

`align-items-center`垂直居中。

`align-items-baseline`将其置于最高基线。

`align-items-stretch`将物品从上到下拉伸。

其他课程选择包括:

*   `.align-items-start`
*   `.align-items-end`
*   `.align-items-center`
*   `.align-items-baseline`
*   `.align-items-stretch`
*   `.align-items-sm-start`
*   `.align-items-sm-end`
*   `.align-items-sm-center`
*   `.align-items-sm-baseline`
*   `.align-items-sm-stretch`
*   `.align-items-md-start`
*   `.align-items-md-end`
*   `.align-items-md-center`
*   `.align-items-md-baseline`
*   `.align-items-md-stretch`
*   `.align-items-lg-start`
*   `.align-items-lg-end`
*   `.align-items-lg-center`
*   `.align-items-lg-baseline`
*   `.align-items-lg-stretch`
*   `.align-items-xl-start`
*   `.align-items-xl-end`
*   `.align-items-xl-center`
*   `.align-items-xl-baseline`
*   `.align-items-xl-stretch`
*   `.align-items-xxl-start`
*   `.align-items-xxl-end`
*   `.align-items-xxl-center`
*   `.align-items-xxl-baseline`
*   `.align-items-xxl-stretch`

# 自我对齐

`align-self`实用程序类让我们单独改变横轴上的对齐。

选择包括`start`、`end`、`center`、`baseline`或`stretch`。

`stretch`是默认选择。

`start`将项目对齐顶部。

`end`将项目底部对齐。

`center`垂直对齐项目。

`baseline`将项目与顶部的基线对齐。

`stretch`从上到下拉伸内容。

我们可以在单个元素中使用它们:

```
<div class="align-self-start">flex item</div>
<div class="align-self-end"> flex item</div>
<div class="align-self-center">flex item</div>
<div class="align-self-baseline">flex item</div>
<div class="align-self-stretch"> flex item</div>
```

其他选择包括:

*   `.align-self-start`
*   `.align-self-end`
*   `.align-self-center`
*   `.align-self-baseline`
*   `.align-self-stretch`
*   `.align-self-sm-start`
*   `.align-self-sm-end`
*   `.align-self-sm-center`
*   `.align-self-sm-baseline`
*   `.align-self-sm-stretch`
*   `.align-self-md-start`
*   `.align-self-md-end`
*   `.align-self-md-center`
*   `.align-self-md-baseline`
*   `.align-self-md-stretch`
*   `.align-self-lg-start`
*   `.align-self-lg-end`
*   `.align-self-lg-center`
*   `.align-self-lg-baseline`
*   `.align-self-lg-stretch`
*   `.align-self-xl-start`
*   `.align-self-xl-end`
*   `.align-self-xl-center`
*   `.align-self-xl-baseline`
*   `.align-self-xl-stretch`
*   `.align-self-xxl-start`
*   `.align-self-xxl-end`
*   `.align-self-xxl-center`
*   `.align-self-xxl-baseline`
*   `.align-self-xxl-stretch`

# 充满

`.flex-fill`类可以用在兄弟元素上，强制它们的宽度等于它们的内容，同时占用所有可用的水平空间。

例如，我们可以写:

```
<div class="d-flex bd-light">
  <div class="p-2 flex-fill bd-light">Lorem ipsum dolor sit amet</div>
  <div class="p-2 flex-fill bd-light">Lorem ipsum</div>
  <div class="p-2 flex-fill bd-light">Lorem ipsum</div>
</div>
```

我们让 div 填满容器的空间。

其他选择包括:

*   `.flex-fill`
*   `.flex-sm-fill`
*   `.flex-md-fill`
*   `.flex-lg-fill`
*   `.flex-xl-fill`
*   `.flex-xxl-fill`

# 增长和收缩

Bootstrap 5 具有用于增长和收缩项目的 flexbox 类。

有`.flex-grow-*`实用程序类来做这件事。

我们可以用它来让一个元素填满所有可用的空间。

例如，我们可以写:

```
<div class="d-flex bd-highlight">
  <div class="p-2 flex-grow-1 bd-highlight">lorem ipsum</div>
  <div class="p-2 bd-highlight">lorem ipsum</div>
  <div class="p-2 bd-highlight">lorem ipsum</div>
</div>
```

现在第一个 div 填充了所有的可用空间，因为我们对它应用了`flex-grow-1`类。

![](img/537482a56ebaf842db30dc014eb95d78.png)

Photo by [Jeremy Thomas](https://unsplash.com/@jeremythomasphoto?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用 Bootstrap 5 类将 flexbox 属性添加到我们的元素中，这样我们就不必自己从头开始编写它们了。

## **简单英语的 JavaScript**

你知道我们有四份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)和 [**找到它们订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**