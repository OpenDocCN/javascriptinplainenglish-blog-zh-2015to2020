# 引导程序 5 —下拉自定义

> 原文：<https://javascript.plainenglish.io/bootstrap-5-dropdown-customization-30f3a550168a?source=collection_archive---------11----------------------->

![](img/00681a5eb447935a131ebc9245120332.png)

Photo by [Nick Fewings](https://unsplash.com/@jannerboy62?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

**写这篇文章时，Bootstrap 5 处于 alpha 状态，可能会更改。**

Bootstrap 是任何 JavaScript 应用程序的流行 UI 库。

在本文中，我们将看看如何使用 Bootstrap 5 定制下拉菜单。

# 向左下拉

我们可以添加`dropleft`类使菜单显示在左边。

箭头也将指向左侧。

例如，我们可以写:

```
<div class="btn-group dropleft">
  <button class="btn btn-secondary dropdown-toggle" type="button" data-toggle="dropdown">
    button
  </button>
  <ul class="dropdown-menu">
    <li><a class="dropdown-item" href="#">foo</a></li>
    <li><a class="dropdown-item" href="#">bar</a></li>
    <li><a class="dropdown-item" href="#">baz</a></li>
  </ul>
</div>
```

我们将`dropleft`类添加到外部 div，使箭头显示在按钮的左侧。

此外，菜单将显示在按钮的左侧。

对于拆分按钮，我们必须将箭头放在右边，而不是左边:

```
<div class="btn-group dropleft">
  <button type="button" class="btn btn-secondary dropdown-toggle dropdown-toggle-split" data-toggle="dropdown">
    <span class="sr-only">Toggle Dropdown</span>
  </button>
  <button type="button" class="btn btn-secondary">
    Split
  </button>
  <ul class="dropdown-menu">
    <li><a class="dropdown-item" href="#">foo</a></li>
    <li><a class="dropdown-item" href="#">bar</a></li>
    <li><a class="dropdown-item" href="#">baz</a></li>
  </ul>
</div>
```

# 菜单项

在旧版本的 Bootstrap 中，菜单项必须是链接。

使用 Bootstrap 5，项目也可以是按钮。

例如，我们可以写:

```
<div class="btn-group">
  <button class="btn btn-secondary dropdown-toggle" type="button" data-toggle="dropdown">
    button
  </button>
  <ul class="dropdown-menu">
    <li><button class="dropdown-item" type="button">foo</button></li>
    <li><button class="dropdown-item" type="button">bar</button></li>
    <li><button class="dropdown-item" type="button">baz</button></li>
  </ul>
</div>
```

我们有 li 元素，它们有按钮而不是链接。

# 活跃的

我们可以用`.active`类将一个项目设置为活动的。

例如，我们可以写:

```
<div class="btn-group">
  <button class="btn btn-secondary dropdown-toggle" type="button" data-toggle="dropdown">
    button
  </button>
  <ul class="dropdown-menu">
    <li><button class="dropdown-item active" type="button">foo</button></li>
    <li><button class="dropdown-item" type="button">bar</button></li>
    <li><button class="dropdown-item" type="button">baz</button></li>
  </ul>
</div>
```

使用`active`类，该项目将被高亮显示。

# 有缺陷的

要禁用一个项目，我们可以添加`disabled`类:

```
<div class="btn-group">
  <button class="btn btn-secondary dropdown-toggle" type="button" data-toggle="dropdown">
    button
  </button>
  <ul class="dropdown-menu">
    <li><button class="dropdown-item disabled" type="button">foo</button></li>
    <li><button class="dropdown-item" type="button">bar</button></li>
    <li><button class="dropdown-item" type="button">baz</button></li>
  </ul>
</div>
```

现在我们不能与禁用的项目进行交互，它是灰色的。

# 菜单对齐

我们可以改变菜单的排列方式。

要改变它，我们可以将`.dropdown-menu-right`类添加到`.dropdown-menu`类中来右对齐菜单。

例如，我们可以写:

```
<div class="btn-group">
  <button class="btn btn-secondary dropdown-toggle" type="button" data-toggle="dropdown">
    button
  </button>
  <ul class="dropdown-menu dropdown-menu-right">
    <li><button class="dropdown-item" type="button">foo</button></li>
    <li><button class="dropdown-item" type="button">bar</button></li>
    <li><button class="dropdown-item" type="button">baz</button></li>
  </ul>
</div>
```

我们有`dropdown-menu`和`dropdown-menu-right`类来将它的对齐移动到右边。

# 响应对准

我们可以通过添加`data-display='static'`属性来禁用动态定位。

例如，我们可以写:

```
<div class="btn-group">
  <button class="btn btn-secondary dropdown-toggle" type="button" data-toggle="dropdown" data-display="static">
    button
  </button>
  <ul class="dropdown-menu dropdown-menu-lg-right">
    <li><button class="dropdown-item" type="button">foo</button></li>
    <li><button class="dropdown-item" type="button">bar</button></li>
    <li><button class="dropdown-item" type="button">baz</button></li>
  </ul>
</div>
```

我们添加了`data-display`属性。

我们添加了`dropdown-menu-lg-right`来正确显示菜单。

这将使菜单向左对齐。

我们也可以用`dropdown-menu-lg-left`类将其向左对齐:

```
<div class="btn-group">
  <button class="btn btn-secondary dropdown-toggle" type="button" data-toggle="dropdown" data-display="static">
    button
  </button>
  <ul class="dropdown-menu dropdown-menu-lg-left">
    <li><button class="dropdown-item" type="button">foo</button></li>
    <li><button class="dropdown-item" type="button">bar</button></li>
    <li><button class="dropdown-item" type="button">baz</button></li>
  </ul>
</div>
```

# 菜单内容

我们可以在菜单上添加各种各样的内容。

# 头球

我们可以添加一个带有`dropdown-header`类的头:

```
<div class="btn-group">
  <button class="btn btn-secondary dropdown-toggle" type="button" data-toggle="dropdown" data-display="static">
    button
  </button>
  <ul class="dropdown-menu dropdown-menu-lg-left">
    <li>
      <h6 class="dropdown-header">Dropdown header</h6>
    </li>
    <li><button class="dropdown-item" type="button">foo</button></li>
    <li><button class="dropdown-item" type="button">bar</button></li>
    <li><button class="dropdown-item" type="button">baz</button></li>
  </ul>
</div>
```

# 圆规

同样，我们可以用`dropdown-divider`类添加一个分割器:

```
<div class="btn-group">
  <button class="btn btn-secondary dropdown-toggle" type="button" data-toggle="dropdown" data-display="static">
    button
  </button>
  <ul class="dropdown-menu dropdown-menu-lg-left">
    <li><button class="dropdown-item" type="button">foo</button></li>
    <li><button class="dropdown-item" type="button">bar</button></li>
    <li>
      <hr class="dropdown-divider">
    </li>
    <li><button class="dropdown-item" type="button">baz</button></li>
  </ul>
</div>
```

我们会在项目之间看到一条空行。

![](img/f9b26d9d3a9be07c23484333228ec009.png)

Photo by [Laiton Barbo](https://unsplash.com/@laitonbarbo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用许多功能来定制菜单。

我们可以添加按钮作为项目。

也可以添加标题和分隔线。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**