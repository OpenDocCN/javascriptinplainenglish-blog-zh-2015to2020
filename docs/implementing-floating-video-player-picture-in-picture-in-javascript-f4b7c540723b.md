# 用 JavaScript 实现浮动视频播放器(画中画)

> 原文：<https://javascript.plainenglish.io/implementing-floating-video-player-picture-in-picture-in-javascript-f4b7c540723b?source=collection_archive---------3----------------------->

## 了解如何使用画中画视频体验来实现

![](img/10be886491a931ef2f7c26a016812d2f.png)

Photo by [veeterzy](https://unsplash.com/@veeterzy?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/video?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 您将学习的主题

✅什么是画中画？

✅检查画中画是否支持？

✅实现画中画视频体验

画中画中的✅事件

✅获取画中画播放器的宽度和高度。

✅迷你项目。

# 🔷画中画

`**Picture-in-Picture (PiP)**`允许用户在浮动窗口(总是在其他窗口之上)观看视频，这样他们就可以在与其他网站或应用程序交互时关注他们正在观看的内容。

换句话说，即使我们切换了播放视频的标签，视频元素仍然是可见的。你可以把它拖到屏幕的任何地方。

# 🔷检查画中画是否支持？

我们可以使用`document.pictureInPictureEnabled`布尔值来确定`PiP`是否启用。

# 🔷实现画中画视频体验

假设我们有视频元素和一个按钮。当用户点击按钮时，我们需要启用浮动视频。

```
**<video id="videoElement" controls="true" src="video_url"> </video>****<button id="PiP"> Enable Floating Video </button>**
```

为了启用`**PiP**` **，**我们需要在 video element 上调用`requestPictureInPicture()`，它将返回一个`promise`。

一旦`promise`被解决，那么视频元素就会被尖叫到右角，我们可以把它移动到任何地方。

我们可以通过使用`document.pictureInPictureElement`来检查是否已经有任何`PiP`视频在播放，其中有`video`元素在`PiP`模式下播放。

上面的代码将创建一个基本的`PiP`视频播放器。

# 🔷画中画事件

关于`PiP`视频的状态有两个事件

`***enterpictureinpicture***` →启用`PiP`模式后触发。

***leavepictureinpicture***

当`PiP`模式退出时触发。这可能是由于

*   视频左画中画。
*   用户可能已经从不同页面播放了画中画视频。

```
video.addEventListener('**enterpictureinpicture**', (event)=> {
    toggleBtn.textContent = "Exit Pip Mode";
});

video.addEventListener('**leavepictureinpicture**', (event) => {
     toggleBtn.textContent = " Enter PiP Mode";
});
```

# 🔷获取画中画播放器的宽度和高度

一旦用户进入`PiP`模式，我们就可以得到`PiP`付款人的宽度和高度。

宽度和高度存储在`enterpictureinpicture`事件对象的`pictureInPictureWindow`属性中。

# 🔷迷你项目。

让我们用`PiP`实现一些有趣的🧐事物

1️⃣ **在画中画窗口显示用户的网络摄像头**

这里我们将使用`navigator`对象的`mediaDevices`属性中的`getUserMedia`方法访问用户网络摄像头。

```
const video = document.createElement('video');video.muted = true;**video.srcObject = await navigator.mediaDevices.getUserMedia({ video: true });**video.play()

video.requestPictureInPicture();
```

**2️⃣在画中画窗口显示用户显示屏**

这里我们将使用`navigator`对象的`mediaDevices`属性中的`getDisplay`方法来访问用户显示。

```
const video = document.createElement('video');video.muted = true;**video.srcObject = await navigator.mediaDevices.getDisplayMedia({ video: true });**video.play();

**video.requestPictureInPicture();**
```

请在这里捐款[。这对我意义重大。](https://paypal.me/jagathishSaravanan?locale.x=en_GB)

如果您发现任何错误，请在评论中报告。

跟随 [Javascript 吉普🚙💨](https://medium.com/u/f9ffc26e7e69?source=post_page-----f4b7c540723b--------------------------------)更多有趣的教程

**用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****

******参考:******

 ****[## 画中画样本

### 说明画中画使用的示例。

谷歌浏览器. github.io](https://googlechrome.github.io/samples/picture-in-picture/)**** ****[](https://www.buymeacoffee.com/Jagathish) [## Jagathish Saravanan

### 你好👋。我是 Jagathish。爱写关于 JavaScript 的文章。你的支持就像夏天吃冰淇淋一样。我…

www.buymeacoffee.com](https://www.buymeacoffee.com/Jagathish) 

> [参观 JavascriptJeep.com](http://javascriptjeep.com)****