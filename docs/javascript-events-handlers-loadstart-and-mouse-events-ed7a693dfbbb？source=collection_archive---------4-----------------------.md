# JavaScript 事件处理程序— LoadStart 和鼠标事件

> 原文：<https://javascript.plainenglish.io/javascript-events-handlers-loadstart-and-mouse-events-ed7a693dfbbb?source=collection_archive---------4----------------------->

![](img/d8341e84a6f702c874df0a4dcf3a01bd.png)

Photo by [Ryan Stone](https://unsplash.com/@rstone_design?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在 JavaScript 中，事件是应用程序中发生的动作。它们是由各种事情触发的，比如输入、提交表单、调整大小等元素变化，或者应用程序运行时发生的错误等。我们可以分配一个事件处理器来处理这些事件。发生在 DOM 元素上的事件可以通过为相应事件的 DOM 对象的属性分配一个事件处理程序来处理。在本文中，我们将看看如何使用`onloadstart`、`onlostpointercapture`和`onmouseup`事件处理程序。

# `**onloadstart**`

`onloadstart`属性是 XmlHttpRequest 对象的一部分。它允许我们给它分配一个事件处理函数来处理数据刚开始加载时触发的`loadstart`事件。

例如，我们可以为`onloadstart`事件分配一个事件处理程序，就像我们在下面的代码中所做的那样:

```
const loadButtonSuccess = document.querySelector('.load.success');
const loadButtonError = document.querySelector('.load.error');
const loadButtonAbort = document.querySelector('.load.abort');
const log = document.querySelector('.event-log');function handleLoadStart(e) {
  console.log(e);
  log.textContent = log.textContent + `${e.type}: ${e.loaded} bytes transferred\n`;
}function addListeners(xhr) {
  xhr.onloadstart = handleLoadStart;
}function get(url) {
  log.textContent = '';
  const xhr = new XMLHttpRequest();
  addListeners(xhr);
  xhr.open("GET", url);
  xhr.send();
  return xhr;
}
loadButtonSuccess.addEventListener('click', () => {
  get('[https://jsonplaceholder.typicode.com/todos/1'](https://jsonplaceholder.typicode.com/todos/1'));
});
loadButtonError.addEventListener('click', () => {
  get('[https://somewhere.org/i-dont-exist'](https://somewhere.org/i-dont-exist'));
});
loadButtonAbort.addEventListener('click', () => {
  get('[https://jsonplaceholder.typicode.com/todos/1').abort();](https://jsonplaceholder.typicode.com/todos/1').abort();)
});
```

在上面的代码中，我们有 3 个按钮，每当每个按钮被点击时都会运行`click`事件处理程序。点击“加载(成功)”按钮将运行`get` 功能。我们将在对`get`函数的调用中传递一个有效的 URL。“加载(成功)”按钮的点击处理由以下模块完成:

```
loadButtonSuccess.addEventListener('click', () => {
    get('[https://jsonplaceholder.typicode.com/todos/1'](https://jsonplaceholder.typicode.com/todos/1'));
});
```

JSONPlaceholder 有一个可以为任何 URL 服务的测试 API，因为它托管了一个假 API，所以我们可以加载它而不会得到任何错误。同样，我们有按钮加载一个会给出错误的 URL，还有另一个按钮加载一个有效的 URL，但是我们中止请求。一旦 XmlHttpRequest 完成，我们分配给`onloadstart`事件处理程序的函数，也就是`handleLoadStart`函数，将会运行。

`handleLoadStart`函数有一个参数，它是一个`Event`对象，包含一些关于已完成请求的数据。在该函数中，我们获得了`type`属性的值，该属性具有被触发的事件类型，应该是`loadstart`。此外，我们还获得了属性`loaded`的值，它包含了已经加载的数据的字节数。它应该总是 0，因为当`loadstart`事件被触发时，还没有加载任何东西。

然后在 HTML 代码中，我们添加上面的`querySelector`调用中列出的元素:

```
<div class="controls">
  <input class="load success" type="button" name="xhr" value="Load Start (success)" />
  <br>
  <input class="load error" type="button" name="xhr" value="Load Start (error)" />
  <br>
  <input class="load abort" type="button" name="xhr" value="Load Start (abort)" />
</div>
<textarea readonly class="event-log"></textarea>
```

我们有 3 个按钮可以点击来加载成功的 HTTP 请求，一个带有不存在的 URL 的 HTTP 请求，和一个中止的 HTTP 请求。然后我们显示触发的事件和加载的字节数。

# onlostpointercapture

DOM 元素的`onlostpointercapture`属性让我们分配一个事件处理函数来处理`lostpointercapture`事件。当释放被捕获的指针时，触发`lostpointercapture`事件。捕获指针意味着特定的指针事件将被重新定向到特定的元素，而不是响应指针事件的普通元素。它用于确保一个元素继续接收指针事件，即使指针设备离开了该元素。

例如，我们可以使用它来记录`lostpointercapture`事件，就像我们在下面的代码中所做的那样:

```
const para = document.querySelector('p');para.onlostpointercapture = () => {
  console.log('Pointer been released!')
};para.onpointerdown = (event) => {
  para.setPointerCapture(event.pointerId);
};
```

然后在相应的 HTML 代码中，我们放入:

```
<p>Touch and Release</p>
```

在上面的代码中，当我们单击`p`元素时，首先触发`pointerdown`事件，因此我们分配给`para`对象的`onpointerdown`属性的事件处理程序运行，该对象是`p`元素的 DOM 对象。在事件处理函数中，我们在`para`对象上运行`setPointerCapture`方法，该方法将鼠标指针事件指向`p`元素。

然后当鼠标按钮被释放时，被捕获的指针被释放，因此触发了`lostpointercapture`方法。然后，我们分配给`onlostpointercapture`事件的事件处理程序运行，记录“指针被释放！”我们添加的消息。

![](img/19d2950cd40ebffa0f676dea80e117ca.png)

Photo by [Ricky Kharawala](https://unsplash.com/@sweetmangostudios?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# onmousedown

我们分配给 DOM 元素的`onmousedown`属性的事件处理函数在`mousedown`事件被触发时运行。当指针在元素内部时按下指点设备按钮，触发`mousedown`事件。这与`click`事件不同，因为`click`是在完全点击动作发生后触发的。这意味着当指针停留在同一元素内时，鼠标按钮既被按下又被释放。最初按下鼠标按钮时触发`mousedown`。

例如，我们可以使用`onmousedown`事件处理程序来创建一个偷窥效果，当我们点击屏幕的一部分时，图像中被点击的部分就会显示出来。

首先，我们为图像添加 HTML 代码，并添加一个黑色的`div`元素来隐藏图像，如下面的代码所示:

```
<div class='container'>
  <div class="view" hidden></div>
  <img src='[https://images.unsplash.com/photo-1503066211613-c17ebc9daef0?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1500&q=80'](https://images.unsplash.com/photo-1503066211613-c17ebc9daef0?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1500&q=80')></div>
```

然后，我们为 HTML 元素添加 CSS 代码，使`.view` `div`元素具有黑色背景:

```
.container {
  background: black;
  width: 500px;
}.view {
  position: absolute;
  width: 200px;
  height: 200px;
  background: white;
  border-radius: 50%;
}img {
  mix-blend-mode: darken;
  width: 500px;
}
```

然后在 JavaScript 代码中，我们添加了`onmousedown`事件处理程序，让我们显示鼠标指针点击过的图像部分:

```
const img = document.querySelector('img');
const view = document.querySelector('.view');
const container = document.querySelector('.container');const showView = (event) => {
  view.removeAttribute('hidden');
  view.style.left = event.clientX - 50 + 'px';
  view.style.top = event.clientY - 50 + 'px';
  event.preventDefault();
}const moveView = (event) => {
  view.style.left = event.clientX - 50 + 'px';
  view.style.top = event.clientY - 50 + 'px';
}const hideView = (event) => {
  view.setAttribute('hidden', '');
}container.onmousedown = showView;
container.onmousemove = moveView;
document.onmouseup = hideView;
```

`.container` `div`元素的`onmousedown`属性被设置为`showView`函数，该函数在鼠标按钮按下时运行。在函数内部，我们从`.view` `div`元素中移除了`hidden`属性，以显示`div`下面的图像。从拥有`Event`对象的`event`参数中，我们获得了`clientX`和`clientY`属性，它们拥有点击位置的鼠标坐标。我们将其设置为代表`.view`元素的`view` DOM 对象的位置。然后我们调用`event.preventDefault()`来停止默认动作，因为我们已经用它之前的代码进行了揭示。

`onmousemove`事件处理程序被设置为`moveView`函数，该函数处理`mousemove`事件。当鼠标移动时触发该事件。在函数中，我们将`.view`元素设置为鼠标指针当前所在的位置，同样使用`event`参数的`clientX`和`clientY`属性，该参数是`MouseEvent`对象。

然后，当鼠标按钮被释放时，`mouseup` is 事件被触发，我们分配给`document`对象的`onmouseup`属性的`hideView`函数被调用。我们设置了`.view`元素的`hidden`属性来隐藏`.view`元素。

`onloadstart`属性是 XmlHttpRequest 对象的一部分。它允许我们给它分配一个事件处理函数来处理数据刚开始加载时触发的`loadstart`事件。

DOM 元素的`onlostpointercapture`属性让我们分配一个事件处理函数来处理`lostpointercapture`事件。当释放被捕获的指针时，触发`lostpointercapture`事件。

我们分配给 DOM 元素的`onmousedown`属性的事件处理函数在`mousedown`事件被触发时运行。当指针在元素内部时按下指点设备按钮，触发`mousedown`事件。这与`click`事件不同，因为`click`是在完全点击动作发生后触发的。这意味着当指针停留在同一元素内时，鼠标按钮既被按下又被释放。最初按下鼠标按钮时触发`mousedown`。