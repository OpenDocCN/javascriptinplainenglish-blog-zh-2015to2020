# DOM 操作简介

> 原文：<https://javascript.plainenglish.io/introduction-to-dom-manipulation-4677595147c8?source=collection_archive---------7----------------------->

![](img/336940bd6e8d9dde797133164805922c.png)

Photo by [Brooke Lark](https://unsplash.com/@brookelark?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是世界上最流行的编程语言之一。为了有效地使用它，我们必须了解它的基本知识。

在本文中，我们将研究 DOM 操作的基础。

# 大教堂

DOM 代表文档对象模型。这是一个 JavaScript 节点对象树。

例如，如果我们有下面的 HTML:

```
<!DOCTYPE html>
<html lang="en"> <head>
    <title>HTML</title>
  </head> <body>
    <!-- some content-->
  </body></html>
```

它将被浏览器解析成树形结构。

HTML 元素将被解析成`documebt`对象。

并且`body`将被存储在`document.body`中。

# 节点对象类型

有几种节点类型。

它们包括:

*   `DOCUMENT_NODE`——`window.document`
*   `ELEMENT_NODE` — body，a，p，h1 等。
*   `ATTRIBUTE_NODE` — `id='foo'`，`class='foo'`
*   `TEXT_NODE` —标签之间的文本。
*   `DOCUMENT_FRAGMENT_NODE`—`document.createDocumentFragment()`返回的任何内容
*   `DOCUMENT_TYPE_NODE` — `<!doctype html>`

不是实例，我们可以写:

```
document.ELEMENT_NODE
```

以获得为 1 的`ELEMENT_NODE`的值。

`Node`对象具有所有的节点常数值。

从`Node`对象我们可以看到`ELEMENT_NODE`是 1。

`TEXT_NODE`是 3，`ATTRIBUTE_NODE`是 2，`DOCUMENT_NODE`是 9，`DOCUMENT_FRAGMENT_NODE`是 11，`DOCUMENT_TYPE_NODE`是 10。

# 子节点对象

`HTMLElement`对象通过`Element`对象继承自`Node`对象。

其他如`Attr`、`Text`、`HTMLDocument`、`DocumentFragment`都是从`Node`插入。

例如，我们可以写:

```
const body = document.querySelector('body');
```

我们可以循环遍历这些键，如下所示:

```
for (const key in body) {
  console.log(key);
}
```

除了`HTMLElement`属性之外，我们还应该看到节点属性。

# 节点的属性和方法

每个节点都具有以下属性:

*   `childNodes`
*   `firstChild`
*   `lastChild`
*   `nextSibling`
*   `nodeName`
*   `nodeType`
*   `nodeValue`
*   `parentNode`
*   `previousSibling`

它们允许我们穿越不同的节点。

`childNodes`有子节点。

`firstChild`有了第一个孩子。

`lastChild`有最后一个孩子。

`nextSibling`有下一个同级节点。

`nodeName`有节点的名称。

`nodeType`有节点的类型。

`parentNode`有当前节点的父节点。

`previousSibling`具有当前节点的上一个同级。

方法包括:

*   `appendChild()`
*   `cloneNode()`
*   `compareDocumentPosition()`
*   `contains()`
*   `hasChildNodes()`
*   `insertBefore()`
*   `isEqualNode()`
*   `removeChild()`
*   `replaceChild()`

`appendChild`添加新的孩子。

`cloneNode`克隆当前节点并返回。

`compareDocumentPosition`比较各节点的位置。

`contains`检查该节点是否有另一个节点作为子节点。

`insertBefore`在当前节点前添加一个节点。

`isEqualNode`检查是否相同。

`removeChild`删除给定的子节点。

用另一个子元素替换一个子元素。

记录方法包括:

*   `document.createElement()`
*   `document.createTextNode()`

`createElement`创建新元素顾名思义，`createTextNode`创建可以放入元素的文本节点。

HTML 元素属性包括:

*   `innerHTML`
*   `outerHTML`
*   `textContent`
*   `innerText`
*   `outerText`
*   `firstElementChild`
*   `lastElementChild`
*   `nextElementChild`
*   `previousElementChild`
*   `children`

`innerHTML`拥有元素内部的 HTML 内容。

`outerHTML`拥有`innerHTML`的所有 HTML 内容，还有封闭标签。

`textContent`拥有元素内部的文本内容。

`innerText`拥有元素内部的文本内容。

`outerText`包含来自`innerText`的内容加上封闭标签。

`firstElementChild`的第一个子节点是 HTML 元素。

`lastElementChild`的最后一个子节点是一个 HTML 元素。

`nextElementChild`当前节点后有元素节点。

`previousElementChild`当前节点之前有元素节点。

`children`有当前节点的子节点。

HTML 元素对象有`insertAdjacentHTML`方法根据指定的位置插入一个节点。

# 标识节点的类型和名称

我们可以用属性`nodeType`标识类型，用属性`nodeName`标识名称。

例如，`document.nodeName`返回`'#document`，而`document.nodeType`返回 9，这意味着它是一个`DOCUMENT_NODE`。

# 获取节点的值

对于大多数类型的节点,`nodeValue`属性返回`null`。

但是我们可以用它从`Text`和`Comment`节点中提取文本。

例如，如果我们有:

```
<a>hi</a>
```

然后:

```
console.log(document.querySelector('a').firstChild.nodeValue);
```

日志`'hi'`。

![](img/68600449653fb548f2a03a43a39e4813.png)

Photo by [The Lucky Neko](https://unsplash.com/@theluckyneko?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

DOM 是一种树形结构，由浏览器从 HTML 解析成 JavaScript 对象。

我们可以从它们那里获得各种属性，还可以通过 DOM 节点可用的各种属性和方法用 JavaScript 操纵它们。

此外，我们可以通过可用的方法来遍历节点。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**