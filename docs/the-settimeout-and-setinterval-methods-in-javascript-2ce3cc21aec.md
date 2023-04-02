# JavaScript 中的 setTimeout 和 setInterval 方法

> 原文：<https://javascript.plainenglish.io/the-settimeout-and-setinterval-methods-in-javascript-2ce3cc21aec?source=collection_archive---------5----------------------->

![](img/08551c291ce9ec475ad4800c75da669a.png)

Photo by [Veri Ivanova](https://unsplash.com/@veri_ivanova?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# setTimeout()方法

`setTimeout()`方法在以毫秒为单位的指定时间后调用函数。该方法将函数作为第一个参数，以毫秒为单位的时间作为第二个参数。您可以在函数体中编写您的逻辑，该函数体将在您指定的时间后执行。

例如:5 秒后显示警告框。

```
setTimeout(()=> {
    alert('Message after 5 seconds');
}, 5000);
```

要取消计时器，您可以首先将计时器存储在一个变量中，并通过将计时器作为参数传递来调用`clearTimeout()`函数。

```
var timer = setTimeout(()=> {
    alert('Message after 5 seconds');
}, 5000);clearTimeout(timer);
```

# setInterval()方法

`setInterval()`方法以毫秒为单位的指定时间间隔调用一个函数。它以函数和时间(毫秒)作为参数。

比如:每秒打印一个随机数。

```
setInterval(() => {
    console.log(Math.random());
}, 1000);
```

要取消间隔，可以先将间隔存储在一个变量中，并通过将间隔作为参数传递来调用`clearInterval()`函数。

```
var interval = setInterval(() => {
    console.log(Math.random());
}, 1000);clearInterval(interval);
```

# 创建一个运行时钟计时器

要创建一个运行的时钟，我们需要借助`setInterval()`方法和`Date`对象。我们可以通过创建一个新的`Date`对象来获得当前时间。

```
new Date().toLocaleTimeString(); // 6:30:49 PM
```

一旦我们创建了一个`Date`对象，它会给我们当前时间的日期信息，它不会随着时间的推移而改变它的值。所以我们每次都需要创建一个新的对象来获取最新的日期和时间。

对于这个例子，我们将每秒创建一个新的`Date`对象并打印出来。

```
var date = setInterval(() => { 
   console.log(new Date().toLocaleTimeString());
}, 1000);
```

要在网页上显示正在运行的时钟计时器，可以在 HTML 中创建一个`div`元素，并用每秒的最新时间设置它的`innerHTML`。

**HTML**

```
<div id="time"> </div>
```

**JavaScript**

```
var date = setInterval(() => {
  const element = document.getElementById('time');
  element.innerHTML = new Date().toLocaleTimeString();
}, 1000);
```

# 你可能也喜欢

*   [JavaScript Array forEach()方法循环遍历数组](https://jscurious.com/javascript-array-foreach-method-to-loop-through-an-array/)
*   [20 种节省您时间的 JavaScript 速记编码技术](https://jscurious.com/20-javascript-shorthand-techniques-that-will-save-your-time/)
*   [JavaScript 中的回调函数](https://jscurious.com/callback-functions-in-javascript/)

*感谢您的宝贵时间* ☺️
更多网络开发博客，请访问[jscurious.com](http://jscurious.com/)