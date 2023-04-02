# 15 个速战速决的前端面试问题

> 原文：<https://javascript.plainenglish.io/15-quick-fire-front-end-interview-questions-bb4d83d0817c?source=collection_archive---------0----------------------->

## 前端速射面试问题

![](img/599ae27256bb5a309ad8a227c65b0c6c.png)

Photo by [Camylla Battani](https://unsplash.com/@camylla93?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## HTML 中的 DOM 是什么？

文档对象模型(DOM)是 HTML 文档的编程接口。它表示页面，以便程序可以更改文档结构、样式和内容。DOM 将文档表示为节点和对象。

## `span`和`div`有什么区别？

`span`是一个行内元素

`div`是块元素

**div**应该用于换行文档的部分，而 **spans** 应该用于换行文本、图像等的小部分。

*加分点*，在内联元素中放置块级元素是非法的。

*示例:*

```
<div>Hi<span>I'm the start of the span element **<div>I'm illegal</div>** I'm the end of the span</span> Bye I'm the end of the div</div>
```

## 什么是元标签？

**元标签**是描述页面内容的文本片段；meta 标签不会出现在页面本身，而只会出现在页面的源代码中。

元标签本质上是一些小的内容描述符，帮助告诉搜索引擎网页是关于什么的。

*示例:*

```
<head>
  <meta charset="UTF-8">
  <meta name="description" content="Description search engines use">
  <meta name="keywords" content="Keywords, of, your, page">
  <meta name="author" content="Me">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>
```

## CSS 中的 ID 选择器和类选择器有什么区别？

**id**是唯一的。每个元素只能有一个 ID，HTML 页面只能有一个具有该 ID 的元素

**类**不是唯一的。可以在多个元素上使用同一个类，一个元素可以有多个类。

任何需要应用于页面上多个对象的样式信息都应该使用类来完成。

## 如何将 CSS 规则应用于特定的媒体？

在媒体查询中使用`@media`规则，为不同的媒体类型/设备应用不同的样式。

*举例:*

```
/* Change the background color of the any <div> element to "red" when the browser window is 600px wide or less */
@media only screen and (max-width: 600px) {
  div {
    background-color: red;
  }
}
```

## 在 CSS 中什么是伪类？

CSS 中的伪类用于定义元素的特殊状态。它可以与 CSS 选择器结合使用，根据现有元素的状态为它们添加效果。

*示例:*

```
/* 
   Any <a> element which the user's pointer is hovering will go green 
*/
a:hover {
  color: green;
}/* Selects any <a> that has been visited and makes the text purple*/
a:visited {
  color: purple;
}
```

他们能说出任何一个吗？这是一个相当大的列表[https://developer . Mozilla . org/en-US/docs/Web/CSS/Pseudo-classes](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes)

## 相对、固定、绝对和静态定位的元素有什么区别？

*   **相对**元件相对于其正常位置定位。
*   **固定**元素相对于浏览器窗口定位。
*   **绝对**元素绝对定位到其第一个定位的父元素。
*   **静态**这是默认值，所有元素在文档中出现时都是有序的。

## PUT 和 POST 有什么区别？

**PUT** :用请求负载替换目标资源。可用于更新或创建新资源。

**POST** :对有效负载执行特定于资源的处理。可用于不同的操作，包括创建新资源、上传文件或提交 web 表单。

**奖励积分:**

另一个区别是 PUT 应该是幂等的——将相同的数据多次放到同一个 URL 应该没问题，而多个 POST 可能会创建多个对象或任何 POST 操作。

## 长轮询、Websockets 和服务器发送事件之间有什么区别？

**长轮询**打开一个 HTTP 请求，并保持打开状态，直到收到更新。收到更新后，会立即打开一个新请求，等待下一次更新。

WebSockets 协议允许服务器和客户端之间持续的双向通信。对于这个测试，Primus 用于抽象协议的多个实现。

**服务器发送的事件**依赖于一个长期的 HTTP 连接，在这个连接中，更新被不断地发送到客户端

## **解释 cookies、会话存储和本地存储之间的区别？**

**本地存储**因为它被称为浏览器的本地存储，它可以保存高达 **10MB** ，**会话存储**也是如此，但正如其名称所示，它是基于会话的，在关闭浏览器后会被删除。会话存储最多节省 5mb。

Cookies 是存储在你的浏览器中的非常小的数据，可以存储 4KB，可以通过服务器或浏览器访问

## 什么是 CORS？

**CORS** 代表跨产地资源共享。

CORS 是一种浏览器机制，允许对位于给定域之外的资源进行受控访问。它扩展并增加了同源政策的灵活性

## 什么是承诺？

它们用于处理异步操作。它们可以轻松处理多个异步操作，并提供比回调和事件更好的错误处理。

## 承诺有哪些不同的状态？

承诺有三种状态:

1.  **完成**:与承诺相关的动作成功
2.  **拒绝**:与承诺相关的行动失败
3.  **待定**:承诺仍处于待定状态，即尚未履行或拒绝

## JavaScript 中的提升是什么？

**提升**是一个术语，用来描述将*变量*和*函数*移动到它们的*(全局或函数)*作用域的顶部，在那里我们定义该变量或函数。

## JavaScript 中有哪些 falsy 值？

**假值**是当转换为布尔值时变为**假值**的值。

以下任何一项:

```
'' 
0 
null
undefined
NaN
false
-0
0n // BigInt, when used as a boolean, follows the same rule as a Number
```

# 结论

在面试中，我被问及上述问题的混合体。将编码挑战与一些快速提问结合起来是一种很好的方式，可以看出候选人的潜力。

我认为对于前端开发人员的面试，你需要比上面更多的 JavaScript 问题。下面是一些 JavaScript & TypeScript 问题。

**JavaScript 问题:**

[](https://medium.com/javascript-in-plain-english/24-quick-fire-javascript-interview-questions-a71f78d03f08) [## 24 个快速 JavaScript 面试问题

### JavaScript 快速面试问题

medium.com](https://medium.com/javascript-in-plain-english/24-quick-fire-javascript-interview-questions-a71f78d03f08) 

**打字稿问题:**

[](https://medium.com/javascript-in-plain-english/top-19-frequently-asked-typescript-interview-questions-dac4ff30c017) [## 19 个常见的打字稿面试问题

### 关于 TypeScript 的常见面试问题

medium.com](https://medium.com/javascript-in-plain-english/top-19-frequently-asked-typescript-interview-questions-dac4ff30c017) 

我认为还包括你使用的任何相关框架的问题。希望你喜欢阅读

## **简单英语的 JavaScript**

你知道我们有三份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)找到所有内容的链接！