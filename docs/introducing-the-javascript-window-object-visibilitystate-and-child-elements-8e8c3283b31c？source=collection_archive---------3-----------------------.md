# JavaScript 窗口对象简介——visibility state 和子元素

> 原文：<https://javascript.plainenglish.io/introducing-the-javascript-window-object-visibilitystate-and-child-elements-8e8c3283b31c?source=collection_archive---------3----------------------->

![](img/93bb41ae70cb0b027c7132c6ca49a532.png)

Photo by [Carlo Villarica](https://unsplash.com/@sobermusings?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

`window`对象是一个全局对象，具有与当前 DOM 文档相关的属性，即浏览器选项卡中的内容。

它包括构造函数、值属性和方法。有一些方法可以操作当前的浏览器标签，比如打开和关闭新的弹出窗口等。

在本文中，我们将看看`window.document`对象的属性，包括`visibilityState`、`childElementCount`和`firstElementChild`属性。

# windows . document . visibility state

`visibilityState`属性是一个只读属性，它返回`document`对象的可见性状态，该元素在该对象的上下文中可见。这有助于了解文档是在背景中可见还是在不可见的选项卡中可见，或者只是为了预渲染而加载。它是一个字符串属性，可能有以下值:

*   `'visible'` —页面内容至少可以部分可见。这意味着该页面位于非最小化窗口的前景选项卡中。
*   `'hidden'` —页面对用户不可见。这意味着页面要么在背景标签中，窗口已经最小化，要么在锁定屏幕后面。
*   `'prerender'` —页面预渲染，但用户不可见。文档可能以此状态开始，但不能转换到另一个值。这个值从标准中删除了，所以许多现代浏览器可能不会返回它。

例如，为了观察浏览器选项卡的可见性，我们可以编写以下代码:

```
console.log(document.visibilityState);
document.addEventListener('visibilitychange', () => {
  console.log(document.visibilityState);
})
```

然后，当我们将浏览器的开发控制台从浏览器选项卡中分离出来时，我们在选项卡之间切换，同时查看来自浏览器开发控制台的`console.log`输出，而选项卡不可见。当我们这样做时，我们应该看到，当浏览器选项卡可见时，它会记录`'visible'`，当您切换出该选项卡时，您会将`visibilityState`从另一个选项卡记录下来，然后`console.log`输出将变成`hidden`。

# window.document. `childElementCount`

`childElementCount`是一个只读的数字属性，它返回一个无符号的长数字，表示给定元素中子元素的数量。例如，如果我们运行:

```
console.log(document.childElementCount);
```

那么我们应该得到 1，因为`document`的唯一子元素是`html`元素。

# 窗口.文档.`children`

要获取`document`的子元素，我们可以使用`children`属性，这是一个只读属性，它返回一个类似 HTMLCollection 数组的对象，其中包含被调用的`document`对象的所有子元素。例如，如果我们有以下 HTML 代码:

```
<div>
  A
</div>
<div>
  B
</div>
<div>
  C
  <div>
    D
  </div>
</div>
```

然后我们可以用`for...of`循环遍历 HTMLCollection 对象，因为它是一个类似数组的对象，如下面的代码所示:

```
for (const child of document.children) {
  const {
    tagName,
    outerHTML,
    outerText
  } = child
  console.log(tagName);
  console.log(outerHTML);
  console.log(outerText);
}
```

然后我们从`console.log`语句中得到输出。首先，我们得到:

```
HTML
```

从第一条`console.log`线开始。然后，从第二个`console.log`语句中，我们得到以下输出:

```
<html><head>
  <meta http-equiv="content-type" content="text/html; charset=UTF-8">
  <title></title>
  <meta http-equiv="content-type" content="text/html; charset=UTF-8">
  <meta name="robots" content="noindex, nofollow">
  <meta name="googlebot" content="noindex, nofollow">
  <meta name="viewport" content="width=device-width, initial-scale=1"><script type="text/javascript" src="/js/lib/dummy.js"></script><link rel="stylesheet" type="text/css" href="/css/result-light.css"><style id="compiled-css" type="text/css">

  </style><!-- TODO: Missing CoffeeScript 2 --><script type="text/javascript">//<![CDATA[window.onload=function(){

for (const child of document.children) {
  const {
    tagName,
    outerHTML,
    outerText
  } = child
  console.log(tagName);
  console.log(outerHTML);
  console.log(outerText);
}}//]]></script></head>
<body>
    <div>
  A
</div>
<div>
  B
</div>
<div>
  C
  <div>
    D
  </div>
</div><script>
    // tell the embed parent frame the height of the content
    if (window.parent && window.parent.parent){
      window.parent.parent.postMessage(["resultsFrame", {
        height: document.body.getBoundingClientRect().height,
        slug: ""
      }], "*")
    }// always overwrite window.name, in case users try to set it manually
    window.name = "result"
  </script></body></html>
```

然后从第三条`console.log`线我们得到:

```
A
B
C
D
```

因为我们从`document`对象中获取`children`属性，我们将`html`元素作为唯一的子元素，所以我们只有一个来自`children`属性的对象。

![](img/ecfd184938d858297a67cce8f7ec3e85.png)

Photo by [🇸🇮 Janko Ferlič — @specialdaddy](https://unsplash.com/@thepootphotographer?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# window.document. `firstElementChild`

`firstElementChild`属性是一个只读属性，它返回`document`的第一个子节点。它只有一个子元素，即`html`元素，所以这就是将要返回的内容。返回的对象将是一个`Element`对象，它具有 HTML 元素的属性。如果没有孩子，则返回`null`。`Element`对象还具有以下属性:

*   `attributes` —一个只读属性，具有包含 HTML 元素的分配属性的`NamedNodeMap`对象
*   `classList` —具有类属性列表的只读属性。
*   `className` —只读字符串属性，具有元素的类
*   `cilentHeight` —只读数字属性，具有元素的内部高度
*   `clientLeft` —只读数字属性，具有元素左边框的宽度
*   `clientTop` —只读数字属性，具有元素上边框的宽度
*   `cilentWidth` —只读数字属性，具有元素的内部宽度
*   `computedName` —只读字符串属性，其标签对可访问性公开。
*   `computedRole` —只读字符串属性，具有应用于特定元素的 ARIA 角色。
*   `id` —具有元素 ID 的字符串
*   `innerHTML` —包含元素内容的 HTML 标记的字符串
*   `localName` —只读字符串，包含元素限定名的本地部分
*   `namespaceURI` —一个只读属性，具有元素的名称空间 URL，如果没有名称空间，则为`null`
*   `nextElementSibling` —一个只读属性，具有另一个`Element`对象，表示紧跟在当前元素之后的元素，如果没有同级节点，则为`null`。
*   `outerHTML` —一个字符串，表示元素的标记，包括其内容。也可以设置它，使它用我们分配给这个属性的 HTML 替换元素的内容
*   `part` —具有使用`part`属性设置的零件标识符。
*   `prefix` —只读字符串属性，具有元素的名称空间前缀，如果没有指定前缀，则为`null`
*   `previousElementSibling` —一个只读属性，它有另一个`Element`对象，表示当前元素之前的元素，如果没有这样的节点，则为`null`。
*   `scrollHeight` —只读数字属性，具有元素的滚动视图高度
*   `scrollLeft` —具有元素左滚动偏移量的数字属性。这可以是一个 getter 或 setter
*   `scroolLeftMax` —只读数字属性，具有元素可能的最大向左滚动偏移量
*   `scrollTop` —一个数字属性，具有垂直滚动的文档顶部的像素数
*   `scrollTopMax` —一个只读属性，具有该元素可能的最大顶部滚动偏移量
*   `scrollWidth` —只读数字属性，具有元素的滚动视图宽度
*   `shadowRoot` —一个只读属性，它具有由元素托管的开放阴影根，如果不存在开放阴影根，则为`null`
*   `openOrClosedShadowRoot` —只读属性，仅适用于 WebExtensions，其影子根由元素托管，与状态无关。
*   `slot` —插入元素的阴影 DOM 槽的名称
*   `tabStop` —一个布尔属性，指示元素是否可以通过按 tab 键接收输入焦点
*   `tagName` —只读，包含具有给定元素的标记名的字符串

例如，我们可以在下面的代码中使用它:

```
console.log(document.firstElementChild);
```

那么我们应该得到这样的结果:

```
<body>
    <div>
  A
</div>
<div>
  B
</div>
<div>
  C
  <div>
    D
  </div>
</div><script>
    // tell the embed parent frame the height of the content
    if (window.parent && window.parent.parent){
      window.parent.parent.postMessage(["resultsFrame", {
        height: document.body.getBoundingClientRect().height,
        slug: ""
      }], "*")
    }// always overwrite window.name, in case users try to set it manually
    window.name = "result"
  </script></body>
```

我们还可以获得`html` `Element`对象的单个属性，如下面的代码所示:

```
const {
  clientLeft,
  innerHTML,
  outerHTML
} = document.firstElementChild;console.log(clientLeft);
console.log(innerHTML);
console.log(outerHTML);
```

然后，我们应该得到元素`html`左边界的宽度 0，并得到最近 2 个控制台日志记录的文档的 HTML 内容。

对于`document`对象，我们有一些方便的属性，让我们通过使用它的方便属性来获取文档中的元素。属性让我们知道浏览器标签的文档是否可见。属性为我们获取了文档对象的子元素的数量，应该是 1，因为文档对象的唯一子元素是元素。属性应该得到文档对象的第一个子元素，应该是元素，因为它是文档对象唯一的子元素。