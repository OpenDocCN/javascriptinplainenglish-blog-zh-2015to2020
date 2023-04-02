# 引导 5 —文本样式

> 原文：<https://javascript.plainenglish.io/bootstrap-5-text-styling-3876f0315ef0?source=collection_archive---------7----------------------->

![](img/8a26c66394937a6f678e20ab1bf76b0d.png)

Photo by [Dominik QN](https://unsplash.com/@dominik_qn?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

写这篇文章时，Bootstrap 5 处于 alpha 状态，可能会发生变化。

Bootstrap 是任何 JavaScript 应用程序的流行 UI 库。

在这篇文章中，我们将看看如何使用 Bootstrap 5 设计文本样式。

# 移行点

Bootstrap 提供了`.text-break`类来设置`word-wrap: reak-word` CSS 属性。

例如，我们可以写:

```
<p class="text-break">
ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff
</p>
```

添加一个 p 元素，将长单词分解以防止溢出视口。

# 文本转换

我们可以用各种类将文本转换成大写字母、小写字母或大写字母。

例如，我们可以写:

```
<p class="text-lowercase">lower case</p>
<p class="text-uppercase">upper case</p>
<p class="text-capitalize">capital case</p>
```

更改类中文本的大小写。

`text-lowercase`将文本改为小写。

`text-uppercase`将文本改为大写。

`text-capitalize`将大小写改为大写。

# 字体粗细和斜体

我们也可以用 Bootstrap 5 提供的类来使文本加粗或斜体。

例如，我们可以写:

```
<p class="font-weight-bold">Bold text.</p>
<p class="font-weight-bolder">Bolder text</p>
<p class="font-weight-normal">Normal text.</p>
<p class="font-weight-light">Light text.</p>
<p class="font-weight-lighter">Lighter text.</p>
<p class="font-italic">Italic text.</p>
<p class="font-normal">unstyled text</p>
```

`font-weight-bold`类使文本加粗。

`font-weitght-bolder`使文本相对于父文本加粗。

`font-weight-light`使文本变浅。

`font-weight-lighter`使文本相对于父文本变浅。

`font-italic`使文本倾斜。

`font-normal`取消文本样式。

# 行高

我们可以用`.lh-*`类改变行高。

例如，我们可以写:

```
<p class="lh-1">Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec nec mi auctor, blandit nisi ac, venenatis erat.</p><p class="lh-sm">Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec nec mi auctor, blandit nisi ac, venenatis erat.</p><p class="lh-base">Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec nec mi auctor, blandit nisi ac, venenatis erat.</p><p class="lh-lg">Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec nec mi auctor, blandit nisi ac, venenatis erat.</p>
```

`lh-l`使每行之间的间距变短。

`lh-sm`使每行之间的间距变大一点。

`lh-base`使每行之间的间距大于`lh-sm`。

`lh-l`使每行之间的间距变大。

# 单一间隔

我们可以用`font-monospace`类实现文本等宽。

例如，我们可以写:

```
<p class="font-monospace">monospace</p>
```

以等宽字体显示文本。

# 重置颜色

要将一段文本或一个链接的颜色重置为与父项相同的样式，我们可以使用`.text-reset`类。

例如，我们可以写:

```
<p class="text-muted">
  <a href="#" class="text-reset">reset link</a>.
</p>
```

因为我们有了`text-muted`类，所以让 p 元素中的文本静音。

`text-reset`将样式应用于链接。

# 文本装饰

Bootstrap 5 有改变文本装饰的类。

例如，我们可以写:

```
<p class="text-decoration-underline">
  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec nec mi auctor, blandit nisi ac, venenatis erat. Proin aliquam auctor ipsum eu congue. Mauris consequat dolor id posuere posuere. Curabitur in blandit eros. Donec at dui turpis. Curabitur faucibus est enim, quis facilisis enim ultrices a. Vivamus sed tellus tortor. Fusce malesuada ante dui. Morbi dignissim sapien eu mattis facilisis. Nunc vel enim nec risus rhoncus mollis at accumsan diam. Aliquam sed volutpat turpis.
</p><p class="text-decoration-line-through">
  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec nec mi auctor, blandit nisi ac, venenatis erat. Proin aliquam auctor ipsum eu congue. Mauris consequat dolor id posuere posuere. Curabitur in blandit eros. Donec at dui turpis. Curabitur faucibus est enim, quis facilisis enim ultrices a. Vivamus sed tellus tortor. Fusce malesuada ante dui. Morbi dignissim sapien eu mattis facilisis. Nunc vel enim nec risus rhoncus mollis at accumsan diam. Aliquam sed volutpat turpis.
</p><a href="#" class="text-decoration-none">
  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec nec mi auctor, blandit nisi ac, venenatis erat. Proin aliquam auctor ipsum eu congue. Mauris consequat dolor id posuere posuere. Curabitur in blandit eros. Donec at dui turpis. Curabitur faucibus est enim, quis facilisis enim ultrices a. Vivamus sed tellus tortor. Fusce malesuada ante dui. Morbi dignissim sapien eu mattis facilisis. Nunc vel enim nec risus rhoncus mollis at accumsan diam. Aliquam sed volutpat turpis.
</a>
```

`text-decoration-underline`使文本带下划线。

`text-decoration-line-through`使文本中间有一条线穿过。

`text-decoration-none`移除所有文本装饰。

![](img/8a5baf0ad2d19d1a1e90dcbdd93a14fc.png)

Photo by [Hanny Naibaho](https://unsplash.com/@hannynaibaho?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

Bootstrap 5 提供了许多按照我们的方式设计文本样式的类。

它们包括下划线、删除线、静音文本等等。

## **简明英语 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**