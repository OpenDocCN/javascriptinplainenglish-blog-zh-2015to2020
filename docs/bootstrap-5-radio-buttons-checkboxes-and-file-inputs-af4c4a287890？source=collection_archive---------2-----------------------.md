# 引导程序 5 —单选按钮、复选框和文件输入

> 原文：<https://javascript.plainenglish.io/bootstrap-5-radio-buttons-checkboxes-and-file-inputs-af4c4a287890?source=collection_archive---------2----------------------->

![](img/3924c2b4f5d5962cf00826b9e32143cc.png)

Photo by [Shanice Garcia](https://unsplash.com/@sagarcia?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

**写这篇文章时，Bootstrap 5 处于 alpha 状态，可能会更改。**

Bootstrap 是任何 JavaScript 应用程序的流行 UI 库。

在本文中，我们将看看如何使用 Bootstrap 5 设计单选按钮、复选框和文件输入。

# 没有标签的单选按钮和复选框

我们可以添加没有标签的单选按钮和复选框。

为此，我们可以写:

```
<div>
  <input class="form-check-input" type="checkbox" id="checkboxNoLabel" value="">
</div><div>
  <input class="form-check-input" type="radio" name="radioNoLabel" id="radioNoLabel1" value="">
</div>
```

来添加它们。

# 切换按钮

Bootstrap 让我们将复选框和单选按钮变成切换按钮。

为此，我们可以写:

```
<input type="checkbox" class="btn-check" id="btn-check" autocomplete="off">
<label class="btn btn-primary" for="btn-check">toggle</label>
```

添加切换按钮。

根据开关的不同，它会有不同的显示。

`input`的`id`和`label`的`for`属性必须匹配。

# 单选切换按钮

我们可以对单选按钮做同样的事情。

例如，我们可以写:

```
<div class="btn-group">
  <input type="radio" class="btn-check" name="options" id="apple" autocomplete="off" checked>
  <label class="btn btn-secondary" for="apple">apple</label><input type="radio" class="btn-check" name="options" id="orange" autocomplete="off">
  <label class="btn btn-secondary" for="orange">orange</label><input type="radio" class="btn-check" name="options" id="grape" autocomplete="off">
  <label class="btn btn-secondary" for="grape">grape</label>
</div>
```

添加按钮组。

每个按钮都是一组单选按钮。

我们使用`btn-group`类来设计按钮组的样式。

输入类型设置为`radio`。

是使它们显示为切换按钮的类。

并且输入的`id`必须匹配`label`的`for`属性值。

# 轮廓样式

切换可以有轮廓样式，而不是背景颜色。

例如，我们可以写:

```
<div class="btn-group">
  <input type="radio" class="btn-check" name="options" id="apple" autocomplete="off" checked>
  <label class="btn btn-outline-secondary" for="apple">apple</label><input type="radio" class="btn-check" name="options" id="orange" autocomplete="off">
  <label class="btn btn-outline-secondary" for="orange">orange</label><input type="radio" class="btn-check" name="options" id="grape" autocomplete="off">
  <label class="btn btn-outline-secondary" for="grape">grape</label>
</div>
```

使未被选中的按钮显示轮廓。

我们在标签的类名中添加单词`outline`。

对于复选框，我们可以写:

```
<input type="checkbox" class="btn-check" id="btn-check-outlined" autocomplete="off">
<label class="btn btn-outline-primary" for="btn-check-outlined">apple</label><br>
```

使切换按钮以轮廓样式而不是背景色显示。

# 文件浏览器

Bootstrap 5 提供了一个文件浏览器，让用户选择文件。

例如，我们可以写:

```
<div class="form-file">
  <input type="file" class="form-file-input" id="customFile">
  <label class="form-file-label" for="customFile">
    <span class="form-file-text">Choose a file...</span>
    <span class="form-file-button">Browse</span>
  </label>
</div>
```

添加一个文件输入，输入的`type`设置为`file`。

同样，我们用`form-file-label`类添加标签。

在它里面，我们添加了占位符，这是带有`form-file-text`类的 span。

我们也有一个带有`form-file-button`类的 span 来显示一个按钮，我们可以点击它来显示文件浏览器，让我们选择一个文件。

要禁用文件输入，我们可以将`disabled`属性添加到输入中。

例如，我们可以写:

```
<div class="form-file">
  <input type="file" class="form-file-input" id="customFile" disabled>
  <label class="form-file-label" for="customFile">
    <span class="form-file-text">Choose file...</span>
    <span class="form-file-button">Browse</span>
  </label>
</div>
```

禁用文件输入。

如果我们有更长的占位符文本，它将被截断，并在末尾添加省略号:

```
<div class="form-file">
  <input type="file" class="form-file-input" id="customFile">
  <label class="form-file-label" for="customFile">
    <span class="form-file-text">Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque quis nunc ultricies enim ullamcorper pretium at cursus tellus. Curabitur sit amet leo arcu. Integer vitae tincidunt odio. Duis id nunc dignissim, fringilla lacus ut, rutrum ligula. Morbi euismod accumsan augue, sit amet finibus ipsum ultrices ac. Ut convallis quis lacus in volutpat. Pellentesque volutpat dui et enim mattis, egestas posuere nisl maximus. Aenean commodo laoreet enim, sit amet tincidunt nisl porttitor non. Ut vestibulum mauris urna, eget consectetur tellus maximus at. Suspendisse pharetra ut erat sed euismod.
    </span>
    <span class="form-file-button">Browse</span>
  </label>
</div>
```

![](img/64bf054be7ae7fca29cd37976364658f.png)

Photo by [Pixzolo Photography](https://unsplash.com/@pixzolo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以将单选按钮和复选框设计成切换按钮。

另外，Bootstrap 5 自带文件输入。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**