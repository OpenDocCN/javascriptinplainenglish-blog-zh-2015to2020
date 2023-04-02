# 基本 JavaScript 模式—事件

> 原文：<https://javascript.plainenglish.io/basic-javascript-patterns-events-b1795db01f26?source=collection_archive---------11----------------------->

![](img/3a1e58161cb14da460c0b4cfaee53e10.png)

JavaScript 让我们可以做很多事情。它的语法有时过于宽容。

为了组织我们的代码，我们应该使用一些基本的设计模式。在本文中，我们将研究事件、HTTP 请求和后台任务。

# 事件

处理用户事件对于开发者来说也是一个问题。

我们处理事件的方式有很多不一致的地方。

监听事件必须以干净的方式进行。

我们不应该违反关注点的分离。

例如，我们应该编写如下所示的 HTML:

```
<button id="clickme">click me</button>
```

我们不希望 HTML 标记中有`clickme` 处理程序。

这违反了关注点的分离，当我们以后不得不改变它时会有问题。

相反，我们应该写:

```
const clickMe = document.querySelector('#clickme');
clickMe.onclick = () => {
  //...
}
```

这使得我们未来的生活更加容易，因为所有的点击处理代码都是 JavaScript。

我们必须做一些事情来处理事件。

我们必须获得对事件对象的访问权。

然后我们必须给它附加一个事件监听器函数。

然后在处理程序中，我们必须更新任何我们想要改变的东西。

那我们必须阻止事件的传播。

如果我们不需要事件冒泡，那么我们就不应该让它冒泡。

标准的方式是在事件对象上调用`stopPropagation`。

例如，我们可以写:

```
const clickMe = document.querySelector('#clickme');
clickMe.onclick = (e) => {
  //...
  e.stopPropagation();
}
```

我们可能还需要阻止默认操作的发生。

例如，如果我们通过 JavaScript 提交表单，我们可能希望停止默认的表单提交行为。

我们调用事件对象上的`preventDefault`方法来实现。

# 事件委托

我们可以使用事件委托来减少附加到不同节点的事件处理程序的数量。

例如，我们可以写:

```
document.onclick = (e) => {
  src = e.target || e.srcElement;
  if (src.nodeName.toLowerCase() !== "button") {
    return;
  }
  //...
}
```

上面的代码取`e`对象，即事件对象，检查节点名是否等于`'button'`。

如果不是，我们就结束事件处理函数。

否则，我们继续运行剩余的代码。

这比什么都附加事件处理程序要好得多。

这使得监听动态元素的 tp 事件变得更加容易。

我们只需要监听我们想要附加侦听器的父元素，然后在事件处理程序中运行我们想要的东西。

# 长时间运行的脚本

如果我们有长时间运行的脚本，那么我们必须异步运行它们。

为此，我们可以使用`setTimeout`函数。

它会在指定的时间后运行代码。

我们可以通过书写来使用它:

```
setTimeout(() =>{
  //...
}, 1000)
```

其中 1000 是我们运行回调之前的时间，以毫秒为单位。

回调函数有我们想要运行的代码。

# 网络工作者

对于长期运行的后台任务，我们也可以使用 web workers 来运行它们，

我们可以这样写:

```
const ww = new Worker('webWorker.js');
ww.onmessage = (event) => {
  console.log(event.data);
};
```

通过调用`postMessage`方法来填充`event`对象。

例如，在`webWorker.js`中，我们写道:

```
postMessage('hello there');
```

这个`event.data`将是`'hello there'`。

![](img/64d0484da081c697307cca4f6a5b633c.png)

Photo by [Srdjan Popovic](https://unsplash.com/@mendokusai?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# HTTP 请求

我们可以用 Fetch API 发出 HTTP 请求。

它可以在大多数现代浏览器中使用。API 是基于承诺的，所以我们不会有回调地狱。

例如，我们可以写:

```
(async () => {
  const res = await fetch('https://api.agify.io/?name=michael');
  const data = await  res.json();
  console.log(data);
})()
```

我们在一个异步函数中调用`fetch`,这样我们就可以得到数据。

现在我们不必使用繁琐的`XMLHttpRequest`构造函数或第三方 HTTP 客户端来发出请求，

这让我们的生活变得更加轻松。

# 结论

我们应该总是在 JavaScript 代码中有一个事件处理代码，而不是 HTML。

这样，我们可以将我们的关注点分开，而不用担心以后当我们不得不改变它们时会弄坏它们。

为了使事件处理更容易，我们可以使用事件委托来避免将侦听器附加到所有节点。

它对于动态添加的元素也非常有效。

要运行后台任务，我们可以使用 web workers。

为了发出 HTTP 请求，我们可以使用 Fetch API。

## **简明英语团队的笔记**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**我们还推出了一个 YouTube，希望你能通过 [**订阅我们的英语频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) 来支持我们**

**一如既往,“简明英语”希望帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****