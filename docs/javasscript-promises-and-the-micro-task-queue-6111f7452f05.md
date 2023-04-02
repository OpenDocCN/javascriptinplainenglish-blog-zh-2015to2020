# JavasScript Promises 和微任务队列。

> 原文：<https://javascript.plainenglish.io/javasscript-promises-and-the-micro-task-queue-6111f7452f05?source=collection_archive---------4----------------------->

![](img/c11b6e4d4ac7cf9d9ace252254004647.png)

**异步代码定义**:当你读取代码的顺序与代码执行的顺序不同时。

在 ES6 的世界中，我在这里[解释的异步模型发生了什么变化](https://medium.com/@camfeg/asynchronous-javascript-explained-a4c1133f5544)，我建议在阅读这篇文章之前阅读一下。

ES6 带来了**承诺**。

**的前提承诺**给出如下代码

```
function printHello(){ console.log("Hello") }
function blockFor1Sec(){
    // Blocks the JavasCript thread for 1 second
    // Imagine a for loop that goes to a million
    for(i=0; i < 15000; i++){
        console.log("i is: ", i)
    }
}
​
setTimeout(printHello, 0);
blockFor1Sec();
console.log("Me First")
```

这是:

在浏览器中，仅调用`setTimeout` **会产生后果。因为它是一个**外观**功能。**

事实上，它的后果是在浏览器中，这意味着我们真的没有任何办法在 JavaScript 中跟踪它。

我们无法在内存中看到的数据状态和浏览器中发生的事情之间保持一致，没有办法将浏览器中发生的事情映射到 JavaScript 中发生的事情。

在开发可扩展的应用程序时，这并不理想。

所以 **Promises** 最有价值的特性是希望/能够说，当你在后台(后台是浏览器)触发某个东西时，不要只是把它扔在那里，还要让它在 JavaScript 内存中产生某种后果。

在 ES6 中，我们引入了新的“双管齐下”的**门面功能** `fetch`。“双管齐下”的意思是做两件事。

*   从浏览器触发网络请求。
*   在 Javascript 领域，它将返回一种特殊的对象，称为 **Promise 对象**。

当后台工作完成时，它将用来自网络请求的数据填充并更新那个**承诺对象**。

那个 **Promise 对象**将允许我们在 Javascript 中，在我们的本地或全局内存中，跟踪我们在网络浏览器中触发的东西的状态。

让我们看一个例子，并讨论一下发生了什么:

```
function display(data){
    console.log(data)
}
const futureData = fetch('https://twitter.com/will/tweets/1')
futureData.then(display);
​
console.log("Me first !");
```

*   第 1 行:定义一个函数`display`
*   第 4 行:运行 fetch，将**承诺对象**存储在`const futureData`中

fetch 立即返回一个 **Promise 对象**，它有两个属性。

```
{ value: nil, onFulfilled: [] }
```

`fetch`还在 web 浏览器中触发了一个**网络请求**，我们将在这里设置一个 XHR，它代表 XML，HTTP，request。XML 是我们通过互联网发送数据的数据格式，HTTP 是我们如何在浏览器和服务器之间发送消息的一套规则。

让我们更深入地看看运行`fetch`时会发生什么

网络请求是否已完成？布尔型。

*   0 毫秒:完成？是`false`
*   0 ms:我们向 twitter 发送一个请求，我们知道这一点，因为我们传入了 URL:“[https://twitter.com/example/tweets/1](https://twitter.com/will/tweets/1)”。它的域名是:`twitter.com`，我们告诉它通过设置路由`example/tweets/1`来获取“示例”的第一条 tweet

与 Twitter 的 API 通信需要时间，所以我们不知道何时会有响应。

*   季克
*   肾结核
*   季克
*   肾结核
*   当 Twitter 发回响应对象时，完成？变成了`true`，并用来自 Twitter 的响应对象更新了**承诺对象**的`value`属性。
*   270 ms:我们接收从 Twitter 返回的数据，并存储在 futureData 中，这将触发我们的`display`函数。

## 我们向 Twitter 请求数据来使用它，因此我们必须告诉 JavaScript 如何处理这些数据。当数据从 Twitter 返回时，我们希望自动运行一些代码来使用这些数据，这随时都可能发生。

## 这就是承诺对象的`onFulfilled` 属性发挥作用的地方。

属性负责在数据返回时保持运行的功能。当`value`属性更新时，该数组中的任何函数都将被触发运行。不仅如此，Twitter 的返回值将作为参数传入，以填充存储在`onFulfilled`数组中的任何函数的参数。

所以我们需要做的是让我们的`display`函数在那个数组中。但是我们不能使用像`push`那样向数组添加元素的常用工具，因为`onFulfilled`属性是 **Promise 对象**上的`**hidden**`属性。

为此我们使用`.then`。

## 当我们的 Promise 对象上的`value`属性被来自我们的网络请求的响应数据填充时，我们传递给`.then`的回调不仅仅是运行。

## 为了解释发生了什么，我们必须引入两个队列，回调队列和微任务队列，它们由 JavaScript 的事件循环管理，负责对代码执行的顺序进行排序。

让我们用下面的代码来说明事件的顺序。

```
function display(data){ console.log(data) }
function printHello(){ console.log ("Hello!") }
function blockFor300ms(){ 
    for(i=0; i < 25000 ; i++){
        console.log("i is: ", i)
    }
}
setTimeout(printHello, 0);
​
const futureData = fetch("https://twitter.com/example/tweets/1")
futureData.then(display)
​
blockFor300ms()
console.log("Me first !")
```

*   0 ms: `setTimeout`被调用，它向浏览器传递一个回调函数，这里是`printHello`，并触发一个定时器，定时器到期时，将把`printHello`推送到**回调队列**。由于这里传递给`setTimeout`的定时器持续时间是 0 ms，`printHello`被立即推送到**回调队列**，但是还没有执行，因为我们知道，所有同步代码必须在 JavaScript 的事件循环将代码从**回调队列**推送到**调用栈**之前执行。
*   1 ms:我们的**双叉门面函数，** `**fetch**`在`const futureData`中存储一个**承诺对象**，并触发 Twitter 的 API 的 XHR。
*   2 ms:一个回调，这里`display`被添加到我们`futureData` **承诺对象**的`value`键中
*   3 ms: `blockFor300ms`被调用，导致 Javascript 线程被阻塞 300 ms。
*   季克
*   除草醚
*   季克
*   除草醚
*   270 ms:我们的网络请求返回了数据，该数据立即存储在我们的`const futureData` **承诺对象**的`value`键中。
*   300 ms:我们的`blockFor300ms`函数已经完成了对 Javascript 线程的阻塞。
*   301 毫秒:我们执行`console.log("Me first !")`。

## 现在出现了一个问题，先执行什么？我们的回调传递给了`setTimeout`还是传递给了我们`futureData`承诺对象的`.value`键？

接下来是**微任务队列**，它是在承诺对象的`.onFulfilled`数组中找到的回调队列。

因此，负责从**回调队列**向调用堆栈添加回调的事件循环，也负责添加在**微任务队列**中找到的回调。

所以决定哪些回调首先被允许返回 JavaScript 的是**事件循环**检查我们两个队列的顺序。

.

.

.

**事件循环**首先检查**微任务队列**，然后检查 C **allback 队列**，因此 M **微任务队列**中的回调将总是在**回调队列**中的回调之前执行。

回到我们的例子:

*   302 毫秒:我们执行`display`
*   303 毫秒:我们执行`printHello`

博客灵感来源于[前端大师](https://frontendmasters.com/) / [前端大师](https://medium.com/u/1b199ed2dfd?source=post_page-----6111f7452f05--------------------------------)，[教授的 JavaScript:难的部分 v2 将会提交一份清单](https://medium.com/u/c211a09475?source=post_page-----6111f7452f05--------------------------------)，这篇文章就是由此而来。对于那些想大幅增加他们的前端编程知识的人来说，这是一个惊人的资源。

感谢阅读！

爱，光，代码❤️

## 用简单英语写的 JavaScript 的注释

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****