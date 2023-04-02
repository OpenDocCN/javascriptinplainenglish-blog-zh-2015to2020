# 可维护的 JavaScript —生命和事件

> 原文：<https://javascript.plainenglish.io/maintainable-javascript-iifes-and-events-965f09f2359b?source=collection_archive---------10----------------------->

![](img/0c97294e1e92d6f6c752fa808291aa46.png)

Photo by [Photos by Lanty](https://unsplash.com/@photos_by_lanty?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果想继续使用代码，创建可维护的 JavaScript 代码很重要。

在本文中，我们将通过在 UI 中避免全局变量和松散耦合来了解创建可维护 JavaScript 代码的基础。

# 没有全球性的方法

我们可以创建没有全局变量的私有变量。

我们把我们需要的全局对象传递到生活中，我们不用改变它就可以使用它。

例如，我们可以写:

```
(function(win) {
  const doc = win.document;
  // ...
}(window));
```

我们创建自己的生命，然后用`window`对象调用，这样我们就可以访问`window.document`并将其赋给一个变量。

那我们就可以随心所欲了。

函数包装器可以用于我们不想创建任何全局对象的脚本。

这种模式用途有限。

页面上其他脚本使用的任何脚本都不能使用这种方法。

如果我们有不与任何其他脚本交互的小脚本，那么可以使用这种方法。

# 事件处理

事件处理一直在客户端 JavaScript 中进行。

我们的应用程序必须处理来自输入，鼠标点击，滚动等事件。

如果我们不小心的话，也有可能产生紧密耦合的代码。

`event`对象在事件处理函数中作为一个参数可用。

我们可以很容易地用它来创建紧密耦合的代码。

为了确保我们不会创建紧密耦合的代码，我们应该记住一些事情。

# 独立的应用程序逻辑

我们需要分离应用程序逻辑，这样我们就不会创建耦合到 UI 的应用程序逻辑代码。

事件处理程序中的应用程序逻辑代码很难跟踪和测试。

此外，当我们可以重用逻辑代码时，我们可能会重复它们。

测试更加困难，因为我们要创建与事件处理代码相耦合的逻辑代码。

我们必须触发事件来测试我们的应用程序逻辑代码，即使我们只是想测试逻辑。

所以我们要把 app 逻辑代码和事件处理代码分开。

例如，我们可以通过编写以下代码将应用程序逻辑代码与事件句柄分开:

```
function onClick(event) {
  showPopup(event);
}function showPopup(event) {
  const popup = document.getElementById("popup");
  popup.style.left = `${event.clientX}px`;
  popup.style.top = `${event.clientY}px`;
  popup.className = "popup";
}const button = document.querySelector('button');
button.addEventListener('click', onClick);
```

我们用`showPopup`函数显示弹出窗口，用`onClick`方法监听按钮上的点击。

当`onClick`运行时`showPopup`运行，这样我们就不会在事件处理函数中包含所有的逻辑。

# 不要传递事件对象

我们不应该传递事件对象，因为它依赖于调用事件处理函数的元素。

我们希望将应用程序逻辑从 DOM 元素中分离出来，所以我们不应该将`event`对象作为应用程序逻辑函数中的参数。

我们不应该取`showPopup`中的`event`对象，而应该只取 x 和 y 坐标。

例如，我们可以写:

```
function onclick(event) {
  const {
    clientX,
    clienty
  } = event;
  showPopup(clientX, clienty);
}function showPopup(x, y) {
  const popup = document.getElementById("popup");
  popup.style.left = `${x}px`;
  popup.style.top = `${y}px`;
  popup.className = "popup";
}const button = document.querySelector('button');
button.addEventListener('click', onClick);
```

我们更改了`showPopup`函数来获取弹出窗口的 x 和 y 坐标，这样我们就不必将`event`对象传递给我们的应用程序逻辑。

这使我们的事件处理代码与应用程序逻辑代码保持松散耦合。

![](img/16293af87a415d0f92e3d3cfe5173f52.png)

Photo by [Alasdair Elmes](https://unsplash.com/@alelmes?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该将应用程序逻辑代码从事件处理代码中分离出来。

此外，我们可以创建函数来获取全局变量，并在函数内部使用它们。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**