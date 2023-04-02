# JavaScript 事件处理程序— onfocus、oncancel 等等

> 原文：<https://javascript.plainenglish.io/javascript-events-handlers-onfocus-oncancel-and-more-ce0d6f76c5c8?source=collection_archive---------4----------------------->

![](img/c197e4908fb8e0e9fa4ed5a7c447c5c7.png)

Photo by [Priscilla Du Preez](https://unsplash.com/@priscilladupreez?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在 JavaScript 中，事件是应用程序中发生的动作。它们是由各种事情触发的，比如输入、提交表单、调整大小等元素变化，或者应用程序运行时发生的错误等。我们可以分配事件处理程序来处理这些事件。发生在 DOM 元素上的事件可以通过为相应事件的 DOM 对象的属性分配一个事件处理程序来处理。在本文中，我们将看看`oncanplaythrough`、`onchange`、`onclick`和`onclose`事件处理程序。

# 在线播放

`oncanplaythrough`属性让我们为它分配自己的事件处理函数来处理`canplaythrough`事件。当用户代理可以播放媒体，并且估计加载了足够的数据来播放媒体直到结束，而不必停止并缓冲更多数据时，就会触发此事件。例如，我们可以通过添加以下 HTML 代码来监听视频的`canplaythrough`事件:

```
<video width="320" height="240" controls id='video'>
  <source src="[https://sample-videos.com/video123/mp4/240/big_buck_bunny_240p_30mb.mp4](https://sample-videos.com/video123/mp4/240/big_buck_bunny_240p_30mb.mp4)" type="video/mp4">
</video>
```

然后在我们的 JavaScript 代码中，我们可以添加:

```
const video = document.getElementById('video');
video.oncanplaythrough = (event) => {
  console.log(event);
}
```

我们还可以通过在视频 DOM 节点上使用`addEventListener`方法来附加`canplaythrough`事件监听器，如以下代码所示:

```
const video = document.getElementById('video');
video.addEventListener('canplaythrough', (event) => {
  console.log(event);
});
```

无论哪种方式，我们都可以通过使用媒体元素(包括视频和音频元素)的`readyState`属性来检查是否已经下载了足够多的媒体部分，以便完成下载。`readyState`可以有以下可能值之一:

*   常量`HAVE_NOTHING` 或数字`0` —没有关于媒体资源的信息
*   常数`HAVE_METADATA`或数字`1` —媒体资源的足够部分已经被下载，元数据属性被初始化。在本州或更远的地方寻找不会引发异常
*   常量`HAVE_CURRENT_DATA`或数字`2` —当前播放位置有足够的数据，但不足以实际播放一帧以上
*   常量`HAVE_FUTURE_DATA`或数字`3` —下载了当前播放位置的足够数据，以及至少播放到未来一点的足够数据，这意味着至少 2 帧视频。
*   常数`HAVE_ENOUGH_DATA`或数字`4`——有足够的数据可用，下载速率足够高，媒体可以不间断地播放到结束。

我们可以通过编写以下代码来获取视频元素的`readyState`属性:

```
const video = document.getElementById('video');
video.oncanplaythrough = (event) => {
  console.log(event.target.readyState);
}
```

如果我们的视频可以一直播放，我们应该有 4 个记录在`console.log`输出中。

# onchange

`onchange`属性让我们给它分配一个事件处理程序来处理`change`事件。当用户提交表单控件的值更改时，触发`change`事件。这可以通过在控件外部单击或使用 Tab 键切换不同的控件来完成。例如，在用户向`input`输入一个值之后，我们可以使用它来获取`input`元素的输入值，然后将焦点移到输入之外。首先，我们可以添加以下 HTML 代码:

```
<input type="text" size="50" id='input' placeholder='Input'>
```

然后在 JavaScript 代码中，我们添加:

```
const input = document.getElementById('input');
input.onchange = (event) => {
  console.log(event.target.value);
}
```

input 元素的`value`属性将包含我们在添加到 HTML 代码的 input 元素中输入的文本值。当用户在不同的输入和表单的不同部分之间移动时，这对于获取输入非常有帮助。

![](img/cf37e46a0e2c18755790ffa715840b9d.png)

Photo by [Harpal Singh](https://unsplash.com/@aquatium?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# onclick

我们可以给元素的`onclick`属性分配一个事件处理程序，当用户点击一个元素时做一些事情。`click`事件由`onclick`事件处理程序处理，当用户点击元素时触发。它按顺序在`mousedown`和`mouseup`事件之后触发。非鼠标或触摸屏设备不会触发`click`事件。为了适应这些用户，我们可以处理`keydown`事件。例如，我们可以使用它来获取浏览器选项卡的位置，该选项卡是用以下 HTML 代码单击的:

```
<p>Click anywhere in this example.</p>
<div id='message'></div>
```

我们将在`div`元素中添加跟踪鼠标位置的消息。

然后在我们的 JavaScript 代码中，我们可以写:

```
const message = document.getElementById('message');
document.onclick = (event) => {
  message.innerHTML = `You click coordinate is x = ${event.clientX}, y = ${event.clientY}`;
}
```

在上面的代码中，我们获得了当鼠标点击页面时鼠标所在页面的位置。我们可以使用`Event`对象的`clientX`和`clientY`分别获取每次鼠标点击的 x 和 y 坐标。然后当我们运行代码时，我们可以看到鼠标被点击的地方的坐标。

# onclose

为了处理`dialog`元素关闭时的情况，我们可以使用`onclose`事件处理程序，因为`close`事件是在它关闭时触发的。用`onclose`事件处理程序处理该事件可以防止它冒泡，所以父处理程序不会得到该事件的通知。一次只能给一个对象分配一个`onclose`处理器。然而，如果我们使用`addEventListener`将事件处理函数附加到我们的元素上，那么我们就可以避开这个限制。`close`和`cancel`事件的区别在于，无论对话框以何种方式关闭，都会触发关闭事件。例如，我们可以在下面的代码中使用它:

```
const dialog = document.getElementById('dialog');
const openButton = document.getElementById('open-button');
openButton.onclick = () => {
  dialog.showModal();
};dialog.onclose = (event) => {
  console.log('dialog closed');
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

在上面的代码中，我们在 HTML 中添加了一个`dialog`元素，并使用`getElementById`方法获取 HTML `dialog`元素的 DOM 元素，这样我们就有了以下方法:

*   `close()` —关闭`dialog`元素的对话框实例方法。一个可选的字符串可以作为参数传入，它更新`dialog`的`returnValue`，这对于指示用户使用哪个按钮关闭它很有用。
*   `show()` —无模式显示对话框的对话框实例方法，这意味着我们仍然允许来自外部的交互。这不需要争论。
*   `showModal()` —一个对话框实例方法，用于将对话框作为模态显示在任何其他对象之上。它与::background 伪元素一起显示在顶层。与对话框外的元素的交互被阻止，并且不能与对话框外的内容进行交互。

`dialog` DOM 元素还具有以下值属性:

*   `open` —一个反映`open` HTML 属性的布尔属性，它指示一个对话框是否为交互而打开。
*   `returnValue` —设置并返回对话框返回值的字符串属性。我们可以直接给它赋值，也可以向`close`方法传递一个参数来设置这个属性。

我们不需要调用`close()`方法来关闭对话框。有个按钮就够了。此外，我们不必单击按钮来关闭对话框，我们也可以按键盘上的 Esc 键来关闭对话框。

请注意，Firefox 默认情况下不启用`dialog`元素。要在 Firefox 中使用它，我们必须在`about:config`页面中将`dom.dialog_element.enabled`设置为`true`。Chrome 默认启用了这个功能。然后，如果我们单击刚才制作的“打开对话框”按钮，我们将看到一个本机浏览器对话框。然后，如果我们按 Esc 键关闭对话框，那么`cancel`事件将被触发，我们分配给`dialog.oncancel`的事件处理函数将运行。事件处理函数的`event`参数将获得一个`Event`对象，其中包含关于`cancel`事件源的信息，也就是我们的`dialog`元素。因此，在我们的`console.log`输出中，我们将得到类似如下的内容:

```
bubbles: false
cancelBubble: false
cancelable: false
composed: false
currentTarget: null
defaultPrevented: false
eventPhase: 0
isTrusted: true
path: (5) [dialog#dialog, body, html, document, Window]
returnValue: true
srcElement: dialog#dialog
target: dialog#dialog
timeStamp: 3902.0349998027086
type: "close"
```

属性让我们给它分配自己的事件处理函数来处理`canplaythrough`事件。当媒体下载完成时，触发`canplaythrough`事件，这样浏览器就不必停下来缓冲以从头到尾播放媒体。属性让我们给它分配一个事件处理器来处理`change`事件。当用户提交表单控件的值更改时，触发`change`事件。我们可以给元素的`onclick`属性分配一个事件处理程序，当用户点击一个元素时做一些事情。为了处理`dialog`元素关闭时的情况，我们可以使用`onclose`事件处理程序，因为`close`事件是在它关闭时触发的。`close`事件与`cancel`事件的不同之处在于`cancel`事件只有在按下 Esc 键关闭对话框时才会被触发，而`close`事件无论用户以何种方式关闭对话框都会被触发。