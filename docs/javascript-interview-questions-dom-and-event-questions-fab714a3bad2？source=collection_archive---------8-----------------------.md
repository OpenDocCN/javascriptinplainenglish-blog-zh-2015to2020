# JavaScript 面试问题— DOM 和事件问题

> 原文：<https://javascript.plainenglish.io/javascript-interview-questions-dom-and-event-questions-fab714a3bad2?source=collection_archive---------8----------------------->

![](img/d16d7e9e58efe427c7d25d051456652c.png)

Photo by [Hello I'm Nik 🇬🇧](https://unsplash.com/@helloimnik?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了得到一份前端开发人员的工作，我们需要搞定编码面试。

在本文中，我们将研究一些关于 DOM 操作和处理事件的更难的问题。

# 什么是重画，什么时候会重画？

当我们改变一个元素的外观而不改变它的大小和形状时，就会发生重画。

它不会导致回流，因为它的尺寸和位置没有改变。

当元素更改背景色、更改文本颜色或隐藏可见性时，会发生重新绘制过程。

# 当 DOM 像`$(document).ready`一样准备好了，我们如何运行一些 JavaScript 呢？

我们可以将脚本放在 HTML body 元素中。当浏览器在那里运行脚本标记时，DOM 已经准备好了。

此外，我们可以将代码放在`DOMContentLoaded`事件处理程序中。只有当 DOM 完全加载后，里面的代码才会运行。

例如，我们可以这样写:

```
document.addEventListener('DOMContentLoaded', () => {
  console.log('DOM loaded');
});
```

我们还可以通过附加一个监听器来观察`readystatechange`事件。

当`readyState`是`'complete'`时，我们就知道 DOM 已经加载了。

例如，我们可以编写以下代码来实现这一点:

```
document.onreadystatechange = () => {
  if (document.readyState == "complete") {
    console.log('DOM loaded');
  }
}
```

# 什么是事件冒泡？事件如何流动？

事件冒泡意味着事件从原始元素传播到其父元素、祖父元素，并一直传播到`window`对象。

除了原始元素之外，浏览器还将运行附加到原始元素的所有父元素的所有事件处理程序。

例如，如果我们有以下 HTML:

```
<div>
  <p>
    <button>Click</button>
  </p>
</div>
```

然后，当我们将事件监听器附加到所有元素和`document`和`window`时，如下所示:

```
const div = document.querySelector('div');
const p = document.querySelector('p');
const button = document.querySelector('button');button.onclick = () => {
  alert('button clicked');
}p.onclick = () => {
  alert('p clicked');
}div.onclick = () => {
  alert('div clicked');
}document.onclick = () => {
  alert('document clicked');
}window.onclick = () => {
  alert('window clicked');
}
```

然后单击“click”按钮，我们将看到所有的警报都按照代码中列出的顺序排列。

因此，我们得到按顺序显示的“按钮被点击”、“p 被点击”、“div 被点击”、“文档被点击”和“窗口被点击”警报。

# 我们如何用一个点击处理程序销毁多个列表项？

我们可以使用事件委托来做到这一点。

它通过监听列表的父元素的点击来工作。然后我们可以检查在 click 处理程序中点击了哪个子元素，然后从 DOM 中移除该元素。

例如，如果我们有以下 HTML:

```
<ul>
    <li>first</li>
    <li>second</li>
    <li>third</li>
    <li>forth</li>
    <li>Fifth</li>
</ul>
```

然后，我们可以编写以下 JavaScript 代码来删除我们单击的 li 元素，如下所示:

```
document.querySelector('ul').addEventListener('click', (e) => {
  const elm = e.target.parentNode;
  elm.removeChild(e.target);
  e.preventDefault();
});
```

在上面的代码中，我们获得了我们单击以获得 ul 的元素的`parentNode`属性。

然后我们可以调用它的`removeChild`来删除我们点击的 li，因为`e.target`是我们点击的元素，也就是 li。

最后，我们调用`preventDefault`停止事件传播。

![](img/38d2e33f153ec7fe7a64a59a735b2243.png)

Photo by [Sander Dalhuisen](https://unsplash.com/@sanderdalhuisen?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 创建一个按钮，点击它就会被破坏，但是两个新的按钮会被替换。

我们可以通过使用上面的逻辑来移除我们点击的按钮。

然后我们添加使用`createElement`和`appendChild`来创建更多的按钮，并将它们添加到列表中。

例如，给定以下 HTML:

```
<div>
  <button>button</button>
</div>
```

我们编写了下面的 JavaScript 代码来添加两个按钮，然后通过在 div 上附加一个 click 侦听器并操纵按钮来删除被点击的原始按钮:

```
document.querySelector('div').addEventListener('click', (e) => {
  if (e.target.tagName === 'DIV') {
    return;
  }
  const elm = e.target.parentNode;
  e.preventDefault(); const btn = document.createElement('button');
  btn.innerHTML = 'button'; const btn2 = document.createElement('button');
  btn2.innerHTML = 'button'; elm.appendChild(btn);
  elm.appendChild(btn2);
  elm.removeChild(e.target);
});
```

在上面的代码中，我们检查我们是否真的点击了一个按钮。

如果我们这样做了，那么我们继续创建两个按钮。然后我们调用被点击按钮的`parentNode`上的`appendChild`来连接两个按钮，这个按钮就是 div。

然后我们调用`e.target`上的`removeChild`来移除按钮，因为我们检查过它不是 div，所以它应该是按钮。

# 结论

我们可以观察 DOM 是否已经为`readystatechange`或`DOMContentLoaded`事件做好准备。

当元素的事件在 DOM 树中传播时，就会发生事件冒泡。

`appendChild`、`removeChild`用于元素的添加和移除。我们可以通过事件委托来处理多个子元素的事件。

当我们改变元素的外观而不改变其几何形状时，就会发生重画。

## **来自 JavaScript 的普通英语注释**

我们推出了三种新的出版物！通过以下方式表达对我们新出版物的热爱: [**通俗易懂的 AI**](https://medium.com/ai-in-plain-english)、 [**通俗易懂的 UX**、](https://medium.com/ux-in-plain-english)、[、**通俗易懂的 Python**、](https://medium.com/python-in-plain-english)、**、**——谢谢您，继续学习！

我们也一直对帮助推广高质量内容感兴趣。如果您有一篇文章想提交给我们的任何出版物，请通过电子邮件发送至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**并使用您的 Medium 用户名，我们会将您添加为作者。另外，请告诉我们您想添加到哪个出版物中。**