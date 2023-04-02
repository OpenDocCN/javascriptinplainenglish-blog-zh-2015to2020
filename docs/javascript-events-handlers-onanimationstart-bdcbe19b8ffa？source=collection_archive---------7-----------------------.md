# JavaScript 事件处理程序— onanimationstart

> 原文：<https://javascript.plainenglish.io/javascript-events-handlers-onanimationstart-bdcbe19b8ffa?source=collection_archive---------7----------------------->

![](img/9e5bea95bdc97429be921a26cb99652a.png)

Photo by [Tamara Gore](https://unsplash.com/@thenightstxlker?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在 JavaScript 中，事件是应用程序中发生的动作。它们是由各种事情触发的，比如输入、提交表单、调整大小等元素变化，或者应用程序运行时发生的错误等。我们可以分配一个事件处理器来处理这些事件。发生在 DOM 元素上的事件可以通过为相应事件的 DOM 对象的属性分配一个事件处理程序来处理。在这一部分，我们仔细看看`onanimationstart`事件处理程序和`animationstart`事件。

# onanimationstart

如果我们想处理 CSS 动画开始播放时的情况，也就是当`animationstart`事件被触发时，我们可以给`onanimation`属性分配我们自己的事件处理函数。我们可以使用 CSS 创建一个动画，通过首先创建 HTML 元素来动画化一个盒子，这些元素将像下面的代码一样被动画化:

```
<div id='container'>
  <div id='box'> </div>
</div>
```

然后，我们向元素添加一些样式，并用 CSS 创建动画，如以下代码所示:

```
#box {
  width: 100px;
  height: 100px;
  left: 0;
  top: 0;
  border: 1px solid red;
  margin: 0;
  position: relative;
  background-color: red;
  display: block;
  justify-content: center;
  animation: 5s resizeBox;
}#container {
  height: 500px;
}[@keyframes](http://twitter.com/keyframes) resizeBox {
  from {
    transform: translateX(100%) scaleX(3);
  } to {
    transform: translateX(0) scaleX(1);
  }
}
```

在上面的代码中，我们将框的大小改为 100 像素乘 100 像素。，将填充颜色更改为红色，然后将框放在我们页面的左上角。然后我们创建了`resizeBox`动画，将盒子水平缩放 3 倍，然后缩小到原始尺寸。我们重复这个动作 5 秒钟。在 CSS 代码中创建动画后，我们可以使用以下代码将事件处理程序设置为`onanimationstart`属性:

```
document.onanimationstart = (event) => {
  console.log(event);
  console.log('Animation starts');
};
```

当上面的事件处理函数运行时，我们在事件处理函数的参数中得到`AnimationEvent`对象。它从具有以下属性的`Event`对象继承属性:

*   `bubbles` —是一个只读布尔属性，指示事件是否在 DOM 树中冒泡。
*   `cancelBubble` —是`stopPropagation`方法的历史别名。在从事件处理程序返回之前将其值设置为`true`可以防止事件的传播。
*   `cancelable` —只读布尔属性，表示事件是否可以取消。
*   `composed` —是一个只读布尔属性，指示事件是否可以跨越阴影 DOM 和常规 DOM 之间的边界。
*   `currentTarget` —只读属性，引用当前注册的事件目标。这是当前计划将事件发送到的对象，但有可能在重定目标的过程中已经发生了变化。
*   `deepPath` —是事件冒泡通过的 DOM 节点数组。
*   `defaultPrevented` —是一个只读的布尔属性，指示是否在事件上调用了`event.preventDefault()`。
*   `eventPhase` —只读属性，表示正在处理事件流的哪个阶段。
*   `explicitOriginalTarget` —是只读属性，具有事件的显式原始目标。该属性仅在 Mozilla 浏览器中可用。
*   `originalTarget` —是一个只读属性，在任何重定目标之前具有事件的原始目标。该属性仅在 Mozilla 浏览器中可用。
*   `returnValue` —是由 Internet Explorer 引入的历史属性，最终被纳入 DOM 规范，以确保现有站点继续工作。理想情况下，您应该尝试使用`preventDefault()`和`defaultPrevented` 来取消事件并检查事件是否被取消，但是如果您选择这样做，也可以使用 returnValue。
*   `srcElement` —是旧版本 Microsoft Internet Explorer 中用于`target`的非标准别名，出于 web 兼容性目的，其他一些浏览器也开始支持该别名。
*   `target` —只读属性，引用事件最初调度到的目标。
*   `timeStamp` —只读，包含事件的创建时间(以毫秒为单位)。根据规范，该值是自 1970 年 1 月 1 日以来的时间，但实际上浏览器的定义各不相同。
*   `type` —只读属性，具有事件的名称(不区分大小写)。
*   `isTrusted` —是一个只读属性，指示事件是由用户交互后的浏览器发起的，还是由使用类似`initEvent`的事件创建方法的脚本发起的。

![](img/31d3f82a5aa4c035ba614b0674e98ac1.png)

Photo by [Matt Popovich](https://unsplash.com/@uh?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

上面的列表是部分属性。它只包含`Event`对象的值属性。`AnimationEvent`继承属性并添加自己的属性，如下所示:

*   `animationName` —只读属性，返回包含 CSS 动画的动画名称值的字符串。
*   `elapsedTime` —只读浮点属性，具有从事件被触发时起动画已经运行的时间量(以秒为单位)。对于`animationstart`事件，`elapsedTime`为 0.0，除非`animation-delay` CSS 属性为负值，在这种情况下，事件将被触发，而`elapsedTime`为负一乘以延迟。
*   `pseudoElement` —以`::`开头的字符串只读属性，包含运行动画的伪元素的名称。如果动画不是在伪元素中运行，那么这个属性的值将是一个空字符串。

如果我们运行上面的代码，我们应该会记录下面的`AnimationEvent`对象:

```
animationName: "resizeBox"
bubbles: true
​cancelBubble: false
​cancelable: false
​composed: false
​currentTarget: null
​defaultPrevented: false
​elapsedTime: 0
​eventPhase: 0
​explicitOriginalTarget: <div id="box">
​isTrusted: true
​originalTarget: <div id="box">
​pseudoElement: ""
​returnValue: true
​srcElement: <div id="box">​target: <div id="box">
​timeStamp: 297
​type: "animationstart"
```

正如我们看到的,`animationName`是`“resizeBox”`,这是我们 CSS 动画的名字。因为我们在一个`onanimationstart`事件处理器中记录这个`Event`对象，所以`elapsedTime`为 0，并且我们的动画没有负延迟。`pseudoElement`是一个空字符串，因为我们没有在任何伪元素中运行我们的动画。

该属性和`animationstart`正在进行中。事件处理程序只在 Firefox 中运行。它不能在 Chrome 或其他流行的现代浏览器中运行。这意味着如果我们想让它在 Firefox 之外的任何东西上工作，就不应该在生产环境中依赖它。

CSS 动画开始时会触发`animationstart`事件。到目前为止，它只在 Firefox 中触发，标准化该事件的工作正在进行中。这对于获取动画开始运行时的开始事件和经过时间以及正在运行的动画的名称非常方便。