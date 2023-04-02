# 面向对象的 JavaScript — DOM 遍历快捷方式

> 原文：<https://javascript.plainenglish.io/object-oriented-javascript-dom-traversal-shortcuts-47d0c802eadf?source=collection_archive---------6----------------------->

![](img/14af98d13b4aed63e72ede4f9059ea62.png)

Photo by [Juan Gomez](https://unsplash.com/@nosoylasonia?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将研究 DOM。

# DOM 访问快捷方式

使用`childNodes`、`parentNode`、`nodeName`、`nodeValue`和`attributes`属性让我们遍历 DOM。

然而，如果我们有任何复杂的文档，使用它们非常不方便。

更深层次的节点很难访问，也没有简单的方法来搜索我们想要的项目。

我们有各种快捷方法，让我们以更容易的方式找到我们想要的 DOM 元素。

方法包括`getElementsByTagName()`、`getElementsByName()`、`getElementById()`、`querySelector()`、`querySelectorAll()`。

`getElementsByTagName`方法让我们获得具有给定标记名的所有元素。

例如，对于给定的 HTML:

```
<body>
  <p class="opener">foo</p>
  <p><em>bar</em> </p>
  <p id="closer">baz</p>
  <!-- the end -->
</body>
```

我们可以写:

```
document.getElementsByTagName('p');
```

返回一个`p`元素的节点列表。

我们可以用`length`属性得到`p`元素的数量:

```
document.getElementsByTagName('p').length
```

我们可以通过索引得到元素:

```
document.getElementsByTagName('p')[0]
```

我们可以用`innerHTML`得到 HTML:

```
document.getElementsByTagName('p')[0].innerHTML;
```

我们得到了`'foo'`。

`getElementById`让我们通过 ID 获取一个元素。

我们可以写:

```
document.getElementById('closer');
```

获取 ID 为`closer`的元素。

`getElementByClassName`让我们通过类名获取元素。

`querySelector`让我们用给定的 CSS 选择器获取一个元素。

而`querySelectorAll`让我们用给定的 CSS 选择器获取所有元素。

# 兄弟姐妹，身体，第一个和最后一个孩子

如果我们引用了一个元素，那么`nextSibling`和`previousSibling`属性是导航 DOM 树的两个方便的属性。

所以如果我们有:

```
const p = document.getElementById('closer');
```

我们可以写作

```
console.log(p.previousSibling)
```

并获得:

```
#text
```

我们可以通过书写来再次使用它:

```
console.log(p.previousSibling.previousSibling)
```

上面是 p 元素。

我们可以混合使用`previousSibling`和`nextSibling`。

例如，我们可以写:

```
p.previousSibling.nextSibling
```

一个 DOM 节点也有`firstChild`和`lastChild`属性。

例如，我们可以使用:

```
document.body.firstChild;
```

获取正文的第一个子节点。

`firstChild`与`childNodes[0]`相同。

获取一个节点的最后一个子节点。

我们可以使用:

```
document.body.lastChild;
```

获取正文的最后一个子节点。

`lastChild`与`childNodes[childNodes.length — 1]`相同。

# 修改 DOM 节点

我们可以通过给`innerHTML`属性赋值来修改 DOM 节点

例如，我们可以写:

```
const p = document.getElementById('closer');
p.innerHTML = 'closer';
```

我们也可以将 HTML 字符串设置为`innerHTML`的值:

```
const p = document.getElementById('closer');
p.innerHTML = '<em>closer</em> closer';
```

我们还可以通过给`nodeValue`属性赋值来改变节点的内容:

```
const p = document.getElementById('closer');
p.nodeValue = 'closer';
```

![](img/42117e8c932ae51c7e6c94f527b401bf.png)

Photo by [Hamish Duncan](https://unsplash.com/@hambourine?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用各种快捷方式得到 DOM 节点。

此外，我们可以用各种属性设置内容。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**