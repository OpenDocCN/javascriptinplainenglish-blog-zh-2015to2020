# Node.js 提示—关闭套接字、编写响应和解析 HTML

> 原文：<https://javascript.plainenglish.io/node-js-tips-closing-sockets-writing-responses-and-parsing-html-c46ecdf8b66b?source=collection_archive---------10----------------------->

![](img/85b8e2a3ce4af773ba1e5a87e2c2028f.png)

Photo by [Tekton](https://unsplash.com/@tekton_tools?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 记录 npm 安装命令的输出

为了记录`npm install`命令的输出，我们可以将 stderr 路由到 stdout。

例如，我们可以写:

```
npm install 2>&1 | tee log.txt
```

`2>&1`将 stderr 路由到 stdout。

`tee`读取标准输出，并将其写入标准输出和一个或多个文件。

我们按照命令中的指定写入`log.txt`。

# 使用 Node.js 和 XPath 解析页面

为了用 XPaths 解析 HTML，我们用`parse5`解析 HTML。

它不把它解析成一个 DOM 对象，但是它很快并且符合 W3C 标准。

我们可以用`xmlserializer`将其序列化为 XHTML，

然后可以使用`xmldom`来获取 DOM。

我们使用`xpath`库来进行 XPath 查询。

例如，我们可以写:

```
const fs = require('mz/fs');
const xpath = require('xpath');
const parse5 = require('parse5');
const xmlSer = require('xmlserializer');
const DOMParser = require('xmldom').DOMParser;(async () => {
  const html = await fs.readFile('./foo.html');
  const document = parse5.parse(html.toString());
  const xhtml = xmlSer.serializeToString(document);
  const doc = new DOMParser().parseFromString(xhtml);
  const select = xpath.useNamespaces({
    x: "http://www.w3.org/1999/xhtml"
  });
  const nodes = select("/html/body/header/div/div[1]/a[2]", doc);
  console.log(nodes);
})();
```

我们导入`fs`库来读取`foo.htmk`文件。

然后我们将带有`parse5`的文档解析成一个`document`对象。

然后我们用`xmlserializer`的`serializeToString`方法将 HTML 转换成 XHTML。

然后，我们通过解析转换后的字符串来创建一个新的`DOMParser`实例。

接下来，我们用`userNamespace`方法创建一个`select`对象，让我们通过 XPath 选择 DOM 元素。

然后我们调用`select`通过 XPath 选择元素。

`doc`是从`DOMParser`返回的 DOM 对象。

`nodes`有返回的节点。

此外，我们可以使用`libxmljs`库通过 XPath 获取节点。

例如，我们可以写:

```
const libxmljs = require("libxmljs");
const xml =  `
<?xml version="1.0" encoding="UTF-8"?>'
<root>
  <child foo="bar">
    <grandchild baz="foo">some content</grandchild>
  </child>
  <sibling>with content!</sibling>
</root>`;const xmlDoc = libxmljs.parseXml(xml);
const gchild = xmlDoc.get('//grandchild');
console.log(gchild.text())
```

我们通过向`parsXml`方法传递一个 XML 字符串来使用`libxmljs`。

然后我们调用传递一个 XPath 字符串给`get`方法来获取项目。

并且`text`将具有所选节点的文本，即`some content`。

# Node.js Webserver 中的结束后写入错误

如果我们使用`net`模块创建一个 web 服务器，那么当我们写一个消息时，我们必须传入一个回调函数作为第二个参数来调用它的`end`。

例如，我们可以写:

```
const netSocket = require('net').Socket();
netSocket.connect(9090);
netSocket.write('hello', err => netSocket.end());
```

我们在第二个参数的回调中调用`netSocket.end()`来在消息被写入后关闭套接字。

我们必须这么做，因为`write`是异步的。不能保证`write`在`end`之前完成，我们不会把它传递到回调中。

大概是这样的:

```
const netSocket = require('net').Socket();
netSocket.connect(9090);
netSocket.write('hello');
netSocket.end();
```

会出现“结束后写入”错误。

# response.setHeader 和 response.writeHead 之间的区别

`response.setHeader`让我们为隐式标题设置一个单独的标题。

如果头已经存在，那么它的值将被替换。

我们可以使用一个字符串数组来发送多个同名的头。

`response.writeHead`发送请求的响应头。

一种方法接受一个带有状态代码和响应头的对象。

要使用`setHeader`，我们可以写:

```
const body = "hello world";
response.setHeader("Content-Length", body.length);
response.setHeader("Content-Type", "text/plain");
response.setHeader("Set-Cookie", "type=foo");
response.status(200);
```

我们用一个`setHeader`调用设置一个响应头。

用`response.status`单独设置状态代码。

另一方面，使用`writeHead`，我们一次性设置响应状态代码和标题。

例如，我们可以写:

```
const body = "hello world";
response.writeHead(200, {
  "Content-Length": body.length,
  "Content-Type": "text/plain",
  "Set-Cookie": "type=foo"
});
```

我们在第一个参数中设置响应状态代码，在第二个参数中设置对象中的所有响应头。

![](img/057885b37ec91b92577ca36c73f6b4f7.png)

Photo by [David Clode](https://unsplash.com/@davidclode?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

`response.setHeader`和`response.writeHead`不一样。

我们可以解析 HTML 或 XML，并使用 XPath 查询节点。

要结束用`net`创建的套接字，我们必须在传入`write`第二个参数的回调中调用它。

我们可以通过重定向输出将`npm install`错误记录到一个文件中。