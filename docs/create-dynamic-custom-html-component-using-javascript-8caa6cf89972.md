# 使用 JavaScript 创建定制组件

> 原文：<https://javascript.plainenglish.io/create-dynamic-custom-html-component-using-javascript-8caa6cf89972?source=collection_archive---------13----------------------->

## 使用 JavaScript 创建动态 HTML 定制组件

[TechnoFunnel](https://medium.com/technofunnel) 提供了另一篇不使用任何前端框架创建**自定义动态 HTML 组件** **的文章。在本文中，我们将创建一个定制的 HTML 组件，并动态更新组件以显示最新时间。在前面的文章中，我们已经创建了一个定制组件。**

这将是一系列的文章，每一篇文章都向创建的定制组件添加更多的细节和功能。

1.  [创建自定义称呼元素以显示 Hello World](https://medium.com/technofunnel/create-custom-html-element-without-any-frontend-framework-html5-6e78ada50162)
2.  [使用属性将数据传递给自定义元素](https://medium.com/technofunnel/creating-passing-data-to-html-custom-elements-using-attributes-bfd9aa759fd4)
3.  [为定时器功能创建动态元素](https://medium.com/technofunnel/create-dynamic-custom-html-component-using-javascript-8caa6cf89972)

![](img/5319c4c16767ecb38f08acbdd54db3e1.png)

让我们从创建自定义元素开始，它显示组件呈现的时间，然后进一步更新它以保持每秒更新一次。让我们创建一个组件来显示当前时间:

[https://gist.github.com/Mayankgupta688/56b7d21ed9fb77354609269c391d3ae5](https://gist.github.com/Mayankgupta688/56b7d21ed9fb77354609269c391d3ae5)

在上面的代码中，我们创建了一个小元素，当组件在浏览器中呈现时，它将显示一个静态时间值，因为函数“connectedCallback”是在自定义组件的初始呈现时触发的。接下来，让我们编写代码在浏览器中显示计时器。为了呈现这个自定义组件，我们将在 HTML 页面中呈现“当前时间”组件。

[https://gist.github.com/Mayankgupta688/e29ae8d995be65544d1dead65822d8f1](https://gist.github.com/Mayankgupta688/e29ae8d995be65544d1dead65822d8f1)

一旦我们在 HTML 页面中添加了自定义元素“current-time ”,它将在 HTML 页面上呈现时间组件。下面给出的是相同的工作沙盒代码…

[https://codesandbox.io/s/custom-time-component-e2x5v?file=/index.html](https://codesandbox.io/s/custom-time-component-e2x5v?file=/index.html)

## 将计时器添加到“connectedCallback”以更新组件

为了动态更新组件，我们可以在自定义组件的“ **connectedCallback** ”函数中引入 setInterval。间隔 1000 毫秒后，我们将更新组件的“innerHTML”。

[https://gist.github.com/Mayankgupta688/e9d18755b88fdbbdb21237d666d367d8](https://gist.github.com/Mayankgupta688/e9d18755b88fdbbdb21237d666d367d8)

在上面的代码中，我们引入了一个“setInterval ”,它将每秒用最新的时间更新组件。下面是代表给定场景的代码沙箱。

[https://codesandbox.io/s/dynamic-time-component-vve8p](https://codesandbox.io/s/dynamic-time-component-vve8p)

## 进一步阅读

[](https://bit.cloud/blog/meet-component-driven-content-applicable-composable-l24cw7ku) [## 满足组件驱动的内容:适用的、可组合的

### 自从 React 和 Angular 等技术出现以来，我们经常将术语“组件”与…

比特云](https://bit.cloud/blog/meet-component-driven-content-applicable-composable-l24cw7ku) 

*更多内容请看*[***plain English . io***](https://plainenglish.io/)*。报名参加我们的* [***免费周报***](http://newsletter.plainenglish.io/) *。关注我们关于*[***Twitter***](https://twitter.com/inPlainEngHQ)[***LinkedIn***](https://www.linkedin.com/company/inplainenglish/)*[***YouTube***](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)***，以及****[***不和***](https://discord.gg/GtDtUAvyhW) *对成长黑客感兴趣？检查* [***电路***](https://circuit.ooo/) ***。*****