# 使用 serve-static 在 Express 应用程序中提供静态文件

> 原文：<https://javascript.plainenglish.io/serving-static-files-in-express-apps-with-serve-static-f42aaa2d6c2b?source=collection_archive---------3----------------------->

![](img/91cc7ce0b9f2ccd9738990e7e7b9c6c4.png)

Photo by [Daan Stevens](https://unsplash.com/@daanstevens?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

要使用 Express 应用程序提供静态文件，我们可以使用`serve-static`包来提供服务器上给定文件夹中的文件。

在本文中，我们将看看如何用它来服务静态文件。

# 装置

`serve-static`库以 Node.js 包的形式提供。要安装它，我们可以运行:

```
npm install serve-static
```

# 配置

`server-static`包为我们提供了一个函数，可以创建一个新的中间件函数来服务给定目录中的文件。

将通过结合`req.url`和提供的根目录来确定要服务的文件。当找不到文件时，它将调用`next()`移动到下一个中间件，而不是用 404 错误来响应。

这允许调用堆栈和回退代码。

我们可以通过编写以下内容来导入模块:

```
const serveStatic = require('serve-static');
```

来自`serve-static`模块的`serveStatic`函数有两个参数。

它有一个`root`和`options`参数。

`root`是一个字符串，包含要服务的根文件夹的路径。

`options`参数接受具有以下属性的对象:

*   `acceptRanges` —启用或禁用接受范围请求。默认值为`true`。禁用此项不会发送`Accept_ranges`并忽略`Range`请求报头的内容
*   `cacheControl` —启用或禁用设置`Cache-Control`响应报头。默认值为`true`。禁用此选项将忽略`immutable`和`maxAge`选项
*   `dotfiles` —让我们设置如何处理隐藏文件，即任何以点开头的文件。如果指定了`root`，只检查根目录以上的点文件。其他可能的值包括`allow`，表示对点文件没有特殊处理，`deny` —拒绝对点文件的请求(403 响应)，`ignore` —假装它们不存在(404 响应)
*   `etag` —启用或禁用 ETag 生成。默认为`true`
*   `extensions` —设置文件扩展名回退。如果这是`true`当没有找到扩展名时，给定的扩展名将被添加到文件名中并被搜索。默认为`false`。
*   `fallthrough` —设置中间件，使客户端错误作为未处理的请求通过，否则转发客户端错误。当这是`true`时，客户端错误将导致这个中间件调用`next`以移动到下一个中间件。设置为`false`将使 404s 短路。
*   `immutable` —启用或禁用`Cache-Control`响应头的`immutable`指令。默认值为`false`。当设置为`true`时，还应指定`maxAge`来启用缓存
*   `index` —默认情况下`server-static`将发送`index.html`文件以响应目录上的请求。将此设置为`false`将防止这种情况发生
*   `lastModified` —启用或禁用`Last-Modified`割台。默认为`true`。该值将是文件系统中最后修改的值
*   `maxAge` —设置缓存的最大时间(毫秒)。默认值为 0。
*   `redirect` —当路径名是目录时，重定向到结尾的`/`。默认为`trie`
*   `setHeaders` —设置响应的自定义标题。它的值是一个带有签名`fn(res, path, stat)`的函数。`res`是响应对象，`path`是发送的文件路径，`stat`是发送的文件的 stat 对象。

# 例子

## 简单用法

我们可以将一个带有`foo.txt`的文件夹作为默认文件显示如下:

```
const express = require('express');
const bodyParser = require('body-parser');
const serveStatic = require('serve-static');
const app = express();
app.use(serveStatic('public/files', { 'index': ['foo.txt'] }))app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));app.listen(3000);
```

然后，当我们向`/`发出 GET 请求时，如果`foo.txt`有内容`foo`，我们就会得到显示为响应的`foo`。

![](img/3306a5db8b254c2474b0dbbdbbe574fd.png)

Photo by [Erol Ahmed](https://unsplash.com/@erol?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 撤退

我们可以为多个目录提供服务，后续的目录作为第一个目录的后备，如果 like 出现错误，当它没有被找到时:

```
const express = require('express');
const bodyParser = require('body-parser');
const serveStatic = require('serve-static');
const path = require('path');
const app = express();
app.use(serveStatic(path.join(__dirname, 'public/files'), { 'index': ['foo'] }));
app.use(serveStatic(path.join(__dirname, 'public/ftp'), { 'index': ['bar.txt'] }));app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));app.listen(3000);
```

然后当文件`public/files/foo`没有被找到时，那么`public/ftp/bar.txt`将被提供。

## 设置标题

我们可以如下设置标题:

```
const express = require('express');
const bodyParser = require('body-parser');
const serveStatic = require('serve-static');
const path = require('path');
const app = express();
const setCustomCacheControl = (res, path) => {
  if (serveStatic.mime.lookup(path) === 'text/html') {
    res.setHeader('Cache-Control', 'public, max-age=0')
  }
}app.use(serveStatic(path.join(__dirname, 'public/files'), {
  setHeaders: setCustomCacheControl
}));app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));app.listen(3000);
```

然后当我们的文件有了`text/html` MIME-type，像默认的索引文件`index.html`，那么我们将得到`Cache-Control`响应头的值为`public, max-age=0`。

# 结论

有了`serve-static`包，我们可以轻松地提供静态文件。它有各种选项来控制缓存、默认文件服务、多个后备文件夹等。

此外，我们可以选择是否提供隐藏文件，以及我们希望在响应和重定向中使用的标题。