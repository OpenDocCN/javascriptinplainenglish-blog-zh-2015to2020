# 如何苗条地处理事件

> 原文：<https://javascript.plainenglish.io/svelte-tutorials-2-e965737edf63?source=collection_archive---------4----------------------->

## 处理事件的简单指南

![](img/3767883e374b49d0723a84856425a837.png)

Logo of the Svelte

这篇文章是苗条教程系列之一。

1.  [开始运行，身材苗条](https://medium.com/@kim.jangwook/svelte-tutorials-1-1f49699da2c0)
2.  [本帖]如何苗条地处理事件
3.  [如何将数据绑定到瘦组件中](https://medium.com/@kim.jangwook/how-binding-data-into-svelte-component-3909c9fb3bdb)
4.  [细长组件的生命周期](https://medium.com/javascript-in-plain-english/lifecycle-of-the-svelte-component-ef00c1969a4a)

## 默认事件处理程序

您可以使用`on:`指令监听元素上的任何事件。

## 内嵌事件处理程序

您可以省略处理程序的引号，但是为了突出语法，建议在处理程序的前面/后面添加引号。

> 在某些框架中，由于性能原因，内联事件处理程序是反模式的。但是在 Svelte 中，内联事件处理程序不是反模式的，因为它在构建时运行！

## 事件修改器

你可以使用这些修改器，也可以像`on:click|once|capture={...}`一样链接多个修改器。

*   `preventDefault`:在运行处理程序之前调用`event.preventDefault()`。
*   `stopPropagation`:通话`event.stopPropagation()`。
*   `passive`:提高触摸/滚轮事件的滚动性能。
*   `capture`:在捕获阶段而不是冒泡阶段触发处理程序。
*   `once`:处理器第一次运行后移除。
*   `self`:只有 event.target 是元素本身时才触发处理程序。

## 组件事件

使用事件调度程序，您可以从组件调度事件。

> 组件实例化时必须先调用`createEventDispatcher`。

## 事件转发

与 DOM 事件不同，组件事件不`bubble`。如果您想要监听某个深度嵌套组件上的事件，则必须将事件转发给该深度嵌套组件。

## 事件转发:速记

事件转发需要太长的源代码，所以 Svelte 支持速记。

如果使用不带值的`on:message`事件指令，则意味着转发所有消息事件。

## DOM 事件转发

比如降价？为什么不用 [Markdium](https://markdium.dev/) 试着在 Markdown 介质上写呢！

## 用简单英语写的 JavaScript 的注释

我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名在[**【submissions@javascriptinplainenglish.com】**](mailto:submissions@javascriptinplainenglish.com)给我们发电子邮件，我们会把你添加为作者。

我们还推出了三种新的出版物！请关注我们的新出版物: [**AI in Plain English**](https://medium.com/ai-in-plain-english) ，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**