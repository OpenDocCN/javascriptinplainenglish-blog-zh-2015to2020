# 面向对象的 JavaScript——分离行为

> 原文：<https://javascript.plainenglish.io/object-oriented-javascript-separating-behavior-7313da99dbeb?source=collection_archive---------20----------------------->

![](img/da61054ee22294fe7542aae8ba614d93.png)

Photo by [Simon Rae](https://unsplash.com/@simonrae?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将研究一些基本的编码和设计模式。

# 编码和设计模式

为了以一种我们可以轻松使用的方式组织我们的代码，我们必须遵循一些允许这种情况发生的模式。

这个问题很多人都想到了，所以我们大部分可以沿用现有的模式。

编码模式是我们可以遵循来改进代码的一些最佳实践。

设计模式是独立于语言的模式，由四人帮的书推广开来。

# 分离行为

网页的构造块包括 HTML、CSS 和 JavaScript 代码。

内容应该在 HTML 代码中。

它们有描述内容含义的标记。

例如，我们用`ul`表示列表，用`li`表示列表项等等。

HTML 代码应该没有任何格式元素。

视觉格式化属于表示层，应该用 CSS 来实现。

`style`属性不应该用于格式化。

也不应该使用表示性的 HTML 标签。

标签应该根据它们的意思来使用。

所以`section`用于将文本分成几个部分，`aside`用于侧边栏等。一切都应该用`div`代替。

# 介绍会；展示会

为了让我们的页面更好看，我们可以重置浏览器的默认设置，让所有浏览器都一样。

像 Bootstrap 这样的流行 CSS 框架会做到这一点。

# 行为

任何动态行为都应该用 JavaScript 代码编写。

JavaScript 应该位于脚本标签中的外部文件的脚本中。像`onclick`、`onmouseover`等内联属性。不应该用。

我们应该尽量减少脚本标签的数量，这样就可以减少需要加载的 JavaScript 代码。

应该避免内联事件处理程序，将 JavaScript 代码放在 HTML 代码之外。

CSS 表达式不应该用来保持兼容性。

为了分离行为，我们可以将表单提交代码与表单本身分离开来。

例如，我们可以编写以下 HTML:

```
<body>
  <form id="nameForm" method="post" action="server.php">
    <fieldset>
      <legend>name</legend>
      <input name="name" id="name" type="text" />
      <input type="submit" />
    </fieldset>
  </form>
  <script src="script.js"></script>
</body>
```

创建一个表单。

然后我们可以监听`script.js`中的提交事件:

```
const nameForm = document.querySelector('#nameForm');
nameForm.addEventListener('submit', (e) => {
  const el = document.getElementById('name');
  if (!el.value) {
    e.preventDefault();
    alert('Please enter a name');
  }
});
```

我们用`querySelector`得到表单。

然后我们用`addEventListener`监听`submit`事件。

在事件处理程序中，我们通过 ID 获取`name`字段。

如果没有值，我们用`preventDefault()`停止提交。

然后我们显示一个警告。

![](img/9cf2265396d99de61af3c2a1be03168f.png)

Photo by [Josué AS](https://unsplash.com/@yehoshuaas?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该在代码中分离内容、样式和动态行为。

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**