# JavaScript 中的 DOM 事件及其动作

> 原文：<https://javascript.plainenglish.io/dom-events-and-their-actions-bfdaf4478086?source=collection_archive---------5----------------------->

![](img/8540002464f89393a69bfc3c9f9d449b.png)

事件和事件监听器是使我们的站点动态和交互的基础。作为一个用户，当导航到一个交互式网站时，你希望能够做某些事情，并让这些事情对你的行为作出反应。这些事情包括，点击，滚动，缩放，鼠标悬停，拖放，按键等等。这些被称为事件，浏览器有能力跟踪这些事件。

我们有能力编写代码来监听特定元素上的特定事件。我们通过在一个元素上放置一个事件监听器并给它一个回调来做到这一点。当该元素被事件触发时，回调将运行。下面是我们使用的方法。

# 。addEventListener()

。addEventListener()接受两个参数。首先是我们告诉它要监听的事件，其次是回调。

```
button.addEventListener('click', (event) => { event.target.style.backgroundColor = 'blue'; });// button will turn blue upon click
```

从上面的例子中可以看出，我们还可以使用事件对象已知的各种属性。要获得 DOM 中属性和事件监听器的详细列表，请访问 [MDN:事件属性](https://developer.mozilla.org/en-US/docs/Web/API/Event#Properties)和 [MDN:事件监听器](https://developer.mozilla.org/en-US/docs/Web/Events)。

![](img/2d23518c37a0d4a1d9f49d671ccbebf3.png)

# 。停止传播()

初级开发人员从事件侦听器开始时的一个朋友是。stopPropagation()方法。在我们深入探讨这个问题之前，让我们定义一下传播的含义。当子元素包含与父元素相同的事件侦听器时，就会发生传播，当子事件被触发时，子事件和父事件都会发生。想象一下，点击一个导航并编码链接，点击后改变颜色，然后让你的整个

变成同样的颜色。等等，我只是想让家变色。不是整个导航条！这就是传播。

人类的类比是流感病毒。所有人都携带这种病毒。当一个孩子的流感病毒被触发时，他/她就会生病，对着爸爸打喷嚏，然后爸爸就会生病。孩子不希望这种情况发生，但它发生了，因为他们都携带相同的事件监听器，或者在这种情况下，他们体内的病毒，所以爸爸的病毒也被激活了。

为了阻止这种情况发生，孩子服用药物来阻止病毒的传播。这种药叫做。停止传播()。