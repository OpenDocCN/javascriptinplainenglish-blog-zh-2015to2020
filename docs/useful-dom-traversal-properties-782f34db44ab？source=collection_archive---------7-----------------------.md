# 有用的 DOM 遍历属性

> 原文：<https://javascript.plainenglish.io/useful-dom-traversal-properties-782f34db44ab?source=collection_archive---------7----------------------->

![](img/bf24d27c39dee377c589fa8de156829c.png)

Photo by [Håkon Helberg](https://unsplash.com/@hakonbrakon?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

客户端 JavaScript 的主要用途是动态操作网页。我们可以用 DOM 节点对象可用的 DOM 遍历方法和属性来做到这一点。

对于任何给定的节点，获取子元素和兄弟元素都很容易，因为 DOM 节点对象中内置了属性来实现这一点。下面是一个 DOM 节点对象的属性，用于获取父节点、子节点和兄弟节点或元素。

# 子节点

属性让我们获得一个节点对象的子节点。它返回一个`NodeList`对象，这是一个类似数组的对象，我们可以用`for`和`for...of`循环遍历它，并用 spread 操作符进行扩展。这是一个只读属性。

例如，我们可以如下使用它:

```
const divChildren = document.querySelector('div').childNodes;for (let c of divChildren) {
  console.dir(c);
  console.log(c.textContent);
}
```

我们得到文本节点和作为子节点的`p`节点，所以换行符和`p`元素都被包含在内。上面的`console.log`和`console.dir`语句应该反映了这一点。

我们可以改变上面的例子来改变文本的颜色。例如，我们可以写:

```
const divChildren = document.querySelector('div').childNodes;for (let c of divChildren) {
  if (c.style) {
    c.style.color = 'red';
  }
}
```

将所有`p`元素变为红色。我们必须检查`style`属性，因为文本节点没有`style`属性。

# 第一个孩子

属性获取给定节点对象的第一个子节点。例如，如果我们有以下 HTML:

```
<div>
  <p>
    foo
  </p>
  <p>
    bar
  </p>
  <p>
    baz
  </p>
</div>
```

我们得到第一个子节点是一个带有换行符的文本节点，包含以下 JavaScript 代码:

```
const firstChild = document.querySelector('div').firstChild;console.log(firstChild)
```

此属性是只读属性。如果没有子节点，那么它将返回`null`。

# 最后一个孩子

属性获取给定节点对象的第一个子节点。例如，如果我们有以下 HTML:

```
<div>
  <p>
    foo
  </p>
  <p>
    bar
  </p>
  <p>
    baz
  </p>
</div>
```

我们得到最后一个子节点是一个带有换行符的文本节点，其 JavaScript 如下:

```
const lastChild = document.querySelector('div').lastChild;console.log(lastChild)
```

此属性是只读属性。如果没有子节点，那么它将返回`null`。

# 下一个

`nextSibling`属性是一个只读属性，表示树中的下一个节点，如果没有节点，则为`null`。

例如，如果我们有以下 HTML:

```
<div>
  foo
</div>
<div>
  bar
</div>
```

那么我们可以如下使用`nextSibling`属性:

```
const nextSibling = document.querySelector('div').nextSibling;
console.log(nextSibling)
```

我们应该从`nextSibling`中获得一个带有新行字符的文本节点作为节点。

为了获得 HTML 中的第二个 div，我们可以如下链接`nextSibling`属性:

```
const barNode = document.querySelector('div').nextSibling.nextSibling;
barNode.style.color = 'red';
```

在上面的代码中，我们将第二个 div 的颜色设置为红色。

# parentNode

`parentNode`属性是一个只读属性，它获取给定节点的父节点，如果不存在，则获取`null`。节点可以包括 HTML 元素或其他节点，如文本节点或注释节点。

例如，给定以下 HTML:

```
<div>
  foo
</div>
<div>
  bar
</div>
```

我们可以编写以下代码来获取父节点:

```
const parentNode = document.querySelector('div').parentNode;console.log(parentNode);
```

`console.log`应该从作为父节点的中获取`body`节点。

一旦有了父节点，我们就可以对它做一些事情。例如，我们可以写:

```
const parentNode = document.querySelector('div').parentNode;parentNode.style.color = 'red';
```

把所有的文字都改成红色。

![](img/b31f1ccae7857269881ca6a41afa9d97.png)

Photo by [Colin Maynard](https://unsplash.com/@invent?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# parentElement

`parentElement`属性是一个只读属性，用于获取给定节点的父元素。如果节点没有父节点或者父节点不是 HTML 元素，那么它返回`null`。

例如，给定以下 HTML:

```
<div>
  bar
  <div id='foo'>
    foo
  </div>
</div>
```

我们可以编写以下 JavaScript 来获取 ID 为`foo`的元素的父元素:

```
const parentElement = document.querySelector('#foo').parentElement;parentElement.style.color = 'red';
```

在上面的代码中，我们获得了`foo`的父元素，并将其`color`样式设置为红色，因此我们应该看到所有颜色为红色的文本，因为`foo`的父元素是我们的外部 div。

# 前兄弟姐妹

`previousSibling`属性是一个只读属性，返回 DOM 树中的前一个节点，如果不存在则返回`null`。

例如，如果我们有给定的 HTML:

```
<div>
  foo
</div>
<div id='bar'>
  bar
</div>
```

那么我们可以得到前一个兄弟姐妹，如下所示:

```
const previousSibling = document.querySelector('#bar').previousSibling;console.log(previousSibling);
```

我们可以看到前一个兄弟节点是带有换行符的文本节点。

要获得带有单词“foo”的 div，我们可以写:

```
const foo = document.querySelector('#bar').previousSibling.previousSibling;foo.style.color = 'red';
```

第一行获取包含单词“foo”的 div。然后最后一行将其设置为红色。

# 文本内容

`textContent`是一个可读可写的属性，它允许我们设置元素及其所有后代的文本内容。例如，给定一个空的`p`元素:

```
<p></p>
```

我们可以通过编写以下代码将其文本内容设置为“foo ”:

```
const p = document.querySelector('p')
p.textContent = 'foo';
```

DOM 节点对象方法对于获取父元素、子元素和兄弟元素非常有用。大多数属性都是只读的，因为它们是在其他地方设置的。对于获取各种节点对象的属性，我们必须小心不要选择错误的节点类型。这意味着我们不能假设所有的节点都是 HTML 元素。