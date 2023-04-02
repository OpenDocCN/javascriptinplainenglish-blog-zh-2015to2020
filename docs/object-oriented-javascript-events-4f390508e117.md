# 面向对象的 JavaScript —事件

> 原文：<https://javascript.plainenglish.io/object-oriented-javascript-events-4f390508e117?source=collection_archive---------6----------------------->

![](img/895004120e34d88b17377a7ea0b77c0c.png)

Photo by [Ignacio Brosa](https://unsplash.com/@ignaciobrosa?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 在一定程度上是一种面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将研究倾听事件。

# 事件

我们可以倾听用户触发的事件。

例如，我们可以听到鼠标点击，按键，页面加载等。

我们可以附加事件侦听器函数来侦听所有这些事件。

# 内嵌 HTML 属性

将事件侦听器添加到元素中的一种方法是将它们作为属性值添加。

例如，我们可以写:

```
<div onclick="alert('clicked')">click me</div>
```

我们有`onclick`属性，其值是 JavaScript 代码`alert(‘clicked’)`。

这很容易添加，但是如果代码变得复杂，就很难使用。

# 元素属性

我们也可以将它们添加为元素属性。

例如，给定以下 HTML:

```
<div>click me</div>
```

我们可以写道:

```
const div = document.querySelector('div');
div.onclick = () => {
  alert('clicked');
};
```

我们有了`document.querySelector`得到的 div。

然后我们将`onclick`属性设置为一个函数，当我们点击 div 时该函数运行。

这更好，因为它将 JavaScript 排除在我们的 HTML 之外。

这种方法的缺点是我们只能给事件附加一个函数。

# DOM 事件侦听器

监听浏览器事件的最好方法是添加事件侦听器。

我们想吃多少就吃多少。

听众不需要互相了解，可以独立工作。

为了将事件侦听器附加到我们的元素，我们可以调用`addEventListener`方法。

例如，我们可以写:

```
const div = document.querySelector('div');
div.addEventListener('click', () => {
  alert('clicked');
});div.addEventListener('click', console.log.bind(div))
```

我们添加了 2 个不同的事件侦听器。

一个是显示警报的功能。

另一种是在控制台中记录事件对象。

# 捕获和冒泡

`addEventListener`方法取第三个参数。

它允许我们设置是否在捕获模式下监听。

事件捕获是指事件从`document`对象向下传播到最初触发事件的元素。

事件冒泡则相反。它是事件从触发事件的元素传播到`document`对象的地方。

如果我们将第三个参数设置为`true`，那么我们就可以听到从`docuemnt`对象到原始元素的事件传播。

事件从捕获阶段到原始元素，再到冒泡阶段。

大多数浏览器都实现了这 3 个阶段，所以我们可以在任何地方设置可选的。

我们可以通过调用事件对象上的`stopPropagation`来停止从源元素到文档的传播。

我们也可以使用事件委托来检查哪个元素触发了一个事件。

![](img/40593f992c05ec9fea6d237fca274c80.png)

Photo by [Pablo Heimplatz](https://unsplash.com/@pabloheimplatz?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

有很多种倾听元素事件的方式。

我们可以添加事件侦听器，或者将内嵌的 JavaScript 添加到 HTML 中。

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**