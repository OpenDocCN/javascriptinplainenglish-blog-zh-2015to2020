# 了解去抖动和节流

> 原文：<https://javascript.plainenglish.io/understanding-debouncing-throttling-2a0a5e9cc74a?source=collection_archive---------1----------------------->

## 作为开发人员，您应该知道的两项技术

![](img/50125802296fed52a302910fec8a32f0.png)

A woman with a laptop

去抖和节流是两种控制函数执行次数的方法。当我们将函数附加到 DOM 的事件时，函数的去抖或节流版本特别有用，因为在这些场景中，我们可能会在不必要的时候调用函数。

## 去抖动

去抖模式让我们控制连续触发的事件，如果两个事件之间的间隔小于一定的时间，它会完全忽略第一个事件。

重复这个过程，直到它得到高于期望的最小间隔的暂停。一旦发生，将只执行暂停前事件的最后一次发生，而忽略之前的所有事件。

简而言之:去抖动使得一个函数在没有被调用的情况下，直到一个特定的时间集合结束后才能被再次调用。示例:仅当超过 1000 毫秒未调用函数时，才执行该函数。

让我们看一个网站上搜索栏的例子。每当我们在搜索栏中键入一些内容时，我们都会进行一次 API 调用，根据在搜索栏中键入的字母从函数服务器中获取数据:

```
<html>
    <body>
        <input  type="text"  
        id="searchId" />
    </body>    
    <script src="debounce.js"> </script>    
    <script src="search.js"> </script>
</html>
```

这里我们的 **makeApiFetch** 函数在每次用户在文本框中键入时被调用，并且后端调用被完成以恢复数据，正如您所猜测的，这在后端和 UX 级别都是低效的。

```
//search.js
let searchIdDom =  
 document.getElementById('searchId');//1.
const makeApiFetch () = > {
   ...
}//2.
searchIdDom.addEventListener('input', () => {
   makeApiFetch();
}
```

1.对服务器执行昂贵的搜索并获取结果。

2.在我们的文本框搜索栏上添加一个事件监听器

为了解决这个问题，我们将创建一个函数，允许我们控制连续触发的事件，并且只执行最后一个事件:

```
//debounce.js
const debounce = (callback, delay) => {
  let timeout = null
  return (...args) => {
    const next = () => 
    callback(...args);
    clearTimeout(timeout);
    //1\.   
    timeout = setTimeout(next, delay)
  }
}
```

1.  我们延迟回调的执行，直到延迟时间过去。

因此，我们没有在 addEventListener 中直接调用 makeApiFetch 函数，而是用我们的 wrap **去抖**函数调用它:

```
searchIdDom.addEventListener('input', () => {
   //1.
   **debounce**(makeApiFetch(), 1000);
}
```

现在，makeApiFetch 函数只有在 1000 毫秒的时间间隔过后才会被调用。

## 一些典型的案例使用

*   去反跳输入类型事件处理程序。(就像我们的搜索输入示例一样)
*   解除滚动事件处理程序的反跳。

## 节流

节流模式限制了一个函数可以被调用的最大次数。此方法通常用于控制调整大小、滚动和鼠标相关的事件。

简而言之:节流强制规定了一个函数可以被调用的最大次数。示例:每 1000 毫秒最多在**执行一次该功能。**

节流可以通过多种方式实现。我们可以通过单位时间内由大量事件触发的事件数量，或者通过两个处理的事件之间的延迟来调节。这里我们将实现最后一个选项，因为这是最简单的方法。

在这个例子中，当函数第一次被调用时，我们设置“时间”变量。每次调用返回的函数时，它都会检查等待时间是否已过，如果是，它会触发回调并重置时间:

```
function throttle(callback, wait) {
 var time = Date.now();
 return function() {
  if ((time + wait - Date.now()) < 0) {
    callback();
    time = Date.now();
  }
 }
}
```

使用它:

```
const myHandler = 
 (event) => {...do some stuf...}//1.
const myThrottleHandler =
  **throttled**(1000, myHandler);//2.
myMouseDomElement.
 addEventListener("mousemove", myThrottleHandler);
```

1.用我们的**节流的**函数包装“myHandler”函数。

2.在 myMouseDomElement 上添加一个事件侦听器。(鼠标 dom 元素)

## 一些典型的案例使用

*   抑制 API 调用。
*   抑制按钮点击，这样我们就不能垃圾点击。
*   抑制触摸/移动鼠标事件处理程序。

## 结论

在本文中，我们已经看到了去抖和节流模式的两个简单实现，虽然我们可以使用各种库中包含的实现，如 [lodash.js](https://lodash.com/) 或 [Jquery](https://jquery.com/) 去抖和节流是我们作为开发人员必须知道的两个简单技术。

非常感谢你阅读这篇文章。我希望它对你有用！

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****