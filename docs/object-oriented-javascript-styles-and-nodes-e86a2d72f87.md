# 面向对象的 JavaScript —样式和节点

> 原文：<https://javascript.plainenglish.io/object-oriented-javascript-styles-and-nodes-e86a2d72f87?source=collection_archive---------13----------------------->

![](img/0d5fa1ba3a63ccfbb25a64736341065e.png)

Photo by [Jordane Mathieu](https://unsplash.com/@mat_graphik?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将研究 DOM 以及插入和复制节点。

# 修改样式

我们可以改变元素的样式。

例如，我们可以写:

```
const p = document.getElementById('closer');
p.style.border = "1px solid green";
```

然后，我们将 p 元素的边框设置为绿色。

我们也可以用`fontWeight`属性设置字体粗细:

```
p.style.fontWeight = 'bold';
```

我们可以用`cssText`属性设置样式:

```
p.style.cssText; "border: 1px solid green; font-weight: bold;"
```

可以通过一些字符串操作来更改样式:

```
p.style.cssText += " border-style: dashed";
```

# 形式

我们可以通过获取表单元素来操作表单。

例如，我们可以写:

```
const input = document.querySelector('input[type=text]');
```

获取第一个将`type`属性设置为文本的输入。

然后我们就可以用`input.name`得到 name 属性了。

我们可以通过以下方式设置输入到框中的值:

```
input.value = 'abc';
```

我们可以通过以下方式获得表单的按钮:

```
document.querySelector("button")
```

# 创建新节点

我们可以用`createElement`方法创建新的节点。

例如，我们可以写:

```
const p = document.createElement('p');
p.innerHTML = 'foo';
```

创建一个内容为`'foo'`的`p`元素。

我们可以用`style`属性获得样式:

```
p.style
```

我们可以赋予它新的风格:

```
p.style.border = '2px dotted green';
```

然后我们可以把它连接到一个像身体一样的节点上:

```
document.body.appendChild(p);
```

`appendChild`将该节点附加为`body`的最后一个子节点。

# 仅 DOM 方法

我们还可以使用 DOM 方法来添加文本节点和样式。

例如，我们可以使用`createTextNode`来创建文本节点:

```
const p = document.createElement('p');
const text = document.createTextNode('something'); p.appendChild(text);
```

我们创建了一个 p 元素，并将文本节点`'something'`附加到它上面。

我们也可以用一个文本节点调用`appendChild`,将其添加为子节点:

```
const str = document.createElement('strong');
str.appendChild(document.createTextNode('bold'));
```

我们创建了一个`strong` 元素并向其中添加了文本`bold`，

# cloneNode()方法

我们可以用`cloneNode`方法克隆一个节点。

例如，我们可以写:

```
const el = document.querySelector('p');
document.body.appendChild(el.cloneNode(false));
```

我们得到一个 p 元素，用`cloneNode`克隆它，然后附加到主体上。

浅抄是没有任何子的。`false`参数会使副本变浅。

为了在复制所有子节点的情况下进行深度复制，我们可以将`cloneNode`的参数改为`true`:

```
const el = document.querySelector('p');
document.body.appendChild(el.cloneNode(true));
```

# insertBefore()方法

我们可以使用`insertBefore`方法在给定节点之前添加一个子节点。

例如，我们可以写:

```
document.body.insertBefore(
  document.createTextNode('first node'),
  document.body.firstChild
);
```

我们调用`inserBefore`在`body`的第一个子节点前插入一个文本节点。

![](img/20772f77c5ac0790acddf9822cdd4915.png)

Photo by [Manuel Sardo](https://unsplash.com/@manuelsardo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用各种方法添加和删除节点。

它们的样式可以用`style`属性来改变。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**