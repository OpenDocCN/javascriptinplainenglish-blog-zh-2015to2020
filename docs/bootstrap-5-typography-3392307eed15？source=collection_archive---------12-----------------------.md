# 自举 5 —排版

> 原文：<https://javascript.plainenglish.io/bootstrap-5-typography-3392307eed15?source=collection_archive---------12----------------------->

![](img/51da2ff43fc599584ff73ebe1d6441da.png)

Photo by [Augustine Fou](https://unsplash.com/@augustinefou?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

写这篇文章时，Bootstrap 5 处于 alpha 状态，可能会更改。

Bootstrap 是任何 JavaScript 应用程序的流行 UI 库。

在这篇文章中，我们将看看如何使用 Bootstrap 5 设计文本样式。

# 内嵌文本元素

Bootstrap 5 为内联文本元素提供了样式。

例如，我们可以写:

```
<p><mark>highlight</mark></p><p><del>deleted text.</del></p><p><s>strikethrough</s></p><p><ins>This line of text is meant to be treated as an addition to the document.</ins></p><p><u>underlined</u></p><p><small>fine print.</small></p><p><strong>bold text.</strong></p><p><em>italicized text.</em></p>
```

`mark`用于标记或高亮显示的文本。

`small`用于边注释和小字。

`s`代表不再相关的元素。

`u`用于带下划线的文本。

Bootstrap 还为这些标签提供了一些类等价物。

`.mark`和`mark`风格一致。

`.small`和`small`风格一样。

`.text-decoration-underline`和`u`风格一致。

`.text-decoration-line-through`与`s`风格相同。

# 缩写

`abbr`元素用于保存缩写。

悬停时会显示扩展版本。

例如，我们可以写:

```
<p><abbr title="attribute">attr</abbr></p>
```

悬停时会显示`title`属性的值。

我们可以为更小的字体添加`.intialism`类。

我们可以写:

```
<p><abbr title="Extensible Markup Language" class="initialism">XML</abbr></p>
```

去拿那个。

# 大宗报价

我们可以使用`blockquote`元素来添加块引号。

此外，我们可以使用`blockquote`类将 HTML 括起来作为引号。

例如，我们可以写:

```
<blockquote class="blockquote">
  <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque quis nunc ultricies enim ullamcorper pretium at cursus tellus. Curabitur sit amet leo arcu. Integer vitae tincidunt odio. Duis id nunc dignissim, fringilla lacus ut, rutrum ligula. Morbi euismod accumsan augue, sit amet finibus ipsum ultrices ac. Ut convallis quis lacus in volutpat. Pellentesque volutpat dui et enim mattis, egestas posuere nisl maximus. Aenean commodo laoreet enim, sit amet tincidunt nisl porttitor non. Ut vestibulum mauris urna, eget consectetur tellus maximus at. Suspendisse pharetra ut erat sed euismod.</p>
</blockquote>
```

为了做到这一点。

# 命名来源

我们可以在报价下面添加`figcaption`元素来添加来源。

我们可以将`.blockquote-footer`类添加到 to 样式中。它。

我们还需要`cite`标签来添加源标题。

例如，我们可以写:

```
<figure>
  <blockquote class="blockquote">
    <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque quis nunc ultricies enim ullamcorper pretium at cursus tellus. Curabitur sit amet leo arcu. Integer vitae tincidunt odio. Duis id nunc dignissim, fringilla lacus ut, rutrum ligula. Morbi euismod accumsan augue, sit amet finibus ipsum ultrices ac. Ut convallis quis lacus in volutpat. Pellentesque volutpat dui et enim mattis, egestas posuere nisl maximus. Aenean commodo laoreet enim, sit amet tincidunt nisl porttitor non. Ut vestibulum mauris urna, eget consectetur tellus maximus at. Suspendisse pharetra ut erat sed euismod.</p>
  </blockquote>
  <figcaption class="blockquote-footer">
    Someone famous in <cite title="Source Title">Source Title</cite>
  </figcaption>
</figure>
```

# 对齐

我们可以使用文本实用程序更改块引用文本的对齐方式。

例如，我们可以写:

```
<figure class="text-center">
  <blockquote class="blockquote">
    <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque quis nunc ultricies enim ullamcorper pretium at cursus tellus. Curabitur sit amet leo arcu. Integer vitae tincidunt odio. Duis id nunc dignissim, fringilla lacus ut, rutrum ligula. Morbi euismod accumsan augue, sit amet finibus ipsum ultrices ac. Ut convallis quis lacus in volutpat. Pellentesque volutpat dui et enim mattis, egestas posuere nisl maximus. Aenean commodo laoreet enim, sit amet tincidunt nisl porttitor non. Ut vestibulum mauris urna, eget consectetur tellus maximus at. Suspendisse pharetra ut erat sed euismod.</p>
  </blockquote>
  <figcaption class="blockquote-footer">
    Someone famous in <cite title="Source Title">Source Title</cite>
  </figcaption>
</figure>
```

使文本居中。

# 列表

Bootstrap 5 删除列表项的默认`list-style`和左边距。

这只适用于直接子列表项。

必须将该类添加到嵌套列表中。

例如，我们可以连接:

```
<ul class="list-unstyled">
  <li>Lorem ipsum dolor sit amet</li>
  <li>Duis id nunc dignissim</li>
  <li>Ut vestibulum mauris urna, eget consectetur tellus</li>
  <li>Facilisis in pretium nisl aliquet</li>
  <li>Nulla volutpat aliquam velit
    <ul>
      <li>Phasellus iaculis neque</li>
      <li> Suspendisse potenti. Aliquam erat volutpat.</li>
      <li>Vestibulum laoreet porttitor sem</li>
      <li>Ac tristique libero volutpat at</li>
    </ul>
  </li>
  <li>Pellentesque habitant morbi tristique senectus</li>
  <li>Aenean sit amet erat nunc</li>
  <li>Eget porttitor lorem</li>
</ul>
```

添加嵌套列表。

我们添加了`list-unstyled`类来添加一个无样式列表。

![](img/4a93f1f985e936295a28b95cc2a8b1db.png)

Photo by [Suad Kamardeen](https://unsplash.com/@skmuse_?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以在应用程序中使用 Bootstrap 5 的文本和列表样式。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**