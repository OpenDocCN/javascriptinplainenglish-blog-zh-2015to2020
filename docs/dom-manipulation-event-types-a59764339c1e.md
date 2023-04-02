# DOM 操作—事件类型

> 原文：<https://javascript.plainenglish.io/dom-manipulation-event-types-a59764339c1e?source=collection_archive---------5----------------------->

![](img/5e6a198925193dda448e36f23175b831.png)

Photo by [Ochir-Erdene Oyunmedeg](https://unsplash.com/@chiklad?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是世界上最流行的编程语言之一。为了有效地使用它，我们必须了解它的基本知识。

在本文中，我们将研究一些可以监听的 DOM 事件类型。

# DOM 事件类型

我们可以听很多种类的事件。

以下是用户事件:

*   `load` —装载东西时触发
*   `unload` —当用户代理删除资源时触发
*   `abort` —当资源在完成加载之前停止加载时触发
*   `error` —加载失败时触发
*   `resize` —调整文档视图大小时触发
*   `scroll`—当用户滚动时触发
*   `contextmenu` —通过右键单击 and 元素触发

焦点事件有:

*   `blur` —当元素因触发 most 或按 tab 键而失去焦点时触发
*   `focus` —当元素获得焦点时激发
*   `focusin` —当事件目标即将获得焦点但焦点尚未转移时触发
*   `focusout` —当事件目标即将失去焦点时触发

表单事件包括:

*   `change` —当控件失去输入焦点时触发
*   `reset` —表单重置时触发
*   `submit` —提交表单时触发
*   `select` —当用户选择字段中的一些文本时触发

鼠标事件包括:

*   `click` —当鼠标指针在元素上单击时激发
*   `dblclick` —当鼠标指针在元素上出现两次时触发
*   `mousedown` —当鼠标指针按在元素上时触发
*   `mouseenter` —当鼠标指针移动到元素上时触发
*   `mouseleave` —当鼠标指针离开一个元素及其所有子元素的边界时触发。
*   `mousemove` —当鼠标指针在元素上移动时触发
*   `mouseup` —当鼠标指针在元素上释放时触发
*   `mouseover` —当鼠标指针在元素上移动时触发

车轮事件包括:

*   `wheel` —当鼠标滚轮旋转时触发

关键词事件包括:

*   `keydown` —最初按下某个键时触发
*   `keypress` —最初按下某个键时触发
*   `keyup`-释放钥匙时触发

触摸事件包括:

*   `touchstart`-触发事件以指示用户何时在触摸表面上放置触摸点
*   `touchend`-当用户从触摸表面移除触摸点时，会激发以指示
*   `touchmove`-当用户沿着触摸表面移动触摸点时触发
*   `toucheneter`-当接触点移动到由 DOM 元素定义的交互区域时触发
*   `touchleave`-当接触点移动到由 DOM 元素定义的区域之外时，触发以指示
*   `touchcancel`-当接触点中断时触发

窗口/正文事件包括:

*   `afterprint`-打印东西时发出火焰
*   `beforeprint` —在某物打印之前激发
*   `beforeunload`-在卸载文档之前激发
*   `hashchange`-当 URL 后跟`#`的部分改变时触发
*   `message` —当用户向工作人员发送消息时激发
*   `offline`-浏览器离线时触发
*   `online`-浏览器在线时触发
*   `pagehide`-从会话历史条目遍历时触发
*   `pageshow`-从会话历史记录条目行进时触发

文档事件包括:

*   `readystatechange`-就绪状态改变时触发
*   `DOMContentLoaded` —当网页被加载时，但在所有资源被下载之前，首先

拖动事件包括:

*   `drag`-拖动源对象时激发
*   `dragstart`-当源对象开始被拖动时触发
*   `dragend`-当用户在拖动的对象上释放鼠标时，在源对象上激发
*   `dragenter`-当被拖动的元素进入放置目标时触发
*   `dragleave`-当物体移动到掉落目标时触发
*   `dragover`-当对象被拖动到放置目标上时触发
*   `drop`-物体掉落时触发

![](img/1bfb2a83e9cfefdbcbddbac96d243d01.png)

Photo by [Jimmy Chang](https://unsplash.com/@photohunter?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 事件流

事件通过 DOM 传播。

它们从原始元素一直到根元素对象。这被称为鼓泡阶段

因此。当我们在祖先对象上注册侦听器时，它们也会运行。

还有捕获阶段，与鼓泡阶段相反。

# 结论

有许多我们可以听的事件。

它们传播到一个元素的祖先并返回。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**