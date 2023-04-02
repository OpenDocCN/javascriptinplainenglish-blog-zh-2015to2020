# JavaScript 事件处理程序—鼠标滚轮和媒体事件

> 原文：<https://javascript.plainenglish.io/javascript-events-handlers-mouse-wheel-and-media-events-b0929f1e8aa9?source=collection_archive---------3----------------------->

![](img/0bf24ea2eeaa31c08f1ba336df8815e3.png)

Photo by [Marieke Tacken](https://unsplash.com/@maretak?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在 JavaScript 中，事件是应用程序中发生的动作。它们是由各种事情触发的，比如输入、提交表单、调整大小等元素变化，或者应用程序运行时发生的错误等。我们可以分配一个事件处理器来处理这些事件。发生在 DOM 元素上的事件可以通过为相应事件的 DOM 对象的属性分配一个事件处理程序来处理。在本文中，我们将看看如何使用`onwheel`处理滚轮事件，`onpause`和`onplay`处理媒体元素的暂停和播放事件。

# 车轮上

DOM 对象的`onwheel`属性允许我们为它分配一个事件处理函数来处理`wheel`事件。旋转鼠标滚轮时会触发该事件。这与`onscroll`不同，因为滚动并不总是使用鼠标滚轮。

例如，当我们旋转鼠标滚轮时，我们可以使用`onwheel`事件处理函数来改变盒子的大小。为此，首先，我们在 HTML 代码中添加一个`div`来生成一个框，我们可以改变它的大小:

```
<div>Make me bigger or smaller by rotating the mouse wheel.</div>
```

然后，我们通过改变`div`元素的大小、位置和颜色来设置它的样式，如下面的代码所示:

```
div {
  width: 100px;
  height: 100px;
  background: lightblue;
  padding: 5px;
  transition: transform .3s;
  margin: 0 auto;
  margin-top: 100px;
}
```

最后，我们添加 JavaScript 代码，该代码利用`onwheel`事件处理函数根据鼠标滚轮的滚动方向缩放`div`元素:

```
let scale = 1;const zoom = (event) => {
  event.preventDefault();
  if (event.deltaY < 0) {
    scale *= event.deltaY * -1.1;
  } else {
    scale /= event.deltaY * 1.1;
  }
  scale = Math.min(Math.max(.1, scale), 3);
  el.style.transform = `scale(${scale})`;
}const el = document.querySelector('div');
document.onwheel = zoom;
```

在上面的代码中，如果我们逆时针移动鼠标滚轮，那么`div`会变大。否则，它会变小。它通过从`event`对象获取`deltaY`属性来实现这一点。当`deltaY`为负时，这意味着鼠标滚轮正在逆时针滚动，因此我们将其乘以`-1.1`以使`scale`变量为正，并将现有的`scale`值乘以该数字以增加`div`的大小。否则，我们取而代之进行除法运算，从而使`div`收缩。

在`if`语句之后，我们将`scale`变量限制在`.1`和`3`之间，这样它就不会变得无限大或无限小。最后，我们通过运行`el.style.transform = `scale(${scale})`;`将`scale`值赋给`transform`属性。

# 暂停

当媒体回放暂停时，触发`pause`事件。为了处理这个事件，我们可以为 DOM 元素中的`onpause`属性分配一个事件处理函数。

当处理暂停活动的请求并且活动进入暂停状态时，发送`pause`事件。当我们通过手动暂停媒体播放或调用`pause()`方法来暂停音频或视频时，这是最常见的。一旦`pause()`方法返回，并且在媒体元素的`paused`属性被更改为`true`之后，该事件被触发。

例如，我们可以通过编写一些代码来记录视频是否暂停。首先，我们为`video`元素添加 HTML 代码，如下所示:

```
<video controls src='[https://sample-videos.com/video123/mp4/240/big_buck_bunny_240p_30mb.mp4'](https://sample-videos.com/video123/mp4/240/big_buck_bunny_240p_30mb.mp4')></video>
```

然后在 JavaScript 代码中，我们可以将我们的事件处理函数分配给`video` DOM 对象的`onpause`属性:

```
const video = document.querySelector('video');video.onpause = (event) => {
  console.log('video paused');
};
```

之后，当我们暂停视频时，我们应该会看到“视频暂停”消息。

如果我们在`video` DOM 对象上调用`pause`方法，也会显示“视频暂停”消息。例如，如果我们编写以下代码，而不是上面的代码:

```
const video = document.querySelector('video');
video.play();
setTimeout(() => {
  video.pause();
}, 2000)video.onpause = (event) => {
  console.log('video paused');
};
```

然后视频将播放 2 秒钟，在`video` DOM 对象上将调用`pause`，这将触发`pause`事件。然后我们会看到记录的“视频暂停”消息。

![](img/3dcad0166ad34072d1fa512c3cb220ac.png)

Photo by [Veronica García](https://unsplash.com/@kurita?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# onplay

像音频或视频这样的 DOM 媒体元素的`onplay`属性允许我们为这些对象分配一个事件处理函数。它处理当媒体元素的`paused`属性从`true`变为`false`时触发的`play`事件，这可能是调用媒体元素上的`play`方法或设置元素的`autoplay`属性的结果。

例如，我们可以创建一个自动播放视频元素，然后在它被触发时记录`play`事件。首先，我们通过添加一个 videon 元素来制作 HTML 代码:

```
<video controls autoplay src='[https://sample-videos.com/video123/mp4/240/big_buck_bunny_240p_30mb.mp4'](https://sample-videos.com/video123/mp4/240/big_buck_bunny_240p_30mb.mp4')></video>
```

然后在 JavaScript 代码中，我们为`play`事件定义了自己的事件处理函数，并将其设置为视频元素 DOM 对象的`onplay`属性:

```
const video = document.querySelector('video');video.onplay = (event) => {
  console.log('video plays');
};
```

当页面加载时视频开始播放，我们应该会看到记录在`console.log`语句中的“视频播放”消息。

当我们在视频元素上调用`play`方法时，它也应该记录。如果我们有:

```
const video = document.querySelector('video');video.play();video.onplay = (event) => {
  console.log('video plays');
};
```

我们应该在控制台中记录相同的消息。如果我们手动播放视频，情况也是如此。

DOM 对象的`onwheel`属性允许我们给它分配一个事件处理函数来处理`wheel`事件。旋转鼠标滚轮时会触发该事件。这与`onscroll`不同，因为滚动并不总是使用鼠标滚轮。我们可以让`deltaY`得到鼠标滚轮旋转的幅度和方向。

当媒体回放暂停时，触发`pause`事件。为了处理这个事件，我们可以为 DOM 元素中的`onpause`属性分配一个事件处理函数。

当处理暂停活动的请求并且活动进入暂停状态时，发送`pause`事件。当我们通过手动暂停媒体播放或调用`pause()`方法来暂停音频或视频时，这是最常见的。一旦`pause()`方法返回，并且在媒体元素的`paused`属性被更改为`true`之后，该事件被触发。

像音频或视频这样的 DOM 媒体元素的`onplay`属性允许我们为这些对象分配一个事件处理函数。它处理当媒体元素的`paused`属性从`true`变为`false`时触发的`play`事件，这可能是调用媒体元素上的`play`方法或设置元素的`autoplay`属性的结果。