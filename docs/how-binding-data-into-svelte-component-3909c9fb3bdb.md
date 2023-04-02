# 绑定数据如何以苗条的方式工作

> 原文：<https://javascript.plainenglish.io/how-binding-data-into-svelte-component-3909c9fb3bdb?source=collection_archive---------11----------------------->

## 因为将数据绑定到组件中很重要！

![](img/3767883e374b49d0723a84856425a837.png)

Logo of the Svelte

这篇文章是苗条教程系列之一。

1.  [苗条地开始跑步](https://medium.com/@kim.jangwook/svelte-tutorials-1-1f49699da2c0)
2.  [如何苗条地处理事件](https://medium.com/javascript-in-plain-english/svelte-tutorials-2-e965737edf63)
3.  [本帖]如何将数据绑定到瘦组件中
4.  [细长组件的生命周期](https://medium.com/javascript-in-plain-english/lifecycle-of-the-svelte-component-ef00c1969a4a)

## TL；速度三角形定位法(dead reckoning)

## 文本输入

## 数字输入

## 复选框输入

## 分组输入(复选框、单选)

如果您有多个与相同值相关的输入，您应该使用`bind:group`。

**收音机**

**复选框**

## 文本区域输入

## 选择绑定

svelet 允许绑定对象作为一个`<option>`值，svelet 建议绑定对象作为一个值。

## 选择多个

## 内容可编辑绑定

具有`contenteditable="true"`属性的元素支持`textContent`和`innerHTML`绑定。

**文本内容**

**innerHTML**

## 媒体元素

`<audio>`和`<video>`元素有几个可以绑定的属性。

> 这个示例代码有点长，但是如果您仔细阅读，您会发现它并不难！

> 为什么这个例子不使用`timeupdate`事件来跟踪`currentTime`？
> 
> 通常在网络上，你可以通过监听`timeupdate`事件来跟踪`currentTime`。但是这些事件触发的频率太低，导致 UI 不稳定。Svelte 做得更好——它使用`requestAnimationFrame`检查`currentTime`。

您可以将这些属性绑定到`<audio>`和`<video>`元素。

*   `duration`(只读)—视频的总时长，以秒为单位
*   `buffered`(只读)—一个{start，end}对象的数组
*   `seekable`(只读)—同上
*   `played`(只读)—同上
*   `seeking`(只读)—布尔型
*   `ended`(只读)—布尔型
*   `currentTime` —视频中的当前点，以秒为单位
*   `playbackRate` —播放视频的速度，其中 1 表示“正常”
*   这个应该是不言自明的
*   `volume` —介于 0 和 1 之间的值

而`<video>`元素有`videoWidth`(只读)和`videoHeight`(只读)属性。

## 规模

所有块级元素都有`clientWidth`、`clientHeight`、`offsetWidth`、`offsetHeight`。和...都是`readonly`。

## 这

您可以使用`bind:this`属性来获取 HTML 元素的引用。

## 组件绑定

您可以使用`bind:`绑定暴露组件的任何属性。

在 Markdown 中写入介质？试试[马克迪](https://markdium.dev/)！

## **用简单英语写的 JavaScript 笔记**

我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到[**submissions@javascriptinplainenglish.com**](mailto:submissions@javascriptinplainenglish.com)给我们，我们会把你添加为作者。

我们还推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**