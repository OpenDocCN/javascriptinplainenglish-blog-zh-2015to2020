# 强大的 JavaScript —安全性和性能

> 原文：<https://javascript.plainenglish.io/robust-javascript-security-and-performance-9012875cd5ce?source=collection_archive---------4----------------------->

![](img/0adde48c769d014bb99ff4cb93db20a0.png)

Photo by [Yves Alarie](https://unsplash.com/@yvesalarie?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究一些在编写 JavaScript 代码时应该遵循的最佳实践。

# 不要污染窗户物体

我们不应该给`window`对象添加我们自己的属性。

这就产生了很容易被覆盖的全局变量。

不同属性的名称可能会冲突。

为了避免添加全局变量，我们应该将变量放在函数内部:

```
(function () {
  for (let i = 0; i < 9; i++) {
    // ...
  }
  window.result = true;
})();
```

我们只是添加必须添加到`window`中的属性，而将其余的保留在里面。

`i`停留在函数内部。

# 安全输出代码

我们应该用一些方法来保护我们的代码。

当我们设置一个元素的`innerHTML`属性时，我们不应该让用户设置任何东西。

例如，如果我们有这样的东西:

```
const linkElement = document.getElementById('link');
const linkUrl = 'https://example.com/';
const linkContent = 'example site';linkElement.innerHTML = `<div class="container"><a href="${linkUrl}">${linkContent}</a></div>`;
```

我们有`linkElement`的`innerHTML`属性，它有设置 URL 的链接。

这段代码很危险，因为攻击者可以将脚本传递给这些变量。

我们可以使用`textContent`属性来代替设置接受任何内容的`innerHTML`属性。

例如，我们可以写:

```
const linkElement = document.getElementById('link');
const linkUrl = 'https://example.com/';
const linkContent = 'example site'linkElement.textContent = linkContent;
linkElement.href  = linkUrl;
```

我们设置`textContent`来设置显示给用户的文本。

并且`href`设置主播的 href 属性。

# 使用`DOM` API 来创建和添加元素

我们可以用`document.createElement`方法添加元素。

例如，我们可以写:

```
const newDiv = document.createElement("div");
document.body.appendChild(newDiv);
```

我们用`createElement`创建一个 div，并调用`appendChild`将它附加到主体。

# 表演

当我们编写 JavaScript 应用程序来提高它的性能时，我们应该注意一些事情。

# 仅加载我们需要的库

我们应该只加载我们需要的库，尽可能加载最少的代码。

这将提高我们代码的加载速度。

如果我们有未使用的库，我们应该删除它们。

如果 JavaScript 标准库中有替代品，我们应该使用它们。

例如，我们应该使用内置的数组实例方法，而不是使用 Lodash 数组方法。

如果我们使用 Lodash 这样的大型库，我们应该只使用；y 导入我们需要的函数。

所以我们应该像这样导入它们:

```
import map from 'lodash/map';
import tail from 'lodash/tail';
import uniq from 'lodash/uniq';
```

# 缓存 DOM 选择

如果我们使用 DOM 节点，我们应该在变量中缓存从 DOM 中检索到的节点。

例如，我们可以写:

```
const menu = document.querySelector('#menu');
const hideButton = document.querySelector('.hide-button');

hideButton.addEventListener('click', () => {
  menu.style.display = 'none';
});
```

缓存`menu`变量。

这比写作更好:

```
const hideButton = document.querySelector('.hide-button');

hideButton.addEventListener('click', () => {
  const menu = document.querySelector('#menu');
  menu.style.display = 'none';
});
```

在第二个例子中，每当我们点击类为`hide-button`的元素时，我们就获得带有 ID 菜单的元素。

这是非常低效的，因为我们每次都从 DOM 中获取它。

![](img/3e40cbfff8d475fca6c14b9b6203f071.png)

Photo by [Artur Wayne](https://unsplash.com/@lctune?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该注意到一些小的变化，这些变化可以极大地提高性能，使它们更加安全。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**