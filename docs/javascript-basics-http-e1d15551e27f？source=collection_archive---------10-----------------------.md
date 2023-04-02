# JavaScript 基础知识— HTTP

> 原文：<https://javascript.plainenglish.io/javascript-basics-http-e1d15551e27f?source=collection_archive---------10----------------------->

![](img/9bc45b7de2afc707afbda48ed50993bd.png)

Photo by [Grant Durr](https://unsplash.com/@blizzard88?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是世界上最流行的编程语言之一。为了有效地使用它，我们必须了解它的基本知识。

在本文中，我们将了解 JavaScript 应用程序如何通过 HTTP 与外界通信。

# 超文本传送协议

HTTP 代表超文本传输协议。

这是在万维网上交流的规则。

该协议规定计算机通过 TCP 连接在端口 80 上通信。

例如，当我们下载网页时，我们可能会看到以下信息:

```
GET /hello.html HTTP/1.1 
Host: example.com 
User-Agent: Your browser's name
```

我们发出 GET 请求来下载`hello.html`网页。

然后我们从服务器得到一个响应:

```
HTTP/1.1 200 OK 
Content-Length: 65525 
Content-Type: text/html 
Last-Modified: Mon, 08 Jan 2020 10:29:45 GMT
```

这些是响应的元数据和头。

响应主体将是网页本身。

GET 是请求的方法。这意味着我们想要获得指定的资源。

其他常用方法包括删除资源。

PUT 是创建或替换资源。

发帖向其发送信息。

服务器没有义务承载我们收到的每一个请求。

方法名后面的部分是应用请求的资源的路径。

它可能是服务器上的文件，也可能是其他文件/

资源可以是像文件一样传输的任何东西。

`HTTP/1.1`是正在使用的 HTTP 协议的版本。

许多网站使用 HTTP 版本 2，但它要复杂得多，所以可以更快。

当与给定的服务器对话时，浏览器切换到适当的协议版本。

无论使用哪个版本，结果都是一样的。

响应也以一个版本开始。`200 OK`表示请求成功。

其他代码包括 404，表示找不到资源。

以 5 开头的代码表示服务器上发生了错误。

第一行后面可能会有一些标题。

`name: value`是头的键和值，是关于请求或响应的额外信息。

上例中的响应头是:

```
Content-Length: 65525 
Content-Type: text/html 
Last-Modified: Mon, 08 Jan 2020 10:29:45 GMT
```

它告诉我们响应有 65525 字节大，内容类型是 HTML。

还会列出上次修改日期。

它们可以包含在请求或响应中，这取决于客户端和服务器。

请求体可以通过 PUT 和 POST 请求发送，但不能通过 GET 或 DELETE 请求发送。

# 浏览器和 HTTP

当我们在地址栏输入 URL 时，浏览器会发出请求。

一个网站可能包括许多资源。为了快速获取这些数据，浏览器会同时发出几个 GET 请求，而不是一次等待一个。

它们可能包括表单，并且它们允许用户填写信息并将信息发送给服务器。

例如，我们可以写:

```
<form method="GET" action="example/message.html">
  <p>Name: <input type="text" name="name"></p>
  <p><button type="submit">Send</button></p>
</form>
```

我们有一个带有动作的表单元素，动作带有发送请求的路径。

方法是用于发出请求的请求方法。

当我们点击发送按钮时，它就被提交了。

然后，当我们发出请求时，我们得到一个查询字符串作为 URL。

我们会得到这样的结果:

```
example/message.html?name=foo
```

当我们提出请求时。

URL 经过编码，因此可以随请求一起发送。

JavaScript 提供了`encodeURIComponent`和`decodeURIComponent`来分别将 URL 编码成适当的格式并解码。

例如，如果我们写:

```
encodeURIComponent("foo?")
```

我们得到:

```
"foo%3F"
```

如果我们写:

```
decodeURIComponent("foo%3F")
```

然后我们得到:

```
"foo?"
```

GET 请求没有副作用，只要求信息。

应该使用 POST 或 PUT 等其他方法来请求服务器上的更改。

一个浏览器知道他们不应该一直发出 POST 请求，但是他们会发出 GET 请求来获取资源。

![](img/ad497b1427f9688f746ff0dd2831405a.png)

Photo by [Sandy Millar](https://unsplash.com/@sandym10?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

浏览器应用程序通过 HTTP 进行通信。

HTTP 请求包含方法、头，可能还有主体。

HTTP 请求-响应包含头部、主体和响应代码。

有几种方法可以提出请求。它们包括获取、发布、上传和删除。

我们可以添加一个表单，通过提供一个动作来发出请求。