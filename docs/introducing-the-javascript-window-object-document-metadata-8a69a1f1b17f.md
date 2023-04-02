# 介绍 JavaScript 窗口对象—文档元数据

> 原文：<https://javascript.plainenglish.io/introducing-the-javascript-window-object-document-metadata-8a69a1f1b17f?source=collection_archive---------4----------------------->

![](img/8a91601924dedea24750e62edec09849.png)

Photo by [Robert Anasch](https://unsplash.com/@diesektion?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

`window`对象是一个全局对象，具有与当前 DOM 文档相关的属性，即浏览器选项卡中的内容。

在这一部分中，我们将看到`window.document`对象的更多属性以及如何使用它们。

# window.document.compatMode

`document.compatMode`属性是一个只读属性，返回文档是以古怪模式还是标准模式呈现。Quirks 模式适用于较旧的页面，其中浏览器必须模拟早期浏览器(如 Netscape 和 Internet Explorer)的专有行为，以便它们可以在所有浏览器中正确呈现。这是必要的，因为在万维网的早期，没有标准，所以每个浏览器制造商都将他们自己的渲染行为添加到他们自己的浏览器中。

相反，在完全标准模式下，页面的呈现行为遵循 W3C 制定的 HTML 和 CSS 标准。还有一种几乎是标准的模式，在这种模式下，只实现了很少一部分奇怪的东西。浏览器呈现的模式由页面顶部的 DOCTYPE 声明决定。在大多数现代网页中，应该是`<!DOCTYPE html>`，这是 HTML5 网页的推荐文档类型。这意味着网页符合 HTML5 标准，并且将以标准模式呈现。DOCTYPE 应该在网页代码的第一行。否则，它将在 Internet Explorer 9 或更早版本上触发 quirks 模式的渲染。DOCTYPE 的唯一目的是在怪癖和标准模式之间切换。

如果我们提供一个在`Content-Type` HTTP 头中带有`application/xhtml+xml` MIME-type 的页面，那么你不需要一个 DOCTYPE 来启用标准模式，因为所有这些文档总是使用完全标准模式。在 Internet Explorer 8 中，这将显示一个下载对话框，因为它不知道如何处理这种 MIME 类型的文件。但是，Internet Explorer 9 或更高版本支持此 MIME 类型。

如果我们像这样运行:

```
console.log(document.compatMode);
```

我们应该得到类似`‘CSS1Compat’`的东西。`CSS1Compat`表示文档以无特征或“标准”模式或有限特征呈现，也称为“近乎标准”模式。该属性的另一个可能值是`BackCompat`，这意味着文档以 quirks 模式呈现。

# 窗口.文档.内容类型

`document.contentType`是一个只读属性，它返回文档呈现的 MIME 类型。它可能来自 HTTP 头或其他 MIME 信息源。浏览器或扩展也可以对 MIME 类型进行自动类型转换。设置`meta`元素不能改变这个属性。

我们可以在下面的代码中使用它:

```
console.log(document.contentType);
```

我们应该从上面的`console.log`语句中得到类似`text/html`的东西。

# window.document.doctype

为了获得与当前打开的文档相关联的文档类型声明(DTD ),我们可以使用`document.doctype`属性。这是一个只读属性，它应该有一个实现`DocumentType`接口的对象。

`DocumentType`接口具有以下属性。`name`属性是一个字符串，其名称在网页代码中的 DOCTYPE 之后。例如，如果我们有`<!DOCTYPE HTML>`，那么我们得到`‘html’`作为值。

`publicId`属性是一个字符串。例如，对于旧的 HTML 4 页面，它是`'-//W3C//DTD HTML 4.01//EN’`，而对于 HTML 5，它是空的。对于旧的 HTML 4 页面，`systemId`是一个具有 DTD URI 的字符串，而对于 HTML5 是一个空字符串。

例如，如果我们运行以下代码:

```
console.log(document.doctype.name);
console.log(document.doctype.publicId);
console.log(document.doctype.systemId);
```

在第一个`console.log`语句中，我们返回`'html'`，而另外两个语句将得到空字符串。

![](img/1d3a00d020a1840458dbefc1b5e47ecf.png)

Photo by [Bernd Klutsch](https://unsplash.com/@bk71?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# window . document . document element

`document.documentElement`属性返回`Element` 对象，它是`document`的根元素。对于 HTML 文档，这应该是`html`元素。我们还可以使用这个属性来获取 XML 文档的根元素，无论根元素是什么，都应该返回。

如果我们运行下面的代码:

```
console.log(document.documentElement);
```

我们应该得到类似下面的输出:

```
<html><head>
  <meta http-equiv="content-type" content="text/html; charset=UTF-8">
  <title></title>
  <meta http-equiv="content-type" content="text/html; charset=UTF-8">
  <meta name="robots" content="noindex, nofollow">
  <meta name="googlebot" content="noindex, nofollow">
  <meta name="viewport" content="width=device-width, initial-scale=1"><script type="text/javascript" src="/js/lib/dummy.js"></script><link rel="stylesheet" type="text/css" href="/css/result-light.css"><style id="compiled-css" type="text/css">

  </style><!-- TODO: Missing CoffeeScript 2 --><script type="text/javascript">//<![CDATA[window.onload=function(){

console.log(document.documentElement);}//]]></script></head>
<body><script>
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

一个`Element`对象还具有以下属性:

*   `attributes` —一个只读属性，具有包含 HTML 元素的已分配属性的`NamedNodeMap`对象
*   `classList` —具有类属性列表的只读属性。
*   `className` —只读字符串属性，具有元素的类
*   `cilentHeight` —具有元素内部高度的只读数字属性
*   `clientLeft` —只读数字属性，具有元素左边框的宽度
*   `clientTop` —只读数字属性，具有元素上边框的宽度
*   `cilentWidth` —具有元素内部宽度的只读数字属性
*   `computedName` —只读字符串属性，其标签对可访问性公开。
*   `computedRole` —只读字符串属性，具有应用于特定元素的 ARIA 角色。
*   `id` —具有元素 ID 的字符串
*   `innerHTML` —包含元素内容的 HTML 标记的字符串
*   `localName` —只读字符串，包含元素限定名的本地部分
*   `namespaceURI` —一个只读属性，具有元素的名称空间 URL，如果没有名称空间，则为`null`
*   `nextElementSibling` —只读属性，具有另一个`Element`对象，表示紧跟在当前元素之后的元素，如果没有同级节点，则为`null`。
*   `outerHTML` —一个字符串，表示元素的标记，包括其内容。也可以设置它，使它用我们分配给这个属性的 HTML 替换元素的内容
*   `part` —具有使用`part`属性设置的零件标识符。
*   `prefix` —只读字符串属性，具有元素的名称空间前缀，如果没有指定前缀，则为`null`
*   `previousElementSibling` —一个只读属性，它有另一个`Element`对象，表示当前元素之前的元素，如果没有这样的节点，则为`null`。
*   `scrollHeight` —只读数字属性，具有元素的滚动视图高度
*   `scrollLeft` —一个数字属性，具有元素的左滚动偏移量。这可以是一个 getter 或 setter
*   `scroolLeftMax` —只读数字属性，具有元素可能的最大向左滚动偏移量
*   `scrollTop` —一个数字属性，具有垂直滚动的文档顶部的像素数
*   `scrollTopMax` —一个只读属性，具有该元素可能的最大顶部滚动偏移量
*   `scrollWidth` —只读数字属性，具有元素的滚动视图宽度
*   `shadowRoot` —一个只读属性，具有由元素托管的开放阴影根，或者如果不存在开放阴影根，则为`null`
*   `openOrClosedShadowRoot` —只读属性，仅适用于 WebExtensions，其影子根由元素托管，与状态无关。
*   `slot` —插入元素的阴影 DOM 槽的名称
*   `tabStop` —一个布尔属性，指示元素是否可以通过按 tab 键接收输入焦点
*   `tagName` —一个只读的，包含具有给定元素的标记名的字符串

例如，我们可以使用下面代码中的一些属性:

```
document.documentElement.id = 'hello';
console.log(document.documentElement);
console.log(document.documentElement.clientWidth);
console.log(document.documentElement.id);
console.log(document.documentElement.scrollWidth);
console.log(document.documentElement.tagName);
```

在上面的代码中，我们设置了`html`元素的 ID，获得了`html`元素的内部宽度、ID、滚动视图宽度和元素的标记名。如果我们运行`console.log(document.documentElement);`，我们应该得到类似这样的结果:

```
<html id="hello"><head>
  <meta http-equiv="content-type" content="text/html; charset=UTF-8">
  <title></title>
  <meta http-equiv="content-type" content="text/html; charset=UTF-8">
  <meta name="robots" content="noindex, nofollow">
  <meta name="googlebot" content="noindex, nofollow">
  <meta name="viewport" content="width=device-width, initial-scale=1"><script type="text/javascript" src="/js/lib/dummy.js"></script><link rel="stylesheet" type="text/css" href="/css/result-light.css"><style id="compiled-css" type="text/css">

  </style><!-- TODO: Missing CoffeeScript 2 --><script type="text/javascript">//<![CDATA[window.onload=function(){

document.documentElement.id = 'hello';
console.log(document.documentElement);
console.log(document.documentElement.clientWidth);
console.log(document.documentElement.id);
console.log(document.documentElement.scrollWidth);
console.log(document.documentElement.tagName);}//]]></script></head>
<body><script>
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

其他人应该得到这样的信息:

```
197
hello
197
HTML
```

在这一部分，我们将继续研究`window.document`对象的更多属性以及如何使用它们。我们查看了`contentType`、`doctype`、`documentElement`属性。

`contentType`让我们知道页面是以标准模式还是古怪模式呈现的。`doctype`属性为我们获取数据，这是 web 页面的 DOCTYPE 声明。

`documentElement`获取关于 HTML 或 XML 文档的根元素的信息，它应该是 HTML 文档的`html`元素。

返回的对象是一个`Element`对象，它有许多属性来获取关于页面的信息。