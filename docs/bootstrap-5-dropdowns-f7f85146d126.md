# 引导程序 5 —下拉菜单

> 原文：<https://javascript.plainenglish.io/bootstrap-5-dropdowns-f7f85146d126?source=collection_archive---------4----------------------->

![](img/0189cf093b722963cfb69c5a73dd787f.png)

Photo by [Herbert Goetsch](https://unsplash.com/@hg_photo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

**写这篇文章时，Bootstrap 5 处于 alpha 状态，可能会更改。**

Bootstrap 是任何 JavaScript 应用程序的流行 UI 库。

在本文中，我们将看看如何用 Bootstrap 5 添加下拉菜单。

# 下拉菜单

下拉列表是显示项目列表的可切换覆盖图。

Bootstrap 5 的实现依赖于 Popper.js。

例如，我们可以写:

```
<div class="dropdown">
  <button class="btn btn-secondary dropdown-toggle" type="button" id="dropdownMenuButton" data-toggle="dropdown">
    Dropdown button
  </button>
  <ul class="dropdown-menu">
    <li><a class="dropdown-item" href="#">foo</a></li>
    <li><a class="dropdown-item" href="#">bar</a></li>
    <li><a class="dropdown-item" href="#">baz</a></li>
  </ul>
</div>
```

我们有带`dropdown`类的 div。

该按钮让我们可以打开和关闭下拉菜单。

还需要`data-toggle`属性来使按钮成为切换按钮。

它有`dropdown-toggle`类来添加切换。

然后我们添加带有`dropdown-menu`类的`ul`来添加列表。

列表项具有`dropdown-item`类。

按钮可以用`a`元件代替。

例如，我们可以写:

```
<div class="dropdown">
  <a class="btn btn-secondary dropdown-toggle" href="#" data-toggle="dropdown">
    Dropdown link
  </a>
  <ul class="dropdown-menu">
    <li><a class="dropdown-item" href="#">foo</a></li>
    <li><a class="dropdown-item" href="#">bar</a></li>
    <li><a class="dropdown-item" href="#">baz</a></li>
  </ul>
</div>
```

我们用`a`元素代替了`dropdown-toggle`类，而不是按钮。

此外，我们可以改变按钮的变体。

例如，我们可以写:

```
<div class="dropdown">
  <a class="btn btn-danger dropdown-toggle" href="#" data-toggle="dropdown">
    Dropdown link
  </a>
  <ul class="dropdown-menu">
    <li><a class="dropdown-item" href="#">foo</a></li>
    <li><a class="dropdown-item" href="#">bar</a></li>
    <li><a class="dropdown-item" href="#">baz</a></li>
  </ul>
</div>
```

将按钮的类别更改为`.btn-danger`。

# 拆分按钮

下拉按钮可以是拆分按钮。

要添加它，我们添加两个按钮:

```
<div class="btn-group">
  <button type="button" class="btn btn-danger">Action</button>
  <button type="button" class="btn btn-danger dropdown-toggle dropdown-toggle-split" data-toggle="dropdown">
    <span class="sr-only">Toggle Dropdown</span>
  </button>
  <ul class="dropdown-menu">
    <li><a class="dropdown-item" href="#">foo</a></li>
    <li><a class="dropdown-item" href="#">bar</a></li>
    <li><a class="dropdown-item" href="#">baz</a></li>
  </ul>
</div>
```

我们在外部 div 中使用了`.btn-group`类，而不是`dropdown`类。

在 div 中，我们有两个按钮。

一个带文本，一个带箭头。

它有`dropdown-toggle`类和`data-toggle`属性，所以我们可以用它来切换菜单。

此外，我们有`dropdown-toggle-split`使按钮与左按钮对齐。

# 胶料

按钮下拉按钮的大小可以改变。

例如，我们可以使用`btn-lg`类使其变大:

```
<div class="btn-group">
  <button class="btn btn-secondary btn-lg dropdown-toggle" type="button" data-toggle="dropdown" aria-expanded="false">
    Large button
  </button>
  <ul class="dropdown-menu">
    <li><a class="dropdown-item" href="#">foo</a></li>
    <li><a class="dropdown-item" href="#">bar</a></li>
    <li><a class="dropdown-item" href="#">baz</a></li>
  </ul>
</div>
```

我们还可以添加`btn-sm`类使它变小:

```
<div class="btn-group">
  <button class="btn btn-secondary btn-sm dropdown-toggle" type="button" data-toggle="dropdown">
    Large button
  </button>
  <ul class="dropdown-menu">
    <li><a class="dropdown-item" href="#">foo</a></li>
    <li><a class="dropdown-item" href="#">bar</a></li>
    <li><a class="dropdown-item" href="#">baz</a></li>
  </ul>
</div>
```

# 方向

下拉菜单的方向可以改变。

我们可以让它显示在按钮上方。

例如，我们可以使用`.dropup`类通过编写:

```
<div class="btn-group dropup">
  <button class="btn btn-secondary dropdown-toggle" type="button" data-toggle="dropdown">
    Large button
  </button>
  <ul class="dropdown-menu">
    <li><a class="dropdown-item" href="#">foo</a></li>
    <li><a class="dropdown-item" href="#">bar</a></li>
    <li><a class="dropdown-item" href="#">baz</a></li>
  </ul>
</div>
```

它也适用于拆分按钮:

```
<div class="btn-group dropup">
  <button type="button" class="btn btn-secondary">
    Split dropup
  </button>
  <button type="button" class="btn btn-secondary dropdown-toggle dropdown-toggle-split" data-toggle="dropdown" aria-expanded="false">
    <span class="sr-only">Toggle Dropdown</span>
  </button>
  <ul class="dropdown-menu">
    <li><a class="dropdown-item" href="#">foo</a></li>
    <li><a class="dropdown-item" href="#">bar</a></li>
    <li><a class="dropdown-item" href="#">baz</a></li>
  </ul>
</div>
```

# 向右下拉

我们可以添加`.dropright`类使菜单显示在右边。

例如，我们可以写:

```
<div class="btn-group dropright">
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

使右箭头显示在文本旁边，并在右侧显示菜单。

对于拆分按钮，我们可以写:

```
<div class="btn-group dropright">
  <button type="button" class="btn btn-secondary">
    Split
  </button>
  <button type="button" class="btn btn-secondary dropdown-toggle dropdown-toggle-split" data-toggle="dropdown" aria-expanded="false">
    <span class="sr-only">Toggle Dropdown</span>
  </button>
  <ul class="dropdown-menu">
    <li><a class="dropdown-item" href="#">foo</a></li>
    <li><a class="dropdown-item" href="#">bar</a></li>
    <li><a class="dropdown-item" href="#">baz</a></li>
  </ul>
</div>
```

用拆分按钮做同样的事情。

![](img/05ef5f015184da406f234da76cf3e61a.png)

Photo by [Randy Bayne](https://unsplash.com/@arbayne?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用 Bootstrap 5 轻松添加下拉列表。

它带有各种定制选项。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅解码得到更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**