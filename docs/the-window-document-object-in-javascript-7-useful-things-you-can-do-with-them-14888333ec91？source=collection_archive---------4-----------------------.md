# JavaScript 中的窗口和文档对象——你可以用它们做 7 件有用的事情

> 原文：<https://javascript.plainenglish.io/the-window-document-object-in-javascript-7-useful-things-you-can-do-with-them-14888333ec91?source=collection_archive---------4----------------------->

## 普通 JavaScript

![](img/580ef648bfce37c4b6aa4d3029db3eff.png)

Source: [Unsplash](https://unsplash.com/photos/lDEW4qMiizc)

**文档**和**窗口**对象是我们可以用 JavaScript 访问的两个父对象。文档对象本身从属于窗口对象。我想每个人都知道它们，因为它们无处不在，尤其是在用普通 javascript 开发 web 应用程序时。

本文旨在向您展示这两者的一些实用功能。没有必要更详细地介绍这两者到底是什么，我现在给你一些实际的例子，你不需要太多以前的知识就可以使用。

# 处理浏览器中的右键单击—上下文菜单

**上下文菜单**是一个 DOM 事件，当用户右键单击上下文菜单想要调用的 DOM 元素时触发。上下文菜单就是当用户右键单击网页时默认显示的菜单，除非网页以不同的方式处理事件，我们马上就要这样做。因此，尽管可能不知道它的名字，但这个小菜单看起来会因操作系统的不同而有所不同，并提供诸如“图像另存为”、“检查”、“重新加载”&“返回”等功能。
基本上，当用户右击时，我们可以使用 DOM 中的`**contextmenu**`事件来执行我们自己的动作。

## 右键单击并阻止显示默认上下文菜单

```
<div *id*=”nocontext”>No context menu available for this</div><script>
noContext = document.getElementById(‘nocontext’)noContext.addEventListener(‘contextmenu’, (event) => {
  event.preventDefault()
})
</script>
```

因此，我们为我们的

设置了一个 EventListener，它应该在上下文菜单被调用时执行，即右键单击。它返回一个事件，我们可以用标准函数 **preventDefault** 来阻止这个事件。

默认情况就像右键单击上下文菜单一样。但是有了 **preventDefault** 我们就阻止了它。所以什么都没发生。

当然，我们不仅可以阻止标准事件，还可以在用户右键单击时执行各种代码。然后，我们可以将它与 preventDefault 结合起来，提供我们自己的上下文菜单，例如:

```
contextAvailable.addEventListener(‘contextmenu’, (e) => {
  console.log(‘you rightclicked this’)
  e.preventDefault()
})
```

# 文档的设计模式

直到这篇文章的研究，这是完全未知的，但实际上这个功能可以非常实用。例如，您可以将它用于内容管理系统，或者在没有代码的情况下快速编辑，以可视化网站上潜在的内容更改。
因为 designMode 使我们可以编辑我们在浏览器中看到的网站的所有内容，而无需更改 devtools 的 HTML 代码。

你现在就可以尝试一下。只需转到任何网站，打开控制台，并输入以下内容:

`document.designMode = "on"`

现在，您可以像编辑 Word 文档一样简单地编辑显示的网站。很酷吧。
**设计模式**只知道两种状态:**开和关**。所以，如果你想停用它，只要做以下事情:

`document.designMode = "off"`

如果你想保护单个元素不改变内容，你可以给它们以下属性:(然后你只能**完全移除**它们，而不能**操作**它们)

`<i contenteditable = "false">This is not editable.</i>`

# 用代码复制一些东西到用户的剪贴板

最初，众所周知的 **document.execCommand()** 就是为了这个目的而设计的，但是现在被认为已经过时了。
现在，剪贴板 API 可用于使用 JavaScript 自动复制内容。
可以通过**导航器**对象使用，导航器对象从属于**窗口**对象。

让我们看一个例子，在按下按钮后，我们将输入字段中输入的文本复制到剪贴板:

# 获取文档的状态

特别是对于更复杂和性能优化的 web 应用程序的开发，应该对文档的加载时间和一般行为进行调查，以便找到潜在的弱点或提高性能和 UX。

因此，通过 **document.readyState** ，我们可以访问文档的当前状态，并根据结果做出进一步的决策。

下面是可能的结果， **document.readyState** 可以返回:

*   未初始化—尚未开始加载
*   正在加载—正在加载
*   已加载—已加载
*   交互式—已加载足够的内容，用户可以与之交互
*   完成—满载
    来源:[https://www.w3schools.com/jsref/prop_doc_readystate.asp](https://www.w3schools.com/jsref/prop_doc_readystate.asp)

如果你在实践中尝试一下，会有一些有趣的结果。为此，我创建了一个小的 HTML 样板文件，用它我们可以在不同的地方输出状态。每个外部脚本只包含`console.log(document.readyState)`

以下是我们网站按时间顺序记录的内容:

*   **正常:**加载
*   **脚本标签:**加载
*   **延期:**互动
*   **异步:**交互
*   **加载正文:**完成

包含外部脚本的不同方式在这里都可以正常工作。但有趣的是，如果我们在页面上只包含一个大图片，图片的加载时间也会延迟 body 标签的 **onload** 事件。

因此，在任何情况下，如果我们用 onload-event 输出状态，状态都是“完成”。

# 使用焦点/活动元素

当涉及到网站用户实际可以与之交互的元素时，例如按钮、输入和文本区域，这些可以具有所谓的焦点。如果输入有焦点，我们在键盘上输入一些东西，它会被写入输入字段。如果该字段没有焦点，则不会写入输入字段。我想每个人都知道重点是什么。

## 确定哪个元素具有焦点

也许你已经知道了 **onfocus** 属性，它是一个元素获得焦点时的事件。然后我们可以用它来创建很酷的 CSS 动画，或者，如果我们想分析用户行为，保存这个事件。

同样，我们可以确定哪个元素具有焦点，而不必给所有元素赋予 **onfocus** 属性。
我们可以通过以下方式做到这一点:

```
var x = document.activeElement.idconsole.log(x)
```

因此，我们可以获得拥有焦点/处于活动状态的元素的 id。

## 手动改变焦点

特别是对于 UX，有时减轻用户选择元素的负担会很有帮助。例如，如果他在一个表单中输入错误，我们希望他在每个输入字段中更改输入:

```
document.getElementById(‘input1’).focus()
```

# 读取元素的当前样式

在大型动态 web 应用程序中，我们不断地改变 DOM 中任何元素的活动样式。
我们也可以随时输出这个到 two，所以检查一下我们的元素有哪些 CSS 属性和值。

1.  `**element.style**`也可用于读取样式，但用于编辑它们
2.  `**Window.getComputedStyle()**` 用于获取当前的样式

因此，使用最后一个函数，我们可以获得包含元素所有样式的整个对象:

```
let box = document.getElementById(‘box’)console.log(window.getComputedStyle(box)) // CSSStyleDeclaration{}
```

但是我们也可以更精确地输出单个属性和相应的值:

```
let box = document.getElementById(‘box’)console.log(
  window.getComputedStyle(box)
  .getPropertyValue(‘background-color’) // E.g. rgb(0, 128, 0)
)
```

# 处理浏览器和显示的网站的大小

当然，在现代 web 开发中，大小是无法回避的。这是你需要的:

*   `**window.outerWidth**`
*   `**window.outerHeight**`

这两个返回浏览器窗口本身的尺寸，因此网页是否被例如开发者工具减小尺寸并不重要。

因此，当真正确定网站当前显示的维度时，可以使用以下功能:

*   `**window.innerWidth**`
*   `**window.innerHeight**`

例如，您可以这样全屏显示一个元素:

```
elem.style.width = window.innerWidth + ‘px’
```

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物: [**AI in Plain English**](https://medium.com/ai-in-plain-english) ，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****