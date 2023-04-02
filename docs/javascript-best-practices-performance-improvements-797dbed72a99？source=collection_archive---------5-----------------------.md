# JavaScript 最佳实践—性能改进

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-performance-improvements-797dbed72a99?source=collection_archive---------5----------------------->

![](img/b1cc8746f3988dae9f3ea5c90c3cdd5c.png)

Photo by [Anna Pelzer](https://unsplash.com/@annapelzer?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究一些在编写 JavaScript 代码时应该遵循的最佳实践。

# 过度优化

我们不应该过度优化我们的代码。

如果没有明显的性能问题，那么我们可以不去管它。

如果它使我们的代码更难运行，我们就不应该做改变。

# 导致过多的文档重排

DOM 修改很慢，因为我们的浏览器必须重新构建 DOM 树并重新计算所有维度，以查看它们是如何组合在一起的。

如果元素有默认的静态定位或相对定位，问题就更大了。

所以我们应该尽量减少对 DOM 的修改。

为了减少操作，我们应该批处理我们的 DOM 操作。

例如，我们可以写:

```
let div = $('<div></div>');$('body').append(div);for (let i = 0; i < 10000; i++) {
  $('div').append(
    '<p>foo</p>'
  );
}
```

我们将`p`元素添加到 div 中 1000 次。

这显然会非常慢。

相反，我们应该只调用身体上的`append`一次:

```
let div = $('<div></div>');for (let i = 0; i < 10000; i++) {
  $('div').append(
    '<p>foo</p>'
  );
}$('body').append(div);
```

在上面的代码中，我们调用`append`将`p`元素添加到 div 中，它还不在 DOM 中。

所以我们不会改变 DOM 10000 次。

唯一的变化是最后一行，我们将 div 附加到主体。

# 过度使用文件连接

我们不应该把所有的东西串联成一个大文件。

一个大的请求可能比多个小的请求要慢。

这是因为一旦对资源发出请求，它将被缓存，直到再次更新。

拥有多个较小的文件可以利用这一点。

如果我们有一个大文件，那么我们就不能利用缓存。

组合代码的最佳方式是拥有一个在任何地方都可以使用的公共文件。

然后我们有特定于页面的文件，这些文件只在请求的页面上使用。

# 非常长的函数链

函数链可能很方便。

但我们应该意识到，我们可能会在链的中间返回`null`或`undefined`。

例如，如果我们有:

```
$('.el').click(handleClick).parents('.container')[0].slideDown();
```

那么这些方法中的任何一个都可能返回这些值，导致我们的应用程序崩溃。

# 由于晚加载 JavaScript，未样式化的内容(FOUC)闪烁

在展示正确的设计之前，我们可能会在短时间内看到不确定的内容。

改善这种情况的最好方法是减少初始加载时的 DOM 操作量。

此外，我们应该在 HTML 而不是 JavaScript 中指定页面的高度和宽度，这样我们就不必等待 JavaScript 加载来设置它们。

另一种处理方法是用 CSS 或 JavaScript 动画掩盖问题。

我们还可以阻止呈现，直到页面上的所有内容都加载完毕。

违规的 JavaScript 可以被内联并放在受影响的 DOM 元素下面。

这样，当加载元素时，代码会立即加载。

然而，这很难维护，应该谨慎使用。

我们可以写:

```
<div class="fouc-element">
  <script>
    // ...
  </script>
  ...
</div>
```

为了做到这一点。

![](img/ae2c29b5cf55a7984febcc91aca523bb.png)

Photo by [Raphael Nogueira](https://unsplash.com/@phaelnogueira?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以通过各种优化来提高性能。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**