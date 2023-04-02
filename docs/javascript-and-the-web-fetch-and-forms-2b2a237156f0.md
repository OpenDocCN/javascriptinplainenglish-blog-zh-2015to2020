# JavaScript 和网络——获取和表单

> 原文：<https://javascript.plainenglish.io/javascript-and-the-web-fetch-and-forms-2b2a237156f0?source=collection_archive---------8----------------------->

![](img/d561a8bcd2ac265fda94ec626d5c9ba1.png)

Photo by [Arno Smit](https://unsplash.com/@_entreprenerd?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是世界上最流行的编程语言之一。为了有效地使用它，我们必须了解它的基本知识。

在本文中，我们将了解如何使用 JavaScript Fetch API 来发出 HTTP 请求和创建表单。

# 取得

Fetch API 是 JavaScript 标准 API 的一部分，允许我们从浏览器 web 应用程序发出 HTTP 请求。

它由`fetch`函数和相关对象组成，如`Response`和`Header`构造函数。

例如，我们可以通过编写以下内容来发出 GET 请求:

```
(async () => {
  const res = await fetch('https://api.agify.io/?name=michael');
  const data = await res.json();
  console.log(data)
})();
```

我们创建了一个`async`函数，并将一个 URL 传递给了`fetch`函数。

它返回一个带有数据的承诺，我们用`json`方法解析它。

然后我们得到最终数据。

所有的函数都返回承诺。

要改变方法，我们可以用`method`选项添加一个对象。

例如，我们可以写:

```
(async () => {
  const res = await fetch('https://api.agify.io/?name=michael', {
    method: "DELETE"
  });
  const data = await res.json();
  console.log(data)
})();
```

要添加正文，我们可以写:

```
(async () => {
  const res = await fetch('https://www.w3.org/TR/PNG/iso_8859-1.txt', {
    headers: {
      'Content-Type': 'text/plain'
    }
  });
  const data = await res.text();
  console.log(data)
})();
```

我们用`headers`选项设置请求的`Content-Type`头。

浏览器会自动添加一些头，如 just 和服务器计算正文大小所需的头。

# HTTP 沙箱

浏览器通过禁止脚本向其他域发出 HTTP 请求来保护我们。

这样，恶意网站就不能使用我们的计算机向他们发出请求，做他们想做的事情。

我们可以在响应体中包含一个头，以允许来自所有域的请求。

我们可以加上:

```
Access-Control-Allow-Origin: *
```

以允许来自所有来源的请求发送到服务器。

# HTTP 的好处

运行在客户端和服务器端应用程序上的 JavaScript 程序需要通信，

我们可以通过远程过程调用做到这一点。它们是用 HTTP 制作的。

它是一种交流的工具。

交流是围绕资源和 HTTP 方法的概念进行的。

例如，我们向`/users/1`发出一个 PUT 请求，对 ID 为 1 的用户进行编辑。

此外，HTTP 还提供了一些特性，比如在客户机上缓存和保存响应的副本，以便更快地访问。

# 安全性

为了保护 HTTP 协议，安全 HTTP 协议用于安全通信。

通信信道用证书加密。

这确保了外界的人无法窥探安全通道。

此外，由于信道被加密，防止了篡改。

# 表单字段

表单是为 JavaScript 之前的 web 设计的，允许网站在 HTTP 请求中发送用户提交的信息。

这允许通过离开网页来与服务器进行交互。

元素是 DOM 的一部分。这意味着可以检查这些元素。

这使得使用 JavaScript 程序检查、控制和输入字段，以及使用客户端 JavaScript 向表单添加新功能成为可能。

表单由任意数量的输入字段组成，这些字段由表单标签分组。

HTML 允许不同样式的字段，如复选框、下拉菜单和文本输入。

许多字段使用输入标签。

它有一个`type`属性，可以有以下值:

*   `text` —单行文本输入
*   `password` —与`text`相同，但隐藏了键入的内容
*   `checkbox` —开/关开关
*   `radio` —多选字段
*   `file` —允许用户选择文件的控件

例如，我们可以写:

```
<input type="text" value="foo">
```

我们将`type`设置为`text`，将`value`设置为`foo`。该值是预先填充的。

单选按钮可以按如下方式编写:

```
<input type="radio" value="apple" name="choice">
<input type="radio" value="orange" name="choice" checked>
```

`type`是`radio`并且`name`在组之间必须相同。

`checked`让我们设置单选按钮的选中值。

文件输入可以这样写:

```
<input type="file">
```

为了制作下拉菜单，我们添加了一个选择标记:

```
<select>
  <option>apple</option>
  <option>orange</option>
  <option>banana</option>
</select>
```

`option`元素是下拉列表的选项。

![](img/8271b5070820088b03301f5abdb9565e.png)

Photo by [Han Chenxu](https://unsplash.com/@hanchenxu?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

要发出 HTTP 请求，我们可以使用 Fetch API。

为了保护我们通过 HTTP 的通信，我们可以通过添加证书来使用 HTTPS 协议。

可以使用 form、input 和 select 元素创建表单。

## **说白了**

你知道我们有四种出版物吗？通过[**plain English . io**](https://plainenglish.io/)找到他们——关注我们的出版物和 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**