# JavaScript 中的事件委托

> 原文：<https://javascript.plainenglish.io/event-delegation-in-javascript-ef3b87f538e?source=collection_archive---------6----------------------->

![](img/d7087d088801b3f5e76697ea2ff0947f.png)

Image: [Pixabay](https://pixabay.com/photos/soap-bubbles-colorful-flying-3517247/)

利用事件冒泡来提高性能并简化代码。

向页面添加大量事件处理程序会对性能产生不利影响。缓解这一问题的一种常见技术是事件委托，在这种技术中，我们将单个事件处理程序添加到层次结构中某处的父元素中。这是由于通过 DOM 的事件流，我在我的[交互周期表](https://medium.com/javascript-in-plain-english/interactive-periodic-table-in-javascript-d7cf7177883a)中很好地使用了这种技术。在这篇文章中，我将使用周期表的简化版本来演示事件委托。

下面的屏幕截图显示了简化的元素周期表和当用户点击其中一个元素时显示的警告。

![](img/5bdae3b104ef5676cc84aab869bdc893.png)

整个 onclick/alert 看起来是非常简单的 JavaScript，但是请注意，这里有 118 个化学元素，每个都急切地等待有人点击它们。通过遍历数据来填充该表，为每一项添加一个`<td>`元素。在这个循环中，我们可以为每个`<td>`添加一个`onclick`处理程序，但是这既会增加填充表格的时间，也会占用更多的内存。

代码实际上是如何工作的，只需向`<table>`元素添加一个点击事件处理程序，并允许它的各种`<td>`元素的点击事件冒泡 DOM，这是一个更有效的解决方案。事件实际上通过`<html>`元素一路冒泡到文档本身，因此如果需要，您可以在整个页面级别使用事件委托。

您可以从 [Github 资源库获得该项目的源代码。](https://github.com/CodeDrome/event-delegation-javascript)

我不会列出整个代码，只列出**periodictabledisplay . js**文件中与事件委托相关的部分。这个文件包含一个名为`PeriodicTableDisplay`的类，它的构造函数包含这样一行:

请注意，事件处理函数只添加到表中一次，而不是添加到单个的`<td>`元素中。这是事件委托模式的核心。

现在让我们看看`_elementClicked`事件处理函数。

传递给事件处理程序的事件对象有一个名为`target`的属性，这是实际被点击的元素，所以我们需要弄清楚用户是点击了一个实际的化学元素还是可以忽略的表格的其他部分。我们通过检查名为`elementcell`的 CSS 类的目标来做到这一点。我们还有额外的复杂性，每个`<td>`包含一个`<span>`来显示化学符号，可以点击其中的任何一个。如果用户点击了一个元素，它的父元素有 CSS 类`elementcell`，那么它就是一个`span`并且`target`被重新分配给它的父元素。

这是与事件委托相关的代码的结尾，但为了完整起见，如果用户单击一个实际的化学元素，我们会获取它的原子序数，汇集一串细节，并显示为一个警告。为此，我使用了数据属性，这里讨论的是使用 HTML5 数据属性和 JavaScript 。

## 结论

正如您所看到的，事件委托很容易实现。如果您发现自己为某个特定类型的事件添加了大量的处理程序，那么可以这样做:

*   向引发事件的元素的父元素添加一个事件处理程序
*   在处理函数中，标识通过事件的目标属性引发事件的实际元素。您可能需要查看它的`id`、`class`或`data- attributes`，并且可能使用一个`if/else`或`switch/case`构造。

## **用简单英语写的 JavaScript**

你知道我们有三份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)找到所有内容的链接！