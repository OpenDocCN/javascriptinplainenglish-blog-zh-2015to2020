# 在 JavaScript 中使用事件处理程序

> 原文：<https://javascript.plainenglish.io/using-event-handlers-in-javascript-3a6f61718e46?source=collection_archive---------0----------------------->

![](img/72353a3f5e79a3030d9b97e9de6698a9.png)

Photo by [Ezra Jeffrey-Comeau](https://unsplash.com/@emcomeau?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

任何具有动态功能的网站都需要处理与它们交互的用户。这是用户可以使用的页面控件。为了让用户使用这些控件，页面必须能够处理用户对控件的操作。这些是 JavaScript 中事件的例子。事件还包括浏览器完成的其他活动，如页面加载。

有了 HTML DOM，JavaScript 可以从中获取元素并处理事件。一些事件根据它们所应用的元素进行分组。其他事件适用于除 body 和 frameset 元素之外的所有元素。

所有 HTML 元素支持的事件列表如下:

*   `abort`，文件加载中止时触发
*   `change`，值改变时触发
*   `click`，点击鼠标或轻击屏幕时触发
*   `dbclick`，鼠标点击两次触发
*   `input`，当`input`或`textarea`元素的输入值改变时触发
*   `keydown`，按下键时触发
*   `keyup`，按键按下后松开时触发
*   `mousedown`，按下鼠标按钮时触发
*   `mouseenter`，释放鼠标按钮时触发
*   `mouseleave`，当鼠标离开一个元素时触发
*   `mousemove`，鼠标移动到元素上时触发
*   `mouseout`，当鼠标离开一个元素时触发
*   `mouseover`，鼠标悬停在元素上时触发
*   `mouseup`，释放鼠标按钮时触发
*   `mousewheel`，鼠标滚轮旋转时触发
*   `onreset`，表格重置时触发
*   `select`，选择文本时触发
*   `submit`，提交表单时触发
*   `drag`，拖动元素时触发
*   `dragend`，拖动元素时触发
*   `dragenter`，当被拖动元素进入放置目标时触发
*   `dragstart`，元素开始拖动时触发
*   `dragleave`，当拖动的元素离开有效的放置目标时触发
*   `dragover`，当元素或文本选择被拖动到有效的放置目标上时触发
*   `drop`，当元素被放置在有效的放置目标上时触发

下面是一些 HTML 音频和视频事件:

*   `pause`，媒体播放开始时触发
*   `play`，回放开始时触发
*   `playing`，媒体播放时触发
*   `seeking`，搜索媒体时触发
*   `seeked`，媒体寻道完成时触发

除了`body`和`frameset`元素之外的每个元素支持的事件如下:

*   `blur`，当元素失去焦点时触发
*   `error`，文件加载失败时触发
*   `focus`，当元素处于焦点时触发
*   `load`，加载文件和附件时触发
*   `resize`，调整文档大小时触发
*   `scroll`，滚动元素时触发

`window`对象支持的事件如下:

*   `afterprint`，打印预览窗口关闭或文档开始打印时触发
*   `beforeprint`，当打印预览窗口打开或文档将要打印时触发
*   `beforeunload`，卸载单据时触发
*   `hashchange`，当 URL 中带有井号(#)的部分发生变化时触发
*   `pagehide`，当浏览器离开浏览器历史中的页面时触发
*   `pageshow`，浏览器转到页面时触发
*   `popstate`，当会话历史项改变时触发
*   `unload`，卸载文档和包含的文件时触发

浏览器可以处理更多的事件。他们在[https://developer.mozilla.org/en-US/docs/Web/Events](https://developer.mozilla.org/en-US/docs/Web/Events)上市。

![](img/0dc09ffaec3b03ffea956f59a8763e89.png)

Photo by [Nils Stahl](https://unsplash.com/@nilsjakob?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 事件处理

当浏览器的 JavaScript 代码响应这些事件时，它被称为事件处理。JavaScript 中有几种处理事件的方法。

## 内嵌事件处理程序

我们可以直接在元素代码中添加事件处理程序。我们可以在 HTML 元素的属性中编写代码来处理代码。例如，如果我们想在按钮被点击时处理事件，我们可以将事件处理代码放在按钮元素的`onclick`属性中，如下所示:

```
<button onclick='alert('Button clicked')>Click me</button>
```

很可能我们想要阻止默认动作的发生，比如导航到一个新的页面或者当点击一个按钮时触发提交。为了防止这些默认动作发生，我们可以将`return false`放在脚本的末尾。如果我们在一个`a`标签中处理`onclick`事件，我们可以做以下事情来防止导航到一个新的页面:

```
<a onclick='alert('Clicked'); return false;'>Click me</a>
```

## 处理脚本标记中的事件

内联事件处理程序不好，因为您在 HTML 中混合了动态代码，而 HTML 应该只关心将页面组织成有意义的部分。此外，如果您有复杂的代码，那么很难将所有代码都放在属性中。我们应该将事件处理程序附加到元素上，方法是将它们放入专用代码中，然后将代码放入事件处理程序的回调函数中来处理事件。

例如，如果我们有当您按下不同按钮时增加和减少计数器的代码，我们可以编写如下代码。

我们创建一个新文件夹，然后在`index.html`中，我们放入:

```
<html>
  <head>
    <title>Counting App</title>
  </head> <body>
    <h1>Click the button to count.</h1>
    <p>Current Number: <span id="currentCount">0</span></p>
    <button id="increaseButton">Increase</button>
    <button id="decreaseButton">Decrease</button>
    <script src="script.js"></script>
  </body>
</html>
```

然后在`script.js`中，我们把:

```
const handleIncrease = () => {
  const currentCount = document.getElementById("currentCount");
  const increaseButton = document.getElementById("increaseButton");
  increaseButton.onclick = () => {
    currentCount.innerHTML++;
  };
};const handleDecrease = () => {
  const currentCount = document.getElementById("currentCount");
  const decreaseButton = document.getElementById("decreaseButton");
  decreaseButton.onclick = () => {
    currentCount.innerHTML--;
  };
};const initialize = () => {
  handleIncrease();
  handleDecrease();
};window.onload = initialize;
```

在`script.js`中，我们有处理`index.html`中按钮点击的函数。我们通过使用`getElementById`获取元素对象来附加事件处理程序，然后我们使用元素自己的匿名事件处理程序函数来设置元素的`onclick`属性。如果我们想给它分配一个函数引用，我们可以像对`window.onload`那样做。在处理程序中，我们获取 ID 为`currentCount`的元素的内容，然后在获取原始内容后修改`innerHTML`属性来更改内容。在最后一行，我们将变量`initialize`赋给它。我们不想立即调用它，而是在事件实际发生时调用，所以在`initialize`后没有括号。我们在函数上给它分配了引用**而不是立即调用它并返回结果。**

## addEventListener

将事件处理程序附加到函数的另一种方式是使用 HTML 元素对象可用的`addEventListener`函数。`addEventListener`事件监听触发的 DOM 事件，并在事件发生时运行您传递给它的回调函数。回调函数有一个参数，包含被触发事件的详细信息。有了`addEventListener`，我们可以重用代码，因为我们可以在多个地方使用同一个回调函数。您还可以控制事件的触发方式，因为我们有包含事件细节的参数。它也适用于 DOM 树中的非元素节点。

如果我们把上面的例子改成使用`addEventListener`，我们可以把`script.js`中的替换成:

```
const handleClick = e => {
  const currentCount = document.getElementById("currentCount");
  if (e.target.id === "increaseButton") {
    currentCount.innerHTML++;
  } if (e.target.id === "decreaseButton") {
    currentCount.innerHTML--;
  }
};const handleIncrease = () => {
  const increaseButton = document.getElementById("increaseButton");
  increaseButton.addEventListener("click", handleClick);
};const handleDecrease = () => {
  const decreaseButton = document.getElementById("decreaseButton");
  decreaseButton.addEventListener("click", handleClick);
};const initialize = () => {
  handleIncrease();
  handleDecrease();
};window.onload = initialize;
```

正如我们所见，我们使用了相同的`handleClick`函数来处理按钮`increaseButton`和`decreaseButton`的点击。我们通过使用`e.target.id`属性来检查哪个元素触发了 click 事件，该属性具有元素的 id 属性。在同一个事件处理函数中为不同的元素做不同的动作称为**事件委托**。当不同的元素被触发时，我们委派不同的动作。与第一个例子相比，重复的代码更少。

`addEventListener`函数有 3 个参数。第一个是包含事件名称的字符串，第二个是处理事件的回调函数。最后一个指示父元素是否也与 then 事件相关联。如果是则为`true`，否则为`false`。

如果有一个父元素也与该事件相关联，那么您可以将该事件传播到父元素。这是一个冒泡事件的调用，如果它冒泡了，父元素也会触发该事件。还有一个捕获方法，让父元素的事件先发生，子元素在后发生。它很少在代码中使用，所以我们不必太担心这一点。

事件冒泡的一个例子如下。如果我们在`index.html`中有以下内容:

```
<html>
  <head>
    <title>Click App</title>
  </head> <body>
    <div id="grandparent">
      <div id="parent">
        <button id="child">
          Click Me
        </button>
      </div>
    </div>
    <script src="script.js"></script>
  </body>
</html>
```

以及`script.js`中的以下内容:

```
window.onload = () => {
  const child = document.getElementById("child");
  const parent = document.getElementById("parent");
  const grandparent = document.getElementById("grandparent");
  child.addEventListener(
    "click",
    () => {
      alert("Child Clicked");
    },
    true
  ); parent.addEventListener(
    "click",
    () => {
      alert("Parent Clicked");
    },
    true
  ); grandparent.addEventListener(
    "click",
    () => {
      alert("Grandparent Clicked");
    },
    true
  );
};
```

然后，当点击“Click me”按钮时，我们应该会得到 3 个警告，因为我们指定让点击事件一直冒泡到顶部元素。

# 停止事件传播

如果我们只希望事件发生在你想要的那个上，那么你必须阻止它传播到父节点。如果我们将前面示例中的`script.js`修改为:

```
window.onload = () => {
  const child = document.getElementById("child");
  const parent = document.getElementById("parent");
  const grandparent = document.getElementById("grandparent");
  child.addEventListener(
    "click",
    e => {
      if (e.stopPropagation) {
        e.stopPropagation();
      }
      alert("Child Clicked");
    },
    true
  ); parent.addEventListener(
    "click",
    e => {
      if (e.stopPropagation) {
        e.stopPropagation();
      }
      alert("Parent Clicked");
    },
    true
  ); grandparent.addEventListener(
    "click",
    e => {
      if (e.stopPropagation) {
        e.stopPropagation();
      }
      alert("Grandparent Clicked");
    },
    true
  );
};
```

我们应该只看到“祖父单击”,因为我们用`stopPropagation`函数阻止了事件的传播。停止传播应该会使您的代码更快，因为冒泡和捕获需要资源。