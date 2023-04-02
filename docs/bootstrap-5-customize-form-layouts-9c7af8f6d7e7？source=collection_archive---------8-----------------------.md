# 引导 5-自定义表单布局

> 原文：<https://javascript.plainenglish.io/bootstrap-5-customize-form-layouts-9c7af8f6d7e7?source=collection_archive---------8----------------------->

![](img/cadfe11b30271c0e887206b1293f6123.png)

Photo by [Ella Olsson](https://unsplash.com/@ellaolsson?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

写这篇文章时，Bootstrap 5 处于 alpha 状态，可能会更改。

Bootstrap 是任何 JavaScript 应用程序的流行 UI 库。

在本文中，我们将了解如何使用 Bootstrap 5 向表单添加布局。

# 水平表单标签尺寸

我们可以用`col-form-label-sm`和`col-form-label-lg`类改变表单字段的大小。

它们可以应用于表单标签和图例。

大小应该匹配表单控件的大小，可以用`form-control-lg`和`form-control-sm`类来设置。

例如，我们可以写:

```
<div class="row mb-3">
  <label for="small" class="col-sm-2 col-form-label col-form-label-sm">Email</label>
  <div class="col-sm-10">
    <input type="email" class="form-control form-control-sm" id="small" placeholder="small">
  </div>
</div><div class="row mb-3">
  <label for="medium" class="col-sm-2 col-form-label">Email</label>
  <div class="col-sm-10">
    <input type="email" class="form-control" id="medium" placeholder="medium">
  </div>
</div><div class="row">
  <label for="large" class="col-sm-2 col-form-label col-form-label-lg">Email</label>
  <div class="col-sm-10">
    <input type="email" class="form-control form-control-lg" id="large" placeholder="large">
  </div>
</div>
```

# 列尺寸

可以使用`.col`和`.row`类来调整列的大小。

例如，我们可以写:

```
<div class="row g-3">
  <div class="col-sm-6">
    <input type="text" class="form-control" placeholder="City">
  </div> <div class="col-sm">
    <input type="text" class="form-control" placeholder="Region">
  </div> <div class="col-sm">
    <input type="text" class="form-control" placeholder="Postal code">
  </div>
</div>
```

如果断点是`sm`或更宽，我们添加`col-sm-6`和`col-sm`类来设置列宽。

在输入框之间添加一些空白。

# 自动调整大小

为了自动改变表单字段的大小，我们可以使用`col`和`col-auto`类来调整列的大小。

例如，我们可以写:

```
<form class="row gy-2 gx-3 align-items-center">
  <div class="col-auto">
    <label class="sr-only" for="name">Name</label>
    <input type="text" class="form-control" id="name" placeholder="james smith">
  </div> <div class="col-auto">
    <label class="sr-only" for="username">Username</label>
    <div class="input-group">
      <div class="input-group-text">@</div>
      <input type="text" class="form-control" id="username" placeholder="Username">
    </div>
  </div> <div class="col-auto">
    <label class="sr-only" for="fruit">Fruit</label>
    <select class="form-select" id="fruit">
      <option selected>Choose...</option>
      <option value="1">apple</option>
      <option value="2">orange</option>
      <option value="3">grape</option>
    </select>
  </div> <div class="col-auto">
    <div class="form-check">
      <input class="form-check-input" type="checkbox" id="agree">
      <label class="form-check-label" for="agree">
        I agree
      </label>
    </div>
  </div> <div class="col-auto">
    <button type="submit" class="btn btn-primary">Submit</button>
  </div>
</form>
```

我们有多个并排显示的字段，因为我们有`col-sm-3`和`col-auto`类来调整列的大小。

如果我们点击了`sm`断点或更宽的断点，它们让我们并排显示表单字段。

# 嵌入式表单

我们可以让表单与`.align-items-center`内联，使表单元素居中对齐。

使用`g-3`类添加檐槽。

例如，我们有:

```
<form class="row row-cols-md-auto g-3 align-items-center">
  <div class="col-12">
    <label class="sr-only" for="name">Name</label>
    <input type="text" class="form-control" id="name" placeholder="james smith">
  </div> <div class="col-12">
    <label class="sr-only" for="username">Username</label>
    <div class="input-group">
      <div class="input-group-text">@</div>
      <input type="text" class="form-control" id="username" placeholder="Username">
    </div>
  </div> <div class="col-12">
    <label class="sr-only" for="fruit">Fruit</label>
    <select class="form-select" id="fruit">
      <option selected>Choose...</option>
      <option value="1">apple</option>
      <option value="2">orange</option>
      <option value="3">grape</option>
    </select>
  </div> <div class="col-12">
    <div class="form-check">
      <input class="form-check-input" type="checkbox" id="agree">
      <label class="form-check-label" for="agree">
        I agree
      </label>
    </div>
  </div> <div class="col-12">
    <button type="submit" class="btn btn-primary">Submit</button>
  </div>
</form>
```

# 确认

Bootstrap 5 为我们提供了显示表单验证状态的类。

它将`:invalid`和`:valid`样式的范围限定到父`.was-validate`类。

此外，它还重置了表单的外观。

提交后`.was-validated`级将从`form`号上移除。

`.is-invalid`和`.is-valid`可以作为伪类的后备。

# 自定义样式

如果我们想要显示定制的验证消息，我们需要表单的`novalidate`布尔属性。

它禁用了浏览器默认的反馈工具提示，但仍然允许我们在 JavaScript 中使用表单验证 API。

当我们尝试提交时，我们将把`:invalid`和`:valid`样式应用于表单控件。

![](img/bb327128fdb097ae02c1cebe82fd11e0.png)

Photo by [Markus Winkler](https://unsplash.com/@markuswinkler?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以根据需要调整列的大小，以便将布局添加到表单中。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**