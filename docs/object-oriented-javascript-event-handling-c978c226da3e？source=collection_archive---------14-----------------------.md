# 面向对象的 JavaScript —事件处理

> 原文：<https://javascript.plainenglish.io/object-oriented-javascript-event-handling-c978c226da3e?source=collection_archive---------14----------------------->

![](img/7520cc6071dde25d409e3be9d5846ae7.png)

Photo by [Will Porada](https://unsplash.com/@will0629?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将研究事件处理。

# 停止传播

我们可以用`stopPropagation`方法阻止事件冒泡。

如果我们不希望事件从事件发生的元素传播到`document`和两者之间的所有元素，我们可以写:

```
const div = document.querySelector('div');
div.addEventListener('click', (e) => {
  e.stopPropagation();
  alert('clicked');
});
```

`e`是事件对象。

我们可以对它调用`stopProgation`，这样只有 div 的监听器会监听 click 事件。

# 防止默认行为

我们可以通过在浏览器上调用`preventDefault`方法来阻止浏览器的默认行为。

例如，如果我们有以下 HTML:

```
<a href='http://example.com'>click me</a>
```

我们可以通过编写以下内容来监听点击事件:

```
const a = document.querySelector('a');
a.addEventListener('click', (e) => {
  if (!confirm('You sure you want to leave?')) {
    e.preventDefault();
  }});
```

我们有一个`confirm`函数调用来显示一个对话框。

如果用户点击取消，那么函数返回`false`并且`preventDefault`将被调用。

一旦完成，那么`a`元素的默认动作，即转到`href`属性中的 URL，将被停止。

并不是所有的事件都让我们阻止默认行为，但大多数情况下是这样的。

如果我们想要确定，我们可以检查事件对象的`cancellable`属性。

# 跨浏览器事件侦听器

大多数现代浏览器都实现了 DOM 2 规范，所以我们编写的代码大多与它们兼容。

唯一不同的是 IE，它正被迅速淘汰。

# 事件的类型

可以发出不同种类的事件。

包括`mousemove`、`mouseup`、`mousdown`、`click`和`dblclick`等鼠标事件。

`mouseover`当用户将鼠标放在一个元素上时发出。

`mouseout`当鼠标在一个元素上但离开它时发出。

关键字事件包括`keydown`、`keypress`和`keuyup`。

这些按顺序发生。

加载和窗口事件包括`load`事件，当一幅图像或一个页面及其所有组件完成加载时会发出该事件。

`unload`在用户离开页面时发出。

`beforeunload`用户离开页面前加载。

出现 JavaScript 错误时会发出`error`。

调整浏览器大小时发出`resize`。

滚动页面时发出`scroll`。

出现右键菜单时会发出`contextmenu`。

当我们进入一个表单域时，表单事件包括`focus`。

`blur`是我们离开一个表单域时发出的。

当我们在值改变后离开一个字段时，发出`change`。

当我们选择一个文本字段时会发出`select`。

清除所有输入时发出`reset`,发送表单时发出`submit`。

现代浏览器也有类似`dragstart`、`dragend`、`drop`等拖拽事件。

![](img/1d8ae56aed03aaa7cc65a645f2497b5b.png)

Photo by [Eugene Chystiakov](https://unsplash.com/@eugenechystiakov?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以停止事件的传播或默认行为。

此外，我们可以听各种各样的事件。

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**