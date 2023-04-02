# JavaScript 和网络——布局和风格

> 原文：<https://javascript.plainenglish.io/javascript-and-the-web-layouts-and-styles-e5d7efa1c419?source=collection_archive---------7----------------------->

![](img/3a4815d64f8c2d1d4c53377cb8c760d0.png)

Photo by [Guillaume Muckensturm](https://unsplash.com/@mucky?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是世界上最流行的编程语言之一。为了有效地使用它，我们必须了解它的基本知识。

在本文中，我们将看看如何在我们的 web 应用程序中使用 JavaScript。

我们看看如何在 JavaScript 中改变元素的布局和样式。

# 布局

我们可以用 JavaScript 动态改变元素的布局。

一个元素节点对象有一些属性，我们可以用它们来改变它们的样式。

`offsetWidth`和`offsetHeight`属性给了我们一个元素以像素为单位的空间。

像素是浏览器中的基本度量单位。

`clientWidth`和`clientHeight`给我们元素内部的空间，忽略边框宽度。

`getBoundingClientRect` 给我们一个元素在屏幕上的精确位置。

它返回`top`、`bottom`、`left`和`right`属性，这些属性具有相对于屏幕左上角的每一边的像素数。/

如果我们想要相对于整个文档的位置，那么我们添加当前的滚动位置，这是通过`pageXOffset`和`pageYOffset`属性获得的。

例如，给定以下 HTML:

```
<p>
  a
</p>
<p>
  b
</p>
<p>
  c
</p>
```

我们可以写:

```
const para = document.body.getElementsByTagName("p")[0];
console.log(para.offsetWidth)
console.log(para.offsetHeight)
```

获取元素的宽度和高度。

我们可以写:

```
console.log(para.getBoundingClientRect())
```

在一个函数中获取包括宽度和高度在内的所有内容。

# 式样

我们可以用元素的`style`属性来设计样式。

例如，我们可以写:

```
<p style='color: green'>
  a
</p>
```

我们得到一些绿色文本。

JavaScript 可以让我们动态地做同样的事情。

样式位于 DOM 元素节点的`style`属性中。

例如，我们可以写:

```
const para = document.getElementsByTagName("p")[0];
para.style.color = "magenta";
```

然后我们可以看到文本变成了洋红色。

在 JavaScript 中，带有破折号的样式被转换成驼色。

所以 CSS 中的`font-family`和 JavaScript 中的`style.fontFamily`是一样的。

# 级联样式

HTML 的样式系统称为 CSS，代表级联样式表。

样式表是一组关于如何设计文档中元素样式的规则。

我们可以把它们放在一个内嵌的样式标签中。

或者我们可以把它们放在一个单独的文件里。

例如，我们可以写:

```
p {
  color: 'magenta';
}
```

然后我们指定所有的`p`元素的内容颜色设置为洋红色。

我们还可以通过编写以下内容来选择具有特定 id 或类别的项目:

```
#foo {
  color: 'magenta';
}.bar {
  color: 'green';
}
```

`#foo`表示我们选择一个 ID 为`foo`的元素。`#`表示 ID。

同样，我们选择带有类别`bar`和`.`的元素

`>`表示我们选择给定元素的直接子元素。

例如，我们可以编写`p > a`来选择所有`p`的直接后代`a`元素。

# 查询选择器

选择器语法可以与`querySelector`或`querySelectorAll`方法一起使用，以更灵活的方式选择元素。

例如，如果我们有下面的 HTML:

```
<p>
  a
  <span>foo</span>
  <span>bar</span>
</p>
```

我们可以写:

```
const span = document.querySelector('p > span');
```

选择`p`元素中的第一个`span`元素。

要选择它们并在节点列表中返回它们，我们可以写:

```
const spans = document.querySelectorAll('p > span');
```

由`querySelectorAll`返回的对象不是活动的，所以当我们改变文档时它们不会改变。

因为它是一个节点列表，我们可以使用 spread 操作符或`Array.from`将其转换为一个数组。

# 定位和动画

`position`样式属性让我们改变元素的位置。

默认情况下，它被设置为`'static'`，这意味着它位于文档中的正常位置。

但是，我们可以将其设置为`relative`或`absolute`，然后我们可以通过`top`和`left`属性来改变元素的位置。

例如，我们可以写:

```
let p = document.querySelector("p");
let pos = 0;const animate = (time, lastTime) => {
  if (lastTime !== null) {
    pos = time;
  }
  p.style.top = `${pos}px`
  p.style.left = `${pos}px`
  requestAnimationFrame(newTime => animate(newTime, time));
}
requestAnimationFrame(animate);
```

要制作动画:

```
<p style='position: fixed;'>foo</p>
```

我们用`animate`函数改变`p`元素的位置，该函数用参数的时间来获取`time`和`lastTime`参数。

然后我们设置`pos`变量。

然后我们设置`style`的`top`和`left`属性来移动`p`元素。

我们调用`requestAnimationFrame`来获取下一帧。

![](img/b77fafcdf57049f66578b22f92084d02.png)

Photo by [Charlota Blunarova](https://unsplash.com/@charlotablunarova?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以设置元素的位置来激活它们。

此外，我们可以用`style`属性来设置它们的样式。

我们还可以得到具有各种属性的元素的维数。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**