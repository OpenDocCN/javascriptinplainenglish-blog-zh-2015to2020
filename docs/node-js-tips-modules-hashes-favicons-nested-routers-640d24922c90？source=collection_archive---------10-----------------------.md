# Node.js 提示—模块、散列、图标、嵌套路由器

> 原文：<https://javascript.plainenglish.io/node-js-tips-modules-hashes-favicons-nested-routers-640d24922c90?source=collection_archive---------10----------------------->

![](img/0bc3d895114309a2849d1008e367794a.png)

Photo by [Orijit Chatterjee](https://unsplash.com/@orijit57?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 生成随机 SHA1 哈希以用作 Node.js 中的 ID

为了生成一个随机的 SHA1 散列，我们可以使用`crypto`模块。

例如，我们可以写:

```
const crypto = require('crypto');
const currentDate = (new Date()).getTime().toString();
const random = Math.random().toString();
crypto.createHash('sha1').update(currentDate + random).digest('hex');
```

我们使用`Date`实例的`getTime`方法来获取时间戳。

然后我们创建一个随机数。

接下来，我们通过使用`crypto`模块的`createHash`和`update`方法来创建散列。

然后我们用`digest`把它转换成十六进制。

# 在 Express 中设置自定义收藏夹图标

我们可以通过使用`serve-favicon`包来提供一个自定义的图标。

例如，我们可以写:

```
const favicon = require('serve-favicon');
app.use(favicon(path.join(__dirname,'public','images','favicon.ico')));
```

通过运行以下命令来安装该软件包:

```
npm i serve-favicon
```

然后我们可以通过传递中间件的路径来使用捆绑的中间件。

# 打开默认浏览器并导航到节点应用程序中的特定 URL

我们可以使用`opn`包打开浏览器，转到一个特定的 URL。

要安装它，我们运行:

```
npm install opn
```

然后我们可以写:

```
const opn = require('opn');
opn('http://example.com');
```

用默认浏览器打开 URL。

我们还可以通过编写以下内容来指定浏览器:

```
opn('http://example.com', { app: 'firefox' });
```

然后 URL 将在 Firefox 中打开。

# 带有嵌套路由器的 Express.js

我们可以嵌套路由器快递。

例如，我们可以写:

```
const express = require('express');
const app = express();const userRouter = express.Router();
const itemRouter = express.Router({ mergeParams: true });userRouter.use('/:userId/items', itemRouter);userRouter.route('/')
  .get((req, res) => {
    res.send('hello users');
  });userRouter.route('/:userId')
  .get((req, res) => {
    res.status(200)
  });itemRouter.route('/')
  .get((req, res) => {
    res.send('hello');
  });itemRouter.route('/:itemId')
  .get((req, res) => {
    res.send(`${req.params.itemId} ${req.params.userId}`);
  });app.use('/user', userRouter);app.listen(3000);
```

我们只是将路由器项目传递到我们希望的地方。

因为我们想要从父路由器访问参数，所以`itemRouter`上需要`mergeParams`。

除此之外，我们只需通过编写以下代码将`itemRouter`嵌套在`userRouter`中:

```
userRouter.use('/:userId/items', itemRouter);
```

# 将二进制 NodeJS 缓冲区转换为 JavaScript ArrayBuffer

我们可以通过编写以下代码将节点缓冲区转换为 JavaScript `ArrayBuffer`:

```
const toArrayBuffer = (buffer) => {
  const arrayBuffer = new ArrayBuffer(buffer.length);
  const view = new Uint8Array(arrayBuffer);
  for (let i = 0; i < buffer.length; i++) {
    view[i] = buffer[i];
  }
  return arrayBuffer;
}
```

我们只是将缓冲区的每个 but 放入`Uint8Array`。

我们可以通过书写来转换另一种方式:

```
const toBuffer = (arrayBuffer) => {
  const buffer = Buffer.alloc(arrayBuffer.byteLength);
  const view = new Uint8Array(arrayBuffer);
  for (let i = 0; i < buffer.length; i++) {
    buffer[i] = view[i];
  }
  return buffer;
}
```

我们用`alloc`方法创建了`Buffer`。

然后我们将`arrayByffer`传递给`Uint8Array`构造函数，这样我们就可以遍历它。

然后我们将所有的位分配给`buffer`数组。

# 在节点应用程序中写入文件时创建目录

我们可以使用`fs.mkdir`方法的`recursive`选项在创建文件之前创建一个目录。

例如，我们可以写:

```
fs.mkdir('/foo/bar/file', { recursive: true }, (err) => {
  if (err) {
    console.error(err);
  }
});
```

我们创建路径为`/foo/bar/file`的文件，因为我们已经将`recursive`选项设置为`true`。

此外，我们可以通过编写以下内容来使用 promise 版本:

```
fs.promises.mkdir('/foo/bar/file', { recursive: true }).catch(console.error);
```

# 加载外部 JS 文件并访问本地变量

如果我们创建一个模块，我们可以从另一个文件加载外部 JS 文件。

例如，我们可以写:

`module.js`

```
module.exports = {
  foo(bar){
    //...
  }
}
```

`app.js`

```
const module = require('./module');
module.foo('bar');
```

我们将`foo`函数放入`module.exports`中导出，用`require`导入。

然后我们用`module.foo('bar');`来称呼它。

![](img/192709ee81b15b5d13d7b81f2a45e76d.png)

Photo by [Landon Martin](https://unsplash.com/@lbmartin12?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用加密模块生成一个带有时间戳和随机字符串的散列。

此外，我们可以用循环在节点缓冲区和数组缓冲区之间进行转换。

这个`serve-favicon`包让我们在我们的 Express 应用程序中提供图标。

我们可以创建模块来导出可以从其他函数运行的函数。

快速路由器可以嵌套超过一层。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**