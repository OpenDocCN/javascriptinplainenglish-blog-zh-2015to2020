# 引导 5 —扩展输入组

> 原文：<https://javascript.plainenglish.io/bootstrap-5-extending-input-groups-91d79af30936?source=collection_archive---------13----------------------->

![](img/27ac75f174720764483a9d6ed55c6408.png)

Photo by [Gerard Richard](https://unsplash.com/@gerardtrichard?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

写这篇文章时，Bootstrap 5 处于 alpha 状态，可能会发生变化。

Bootstrap 是任何 JavaScript 应用程序的流行 UI 库。

在本文中，我们将看看如何用 Bootstrap 5 设计输入组的样式。

# 多端输入

输入组中可以添加多个输入。

例如，我们可以写:

```
<div class="input-group">
  <span class="input-group-text">First and last name</span>
  <input type="text" class="form-control">
  <input type="text" class="form-control">
</div>
```

在一个输入组中添加名字和姓氏输入。

# 多个插件

一个输入组可以添加多个插件。

例如，我们可以写:

```
<div class="input-group mb-3">
  <span class="input-group-text">$</span>
  <span class="input-group-text">0.00</span>
  <input type="text" class="form-control">
</div><div class="input-group">
  <input type="text" class="form-control">
  <span class="input-group-text">$</span>
  <span class="input-group-text">0.00</span>
</div>
```

我们用`input-group-text`类增加了 2 个输入插件。

# 按钮插件

按钮可以作为输入组插件添加。

例如，我们可以写:

```
<div class="input-group mb-3">
  <button class="btn btn-outline-secondary" type="button">Button</button>
  <input type="text" class="form-control" placeholder="name">
</div>
```

我们在`input-group`上直接添加了一个按钮，作为插件使用。

我们还可以在右侧添加一个按钮:

```
<div class="input-group mb-3">
  <input type="text" class="form-control" placeholder="name">
  <button class="btn btn-outline-secondary" type="button" id="button-addon2">Button</button>
</div>
```

此外，我们可以添加多个插件。

例如，我们可以写:

```
<div class="input-group mb-3">
  <button class="btn btn-outline-secondary" type="button">Button</button>
  <button class="btn btn-outline-secondary" type="button">Button</button>
  <input type="text" class="form-control" placeholder="name">
</div><div class="input-group">
  <input type="text" class="form-control" placeholder="name">
  <button class="btn btn-outline-secondary" type="button">Button</button>
  <button class="btn btn-outline-secondary" type="button">Button</button>
</div>
```

# 带有下拉菜单的按钮

下拉菜单可以作为输入组插件添加。

下拉菜单需要引导 JavaScript 文件。

例如，我们可以写:

```
<div class="input-group mb-3">
  <button class="btn btn-outline-secondary dropdown-toggle" type="button" data-toggle="dropdown" aria-expanded="false">fruit</button>
  <ul class="dropdown-menu">
    <li><a class="dropdown-item" href="#">apple</a></li>
    <li><a class="dropdown-item" href="#">orange</a></li>
    <li><a class="dropdown-item" href="#">lemon</a></li>
    <li><hr class="dropdown-divider"></li>
    <li><a class="dropdown-item" href="#">grape</a></li>
  </ul>
  <input type="text" class="form-control">
</div>
```

向左侧添加下拉列表。

我们添加了`dropdown-toggle`类来创建切换。

我们有`data-toggle`属性使它成为一个开关。

要在右边加一个，我们可以写:

```
<div class="input-group mb-3">
  <input type="text" class="form-control">
  <button class="btn btn-outline-secondary dropdown-toggle" type="button" data-toggle="dropdown" aria-expanded="false">fruit</button>
  <ul class="dropdown-menu">
    <li><a class="dropdown-item" href="#">apple</a></li>
    <li><a class="dropdown-item" href="#">orange</a></li>
    <li><a class="dropdown-item" href="#">lemon</a></li>
    <li>
      <hr class="dropdown-divider">
    </li>
    <li><a class="dropdown-item" href="#">grape</a></li>
  </ul>
</div>
```

我们可以在两边添加下拉菜单:

```
<div class="input-group mb-3">
  <button class="btn btn-outline-secondary dropdown-toggle" type="button" data-toggle="dropdown" aria-expanded="false">fruit</button>
  <ul class="dropdown-menu">
    <li><a class="dropdown-item" href="#">apple</a></li>
    <li><a class="dropdown-item" href="#">orange</a></li>
    <li><a class="dropdown-item" href="#">lemon</a></li>
    <li>
      <hr class="dropdown-divider">
    </li>
    <li><a class="dropdown-item" href="#">grape</a></li>
  </ul>
  <input type="text" class="form-control">
  <button class="btn btn-outline-secondary dropdown-toggle" type="button" data-toggle="dropdown" aria-expanded="false">fruit</button>
  <ul class="dropdown-menu">
    <li><a class="dropdown-item" href="#">apple</a></li>
    <li><a class="dropdown-item" href="#">orange</a></li>
    <li><a class="dropdown-item" href="#">lemon</a></li>
    <li>
      <hr class="dropdown-divider">
    </li>
    <li><a class="dropdown-item" href="#">grape</a></li>
  </ul>
</div>
```

# 分段按钮

我们可以将下拉按钮分段。

为此，我们可以写:

```
<div class="input-group mb-3">
  <button type="button" class="btn btn-outline-secondary">fruit</button>
  <button type="button" class="btn btn-outline-secondary dropdown-toggle dropdown-toggle-split" data-toggle="dropdown">
    <span class="sr-only">Toggle Dropdown</span>
  </button>
  <ul class="dropdown-menu">
    <li><a class="dropdown-item" href="#">apple</a></li>
    <li><a class="dropdown-item" href="#">orange</a></li>
    <li><a class="dropdown-item" href="#">lemon</a></li>
    <li>
      <hr class="dropdown-divider">
    </li>
    <li><a class="dropdown-item" href="#">banana</a></li>
  </ul>
  <input type="text" class="form-control">
</div>
```

我们通过并排添加 2 个按钮来添加分离切换按钮。

我们有`dropdown-toggle`和`dropdown-toggle-split`类来添加分割下拉按钮。

此外，我们可以将拆分按钮和下拉菜单放在右侧:

```
<div class="input-group mb-3">
  <input type="text" class="form-control">
  <button type="button" class="btn btn-outline-secondary">fruit</button>
  <button type="button" class="btn btn-outline-secondary dropdown-toggle dropdown-toggle-split" data-toggle="dropdown">
    <span class="sr-only">Toggle Dropdown</span>
  </button>
  <ul class="dropdown-menu">
    <li><a class="dropdown-item" href="#">apple</a></li>
    <li><a class="dropdown-item" href="#">orange</a></li>
    <li><a class="dropdown-item" href="#">lemon</a></li>
    <li>
      <hr class="dropdown-divider">
    </li>
    <li><a class="dropdown-item" href="#">banana</a></li>
  </ul>
</div>
```

![](img/89bd98f2eada3e67c92e6966d91a4dad.png)

Photo by [Mae Mu](https://unsplash.com/@picoftasty?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以在一个输入组中添加多个插件、输入和下拉菜单。

为了显示下拉列表，我们需要 JavaScript 文件。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**