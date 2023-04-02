# JavaScript 事件处理程序— onfocus、oncancel 和 oncanplay

> 原文：<https://javascript.plainenglish.io/javascript-events-handlers-onfocus-oncancel-and-oncanplay-e62dfca93dca?source=collection_archive---------2----------------------->

![](img/b8b112e5fee38596a1fe45fcfb7135d6.png)

Photo by [Abigail Keenan](https://unsplash.com/@akeenster?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在 JavaScript 中，事件是应用程序中发生的动作。它们是由各种事情触发的，比如输入、提交表单、调整大小等元素变化，或者应用程序运行时发生的错误等。我们可以分配事件处理程序来处理这些事件。发生在 DOM 元素上的事件可以通过为相应事件的 DOM 对象的属性分配一个事件处理程序来处理。在本文中，我们将看看`onfocus`、`oncancel`和`oncanplay`事件处理程序。

# 专注

`document`对象的`onfocus`属性让我们为`focus`事件设置一个事件处理程序，这与`blur`事件相反。当用户在 HTML 元素上设置焦点时，触发`focus`事件。如果我们想让焦点事件触发非输入元素，我们必须给它加上`tabindex`属性。那个属性添加了什么，我们可以用计算机的 Tab 键来关注它。例如，我们可以通过编写以下代码将`onfocus`事件处理程序附加到`document`对象:

```
document.onfocus = () => {
  console.log('focus');
}
```

我们还可以通过将`event`参数添加到事件处理函数中来获取触发焦点事件的`Event`对象，如以下代码所示:

```
document.onfocus = (event) => {
  console.log(event);
}
```

然后，当我们在页面内外点击时，我们会从上面的代码中得到类似下面这样的输出:

```
bubbles: false
​cancelBubble: false
​cancelable: false
​composed: true
​currentTarget: null
​defaultPrevented: false
​detail: 0
​eventPhase: 0
​explicitOriginalTarget: <html>
​isTrusted: true
​layerX: 0
​layerY: 0
​originalTarget: HTMLDocument [https://fiddle.jshell.net/_display/](https://fiddle.jshell.net/_display/)
​rangeOffset: 0
​rangeParent: null
​relatedTarget: null
​returnValue: true
​srcElement: HTMLDocument [https://fiddle.jshell.net/_display/](https://fiddle.jshell.net/_display/)
​target: HTMLDocument [https://fiddle.jshell.net/_display/](https://fiddle.jshell.net/_display/)
​timeStamp: 1463
​type: "focus"
​view: Window [https://fiddle.jshell.net/_display/](https://fiddle.jshell.net/_display/)
​which: 0
```

上面的输出是`Event`对象的属性和相应的值。要查看关于`Event`对象的更多细节，我们可以看看以前的文章。

# 癌症

为了处理`dialog`元素关闭时的情况，我们可以使用`oncancel`事件处理程序，因为`cancel`事件是在它关闭时触发的。用`oncancel`事件处理程序处理该事件可以防止它冒泡，所以父处理程序不会得到该事件的通知。一次只能给一个对象分配一个`oncancel`处理器。然而，如果我们使用`addEventListener`将事件处理函数附加到我们的元素上，那么我们就可以避开这个限制。例如，我们可以在下面的代码中使用它:

```
const dialog = document.getElementById('dialog');
const openButton = document.getElementById('open-button');
openButton.onclick = () => {
  dialog.showModal();
};dialog.oncancel = (event) => {
  console.log('cancel');
  console.log(event);
}
```

然后我们必须将`dialog`元素添加到我们的 HTML 代码中:

```
<button id='open-button'>
  Open Dialog
</button>
<dialog id="dialog">
  <form method="dialog">
    <p>
      Dialog
    </p>
    <menu>
      <button id="cancel-button" value="cancel">Cancel</button>
      <button id="confirmBtn" value="default">Confirm</button>
    </menu>
  </form>
</dialog>
```

在上面的代码中，我们向 HTML 添加了一个`dialog`元素，并使用`getElementById`方法获取 HTML `dialog`元素的 DOM 元素，这使我们有了以下方法:

*   `close()` —关闭`dialog`元素的对话框实例方法。一个可选的字符串可以作为参数传入，它更新`dialog`的`returnValue`，这对于指示用户使用哪个按钮关闭它很有用。
*   `show()` —一个无模式显示对话框的对话框实例方法，这意味着我们仍然允许来自外部的交互。这不需要争论。
*   `showModal()` —一个对话框实例方法，将对话框作为模态显示在任何其他对象之上。它与::background 伪元素一起显示在顶层。与对话框外的元素的交互被阻止，并且不能与对话框外的内容进行交互。

`dialog` DOM 元素还具有以下值属性:

*   `open` —反映`open` HTML 属性的布尔属性，该属性指示对话框是否为交互而打开。
*   `returnValue` —设置并返回对话框返回值的字符串属性。我们可以直接给它赋值，也可以向`close`方法传递一个参数来设置这个属性。

我们不需要调用`close()`方法来关闭对话框。有个按钮就够了。此外，我们不必单击按钮来关闭对话框，我们也可以按键盘上的 Esc 键来关闭对话框。

注意在 Firefox 上默认情况下`dialog`元素是不启用的。要在 Firefox 中使用它，我们必须在`about:config`页面中将`dom.dialog_element.enabled`设置为`true`。Chrome 默认启用了这个功能。然后，如果我们单击刚才制作的“打开对话框”按钮，我们将看到一个本机浏览器对话框。然后，如果我们按下 Esc 键关闭对话框，那么`cancel`事件将被触发，我们分配给`dialog.oncancel`的事件处理函数将运行。事件处理函数的`event`参数将获得一个`Event`对象，其中包含关于`cancel`事件源的信息，也就是我们的`dialog`元素。因此，在我们的`console.log`输出中，我们将得到类似如下的内容:

```
bubbles: false
cancelBubble: false
cancelable: true
composed: false
currentTarget: null
defaultPrevented: false
eventPhase: 0
isTrusted: true
path: (5) [dialog#dialog, body, html, document, Window]
returnValue: true
srcElement: dialog#dialog
target: dialog#dialog
timeStamp: 1102.240000385791
type: "cancel"
```

要查看关于`Event`对象属性的更多细节，我们可以查看本系列的前几部分。

![](img/c2cb15cc5d4d56e331557b1293f53b6a.png)

Photo by [William Bayreuther](https://unsplash.com/@wbayreuther?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 在线播放

当我们想要处理`canplay`事件时，我们可以给`oncanplay`属性分配一个事件处理函数。当用户代理可以播放媒体，但估计没有加载足够的数据来播放媒体直到其结束而不必停止以进一步缓冲内容时，触发`canplay`事件。例如，我们可以将它用于视频，以查看是否下载了足够的视频部分来播放，直到结束，我们可以首先将视频元素添加到 HTML 代码中:

```
<video width="320" height="240" controls id='video'>
  <source src="[https://sample-videos.com/video123/mp4/240/big_buck_bunny_240p_30mb.mp4](https://sample-videos.com/video123/mp4/240/big_buck_bunny_240p_30mb.mp4)" type="video/mp4">
</video>
```

然后在我们的 JavaScript 代码中，我们可以添加以下代码来检查视频是否下载了足够的内容来播放:

```
const video = document.getElementById('video');
video.oncanplay = (event) => {
  console.log(event);
}
```

我们还可以通过在视频 DOM 节点上使用`addEventListener`方法来附加`canplay`事件监听器，如以下代码所示:

```
const video = document.getElementById('video');
video.addEventListener('canplay', (event) => {
  console.log(event);
});
```

无论哪种方式，我们都可以通过使用媒体元素(包括视频和音频元素)的`readyState`属性来检查是否已经下载了足够多的媒体部分，以便完成它。`readyState`可以有以下可能值之一:

*   常量`HAVE_NOTHING` 或数字`0` —没有关于媒体资源的信息
*   常量`HAVE_METADATA`或数字`1` —媒体资源的足够部分已经被下载，元数据属性被初始化。在本州或更远的地方寻找不会引发异常
*   常量`HAVE_CURRENT_DATA`或数字`2` —当前播放位置有足够的数据，但不足以实际播放一帧以上
*   常数`HAVE_FUTURE_DATA`或数字`3`——下载了当前播放位置的足够数据，以及至少播放到未来一点的足够数据，这意味着至少 2 帧视频。
*   常数`HAVE_ENOUGH_DATA`或数字`4`——有足够的数据可用，下载速率足够高，媒体可以不间断地播放到结束。

我们可以通过编写以下代码来获取视频元素的`readyState`属性:

```
const video = document.getElementById('video');
video.oncanplay = (event) => {
  console.log(event.target.readyState);
}
```

如果我们的视频可以一直播放，我们应该有 4 个记录在`console.log`输出中。

`document`对象的`onfocus`属性让我们为`focus`事件设置一个事件处理程序。我们可以用它来检查我们的元素是否是焦点。为了关注非输入元素，我们可以给它添加一个`tabindex`属性。为了处理`dialog`元素关闭时的情况，我们可以使用`oncancel`事件处理程序。当按下键盘上的 Esc 键时，触发相应的事件，即`cancel`事件。当我们想要处理`canplay`事件时，我们可以为`oncanplay`属性分配一个事件处理函数，该事件是当用户代理可以播放媒体时触发的，但估计没有足够的数据加载来播放媒体直到其结束，而不必停止以获得更多的内容缓冲。我们可以通过检查 media 元素中的`readyState`来检查媒体是否下载了足够的数据来播放。