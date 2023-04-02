# 在 Express.js 中发送邮件的指南

> 原文：<https://javascript.plainenglish.io/guide-to-the-express-response-object-sending-things-2defae78d53c?source=collection_archive---------2----------------------->

![](img/00e23f11d88cfa82ca19801b556d30fd.png)

Photo by [Priscilla Du Preez](https://unsplash.com/@priscilladupreez?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Express response 对象允许我们向客户端发送响应。

可以发送各种响应，比如字符串、JSON 和文件。此外，除了主体之外，我们还可以向客户端发送各种标头和状态代码。

在本文中，我们将查看响应对象的各种属性，包括`send`、`sendFile`和`sendStatus`。

# 方法

## RES . send([正文])

我们可以使用`res.send`方法发送一个 HTTP 响应。

`body`参数可以是一个`Buffer`对象、字符串、普通对象或数组。

例如，我们可以通过编写以下内容来发送二进制响应:

```
const express = require('express')
const app = express()
app.use(express.json())
app.use(express.urlencoded({ extended: true }))app.get('/', (req, res, next) => {
  res.send(Buffer.from('foo'));
})
app.listen(3000);
```

要发送 JSON，我们可以传入一个对象:

```
const express = require('express')
const app = express()
app.use(express.json())
app.use(express.urlencoded({ extended: true }))app.get('/', (req, res, next) => {
  res.send({ foo: 'bar' });
})
app.listen(3000);
```

我们也可以通过传入一个字符串来发送 HTML:

```
const express = require('express')
const app = express()
app.use(express.json())
app.use(express.urlencoded({ extended: true }))app.get('/', (req, res, next) => {
  res.send('<p>foo</p>');
})
app.listen(3000);
```

除了`send`方法之外，我们还可以发送响应代码:

```
const express = require('express')
const app = express()
app.use(express.json())
app.use(express.urlencoded({ extended: true }))app.get('/', (req, res, next) => {
  res.status(404).send('not found');
})app.listen(3000);
```

我们可以在发送响应之前设置响应头:

```
const express = require('express')
const app = express()
app.use(express.json())
app.use(express.urlencoded({ extended: true }))app.get('/', (req, res, next) => {
  res.set('Content-Type', 'text/html');
  res.send(Buffer.from('<p>foo</p>'))
})
app.listen(3000);
```

`send`也适用于数组:

```
const express = require('express')
const app = express()
app.use(express.json())
app.use(express.urlencoded({ extended: true }))app.get('/', (req, res, next) => {
  res.send([1, 2, 3]);
})app.listen(3000);
```

然后我们得到`[1,2,3]`。

## res.sendFile(路径[，选项] [，fn])

我们可以使用`sendFile`来发送一个带有在`options`对象中指定的各种选项的文件。

`path`需要一个绝对路径才能使`sendFile`工作。

例如，我们可以如下使用它:

```
const express = require('express');
const path = require('path');
const app = express()
app.use(express.json())
app.use(express.urlencoded({ extended: true }))app.get('/', (req, res, next) => {
  const options = {
    root: path.join(__dirname, 'files'),
    dotfiles: 'deny',
    headers: {
      'x-timestamp': Date.now(),
      'x-sent': true
    }
  }
  res.sendFile('foo.txt', options);
})
app.listen(3000);
```

上面的代码在`options`中设置了根路径，这样我们就不必在每次调用`sendFile`时都构建绝对路径。

以下属性可以添加到`options`对象中:

*   `maxAge` —设置`Cache-Control`标题的`max-age`属性，单位为毫秒或 ms 格式的字符串
*   `root` —相对文件名的根目录
*   `lastModified` —将`Last-Modified`头设置为文件在操作系统上的最后修改日期。设置`false`将其禁用。
*   `headers` —包含与文件一起提供的 HTTP 响应头的对象。
*   `dotfiles` —提供点文件的选项。可能的值有`allow`、`deny`或`ignore`。
*   `acceptRanges` —启用或禁用接受范围请求
*   `cacheControl` —启用或禁用设置`Cache-Control`响应头
*   `immutable` —启用或禁用`Cache-Control`表头中的`immutable`指令。如果启用，应该指定`maxAge`选项来启用缓存。`immutable`指令将阻止受支持的客户端在`maxAge`选项的生命周期内发出条件请求，以检查文件是否已更改。

我们还可以传入一个回调函数来处理错误，作为最后一个参数:

```
const express = require('express');
const path = require('path');
const app = express()
app.use(express.json())
app.use(express.urlencoded({ extended: true }))app.get('/', (req, res, next) => {
  const options = {
    root: path.join(__dirname, 'files'),
    dotfiles: 'deny',
    headers: {
      'x-timestamp': Date.now(),
      'x-sent': true
    }
  }
  res.sendFile('foo.txt', options, err => next(err));
})
app.listen(3000);
```

![](img/b93428f01a113ed885d4251a30ff9fcf.png)

Photo by [Thiago Thadeu](https://unsplash.com/@thiagothehuman?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 资源发送状态(状态代码)

我们可以用这个方法发送一个 HTTP 响应状态代码。我们只需要传入一个数字。

如果指定了不支持的状态代码，仍将发送该代码，并且将在响应正文中发送其字符串版本。

例如，如果我们有:

```
const express = require('express');
const path = require('path');
const app = express()
app.use(express.json())
app.use(express.urlencoded({ extended: true }))app.get('/', (req, res, next) => {
  res.sendStatus(200)
})
app.listen(3000);
```

然后我们再回来`OK`。

如果我们写下以下内容:

```
const express = require('express');
const path = require('path');
const app = express()
app.use(express.json())
app.use(express.urlencoded({ extended: true }))app.get('/', (req, res, next) => {
  res.sendStatus(900)
})
app.listen(3000);
```

我们得到`900`，因为我们发送了一个不支持的响应代码。

# 结论

使用 Express，我们可以通过各种方式发送响应。最通用的是`send`，它允许我们发送文本和二进制数据。我们可以设置标题来指示我们要发送的内容。

`sendFile`发送带有服务器上文件的绝对文件路径的文件响应。我们可以为发送的文件指定各种选项。

`sendStatus`方法让我们只发送一个 HTTP 响应代码。我们可以用它来发送支持和不支持的响应代码。