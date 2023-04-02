# JavaScript 事件处理程序— onvisibilitychange 事件、onanimationcancel 等等

> 原文：<https://javascript.plainenglish.io/javascript-events-handlers-onvisibilitychange-event-onanimationcancel-and-more-de974c205ed5?source=collection_archive---------3----------------------->

![](img/dc7ff935a4cddd67b5b00b001462ce6a.png)

Photo by [George Gvasalia](https://unsplash.com/@ggvasalia?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在 JavaScript 中，事件是应用程序中发生的动作。它们是由各种事情触发的，比如输入、提交表单、调整大小等元素变化，或者应用程序运行时发生的错误等。我们可以分配事件处理程序来处理这些事件。发生在 DOM 元素上的事件可以通过为相应事件的 DOM 对象的属性分配一个事件处理程序来处理。在本文中，我们查看了标准的`onvisibilitychange`事件，以及非标准的`onanimationcancel`、`onanimationend`和`onanimationiteration`事件。

# 视觉变化

`onvisibilitychange`是一个事件处理程序，当`visibilitychange`事件被触发并到达对象时运行。我们定义自己的事件处理函数，然后将它设置为这个属性。例如，我们可以编写以下代码来记录当前页面的可见性:

```
document.onvisibilitychange = (event) => {
  console.log("Visibility changed!");
  console.log(event);
};
```

对象有事件的细节。我们应该可以看到`event`对象的`type`属性是`'visibilitychange’`。

# onanimationcancel

通过`onanimationcancel`事件，我们可以在`animationcancel`事件发生时附加一个事件处理程序，也就是当 CSS 动画意外中止时，比如当我们停止运行动画而没有触发`animationend`事件时，这可能发生在动画名称被更改、动画被删除，或者当它直接或通过 CSS 中的更改被隐藏时。`onanimationcancel`仅在 Firefox 中触发。例如，如果我们有以下 HTML 代码来创建一个框和一个隐藏该框的按钮:

```
<button id='hide-button'>
  Hide Box
</button><div id='container'>
  <div id='box'> </div>
</div>
```

然后，我们添加以下 CSS 来设置盒子的样式，并将大小更改为 100 x 100 像素，并创建动画:

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
  animation: 5s slideBox;
}#container {
  height: 500px;
}[@keyframes](http://twitter.com/keyframes) slideBox {
  from {
    left: 0;
    top: 0;
  } to {
    left: calc(100% - 100px);
    top: calc(100% - 100px)
  }
}
```

在上面的 CSS 中，最初我们把盒子放在页面的左上角，并用红色填充。我们有`slideBox`动画序列，通过在每一帧中向右滑动 100 个像素和向下滑动 100 个像素，从页面的左上角到右下角喜爱这个框。

然后，我们添加以下 JavaScript 代码:

```
const hideButton = document.getElementById('hide-button');
const box = document.getElementById('box');document.onanimationcancel = () => {
  console.log('Animation cancelled unexpectedly');
};hideButton.onclick = () => {
  box.style.display = 'none';
}
```

然后，当我们点击“隐藏框”按钮，然后我们得到“动画意外取消”记录在 Firefox 中。当点击“隐藏框”按钮时，不会触发`animationcancel`事件，所以它似乎没有跨浏览器的一致实现。

![](img/71d6a39a605e88e2f51a0258d7dd77b0.png)

Photo by [Steven Roe](https://unsplash.com/@steveroe_?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# onanimationend

`onanimationend`属性是一个属性，我们可以将它的值设置为一个事件处理函数，该函数在`animationend`事件被触发时被触发。当 CSS 动画达到其活动周期的末尾时触发，活动周期是动画持续时间的总和乘以动画迭代次数和动画延迟。例如，如果我们有以下 HTML 代码:

```
<div id='container'>
  <div id='box'> </div>
</div>
```

然后，我们添加以下 CSS 来设置盒子的样式，并将大小更改为 100 x 100 像素，并创建动画:

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
  animation: 5s slideBox;
}#container {
  height: 500px;
}[@keyframes](http://twitter.com/keyframes) slideBox {
  from {
    left: 0;
    top: 0;
  } to {
    left: calc(100% - 100px);
    top: calc(100% - 100px)
  }
}
```

在上面的 CSS 中，最初我们把盒子放在页面的左上角，并用红色填充。我们有`slideBox`动画序列，通过在每一帧中向右滑动 100 像素和向下滑动 100 像素，从页面的左上角到右下角爱上这个盒子。

然后，我们可以添加以下 JavaScript 代码，通过我们的`onanimationend`事件处理程序记录`animationend`事件:

```
const box = document.getElementById('box');document.onanimationend = () => {
  console.log('Animation ended');
};
```

注意这个事件处理程序只在 Firefox 上工作，因为这个事件的实现还没有被标准化，所以我们不应该期望这个事件被触发和事件处理函数被调用。

# onanimationiteration

为了在 CSS 动画运行时做一些事情，我们可以通过给属性分配一个事件处理函数来使用`onanimationiteration`属性。当`animationiteration`事件被触发时，我们分配给这个属性的事件处理程序将被调用。

当 CSS 动画的一次迭代结束，另一次迭代开始时，触发`animationiteration`。

该事件不会与`animationend`事件同时触发，如果动画迭代计数为 1，它也不会被触发。如果我们设置正在被动画化的元素的`animation-iteration-count`属性，使元素重复动画化，那么这个事件将被触发。例如，如果我们在 HTML 代码中有和以前一样的 box `div`元素:

```
<div id='container'>
  <div id='box'> </div>
</div>
```

但是我们通过添加`animation-iteration-count`属性来改变 CSS 代码:

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
  animation: 5s slideBox;
  animation-iteration-count: 5;
}#container {
  height: 500px;
}[@keyframes](http://twitter.com/keyframes) slideBox {
  from {
    left: 0;
    top: 0;
  } to {
    left: calc(100% - 100px);
    top: calc(100% - 100px)
  }
}
```

然后切换 JavaScript 代码，使用以下代码将事件处理程序设置为`onanimationiteration`属性:

```
const box = document.getElementById('box');let iterationCount = 0;document.onanimationiteration = () => {
  console.log('Animation is running');
  iterationCount++;;
  console.log(iterationCount);
};
```

然后在 Firefox 中，我们应该看到以下记录:

```
Animation is running1Animation is running2Animation is running3Animation is running4
```

我们看到每次动画运行时`iterationCount`的值增加 1，所以事件处理程序由火狐中的`animationiteration`事件触发运行。这个事件是一个实验性的事件，只是到目前为止在火狐中才有，和我们看到的其他动画事件一样，这是一个正在进行的工作。

`onvisibilitychange`是当`visibilitychange`事件被触发并到达物体时运行的事件处理程序。这是一个标准事件，适用于大多数现代浏览器。

当从它开始的页面不可见时，我们可以用它来记录情况，反之亦然。

然后我们看了非标准的`onanimationcancel`、`onanimationend`和`onanimationiteration`，它们只有在和 Firefox 一起使用时才会被触发。

其他浏览器不运行这些事件处理程序，我们可以推断相应的`animationcancel`、`animationend`和`animationiteration`事件没有被触发。

这些事件仅在 CSS 动画期间当动画意外取消时才会在火狐上触发，比如当动画结束时被动画元素突然移除，以及当动画重复时。