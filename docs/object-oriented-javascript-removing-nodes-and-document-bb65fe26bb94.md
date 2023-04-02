# 面向对象的 JavaScript —删除节点和文档

> 原文：<https://javascript.plainenglish.io/object-oriented-javascript-removing-nodes-and-document-bb65fe26bb94?source=collection_archive---------8----------------------->

![](img/0b7fccadd9d5a4581e2ceea4896bec8f.png)

Photo by [K8](https://unsplash.com/@k8_iv?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将研究 DOM 和一些有用的`document`属性。

# 删除节点

我们可以调用`removeChild`方法来删除一个子节点。

例如，如果我们有下面的 HTML:

```
<body>
  <p class="opener">foo</p>
  <p><em>bar</em> </p>
  <p id="closer">baz</p>
  <!-- the end -->
</body>
```

我们可以通过编写以下内容来删除节点:

```
const p = document.querySelector('p');
const removed = document.body.removeChild(p);
```

然后用`removeChild`将`p`元件从机体上取下。

移除的节点被返回。

还有一种`replaceChild`方法，让我们移除一个节点，并在适当的位置放置另一个节点。

例如，我们可以写:

```
const p1 = document.querySelector('.opener');
const p2 = document.querySelector('#closer');
const removed = document.body.replaceChild(p1, p2);
```

用具有类`opener`的节点替换 ID 为`closer`的节点。

# HTML —仅 DOM 对象

有多种方法可以访问纯 HTML 的 DOM 对象。

我们可以用现代浏览器中的一些属性来访问 than。

`document.body`有`body`元素。

它是 DOM Level 0 规范中的元素之一，被移到了 DOM 规范的 HTML 扩展中。

其他属性包括`document.images`来获取 img 元素。

`document.applets`让我们获取 applet 元素。

`document.links`获取`link`元素。

`document.anchors`获取我们锚定的`a`元素。

让我们拿表格。

所以:

```
document.forms[0];
```

与以下内容相同:

```
document.getElementsByTagName('forms')[0];
```

如果我们有以下形式:

```
<form>
  <input name="name" id="name" type="text" size="50" maxlength="255" value="Enter name" />
</form>
```

然后，我们可以获取输入元素，并用以下内容设置值:

```
document.forms[0].elements[0].value = 'foo';
```

我们可以通过以下方式禁用输入:

```
document.forms[0].elements[0].disabled = true;
```

我们可以通过它的`name`属性得到输入:

```
document.forms[0].elements['name'];
```

或者:

```
document.forms[0].elements.name;
```

# document.write()方法

`document.write`方法让我们将 HTML 写入页面。

例如，我们可以写:

```
document.write(`<p>${new Date()}</p>`);
```

把日期写到屏幕上。

如果我们加载页面，`document.write`将替换页面上的所有内容。

# Cookies、标题、推荐人和域

我们可以用`document.cookie`属性获取并设置客户端 cookies。

这是一个包含一些键值和有效期的字符串。

当服务器向客户机发送页面时，它可能会将`Set-Cookie` HTTP 响应头和 cookie 一起发送回去。

`document.title`属性让我们获取并设置页面的标题，因此我们可以写:

```
document.title = 'title';
```

属性拥有当前页面的域名。

如果我们在那里，我们可能会得到类似`“jsfiddle.net”`的东西。

我们不能将它设置到不同的域，所以如果我们在 jsfiddle.net，我们就不能写:

```
document.domain = 'example.com'
```

我们将得到错误:

```
Uncaught DOMException: Failed to set the 'domain' property on 'Document': 'example.com' is not a suffix of 'jsfiddle.net'.
```

这对保护用户有好处。我们不想让脚本去任何他们喜欢的领域。

![](img/b6399e492514a492931073b8bc4af662.png)

Photo by [Eric Prouzet](https://unsplash.com/@eprouzet?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用 DOM 方法删除节点。

同样，我们可以使用`document`的各种属性和方法来操作文档。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**