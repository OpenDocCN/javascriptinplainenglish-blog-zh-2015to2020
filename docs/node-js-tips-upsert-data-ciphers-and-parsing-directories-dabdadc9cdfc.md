# Node.js 提示—更新数据、密码和解析目录

> 原文：<https://javascript.plainenglish.io/node-js-tips-upsert-data-ciphers-and-parsing-directories-dabdadc9cdfc?source=collection_archive---------11----------------------->

![](img/7984c261eeb94b5c8ff99f88e885613f.png)

Photo by [Sveta Golovina](https://unsplash.com/@murder666?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# Express 中 response.send 和 response.write 的区别

`res.send`只被调用一次，它将缓冲区大小的数据块发送给客户端。

然后，它返回并向客户端发送更多具有该缓冲区大小的数据，直到发送完成。

`res.write`被多次调用，后跟`res.end`。它创建一个大小基于整个数据的缓冲区。

然后通过 HTTP 发送出去。因此，在数据量巨大的情况下速度更快。

# 如果不存在，则创建文档，否则用 Mongoose 更新现有文档

如果文档不存在，我们可以创建文档，否则用`findOneAndUpdate`方法进行更新。

例如，我们可以写:

```
const query = {};
const update = { expire: new Date() };
const options = { 
  upsert: true
}; Model.findOneAndUpdate(query, update, options, (error, result) => {
  if (error) return;
});
```

我们传入一个属性设置为`true`的`options`对象。

`update`有我们想要添加或更新的数据。

`query`有获取数据的查询。

当操作完成时，回调被调用。

`result`已将数据写入文件。

`error`有错误对象。

# 用 Node.js 将文件系统中的目录结构转换成 JSON

我们可以使用`directory-package`将文件系统的结构读入 JSON。

要安装它，我们运行:

```
npm i directory-tree
```

然后我们可以通过写来使用它:

```
const dirTree = require("directory-tree");
const tree = dirTree("/foo/bar");
```

我们使用`directory-tree`包的`dirTree`功能。

然后我们将路径传递给`dirTree`函数。

我们可以通过编写以下内容来限制要枚举的文件类型:

```
const dirTree = require("directory-tree");
const filteredTree = dirTree("/some/path", { extensions: /\.txt/ });
```

我们使用`extension`正则表达式进行过滤。

# 在 HTTP 和 HTTPS 上监听单个 Express 应用程序

我们可以通过使用`http`和`https`包并在两者上调用`createServer`方法来同时监听 HTTP 和 HTTPS。

例如，我们可以写:

```
const express = require('express');
const https = require('https');
const http = require('http');
const fs = require('fs');
const app = express();const options = {
  key: fs.readFileSync('/path/to/key.pem'),
  cert: fs.readFileSync('/path/to/cert.pem'),
  ca: fs.readFileSync('/path/to/ca.pem')
};http.createServer(app).listen(80);
https.createServer(options, app).listen(443);
```

我们读取证书文件，并用它们调用`createServer`来监听 HTTPS 的请求。

而且我们还像往常一样使用`http`监听常规的 HTTP 请求。

# Node.js 中 process.nextTick 的用例

我们可以使用`process.nextTick`将回调放到队列中。

这和用`setTimeout`延迟代码执行是一样的。

例如，我们可以写:

```
process.nextTick(() => {
  console.log('hello');
});
```

# 加密需要解密的数据

我们可以用`crypto`模块加密需要解密的数据。

它有`createCipher`方法让我们创建一个可以被破译的密码。

例如，我们可以写:

```
const crypto = require('crypto');const algorithm = 'aes256'; 
const key = 'sercetkey';
const text = 'hello world';const cipher = crypto.createCipher(algorithm, key);  
const encrypted = cipher.update(text, 'utf8', 'hex') + cipher.final('hex');const decipher = crypto.createDecipher(algorithm, key);
const decrypted = decipher.update(encrypted, 'hex', 'utf8') + decipher.final('utf8');
```

我们用`algorithm`和`key`调用`createCipher`来创建密码。

然后为了加密`text`，我们调用`cipher.update`并将其与`cipher.final('hex')`连接起来。

`cipher.final`只能调用一次，这样文本只能加密一次。

当我们想要破译文本时，我们用相同的`algorithm`和`key`调用`createDecipher`方法。

然后，我们可以使用`decipher.update`来解密加密的文本，并将其与`decipher.find('hex')`连接来解密文本。

`decipher.final`只能调用一次，这样加密的文本只能解密一次。

![](img/866e591cfd2c42882a9ed59e425523fb.png)

Photo by [Alex Ware](https://unsplash.com/@alexbeware?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用`crypto`模块创建密码和破译文本。

`res.send`和`res.write`表达不同。一个适用于小数据，一个适用于大数据。

我们可以用猫鼬上传数据。

目录结构可以读入 JSON。