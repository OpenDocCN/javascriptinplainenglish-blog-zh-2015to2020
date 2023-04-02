# 使用 Morgan Express 中间件轻松登录

> 原文：<https://javascript.plainenglish.io/easy-logging-with-the-morgan-express-middleware-4569182ffda4?source=collection_archive---------1----------------------->

![](img/c74b87f53740cbc65887b2d2328d3abb.png)

Photo by [Andrik Langfield](https://unsplash.com/@andriklangfield?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

日志记录是任何应用程序的重要组成部分。我们想知道正在进行什么活动，并在问题出现时寻找信息来解决问题。

在本文中，我们将了解如何使用`morgan`中间件在 Express 应用程序中添加日志。

# 因素

中间件让我们传入两个参数。第一个是`format`，第二个是`options`物体。

## 格式

`format`是一个字符串。例如，我们可以传入`'tiny'`来显示最少的信息。

此外，我们可以传入一个字符串，其中包含我们想要显示的字段，如 1:

```
morgan(':method :url :status');
```

`format`也可以是如下自定义格式函数:

```
const express = require('express');
const bodyParser = require('body-parser');
const morgan = require('morgan')
const app = express();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));morgan((tokens, req, res) => {
  return [
    tokens.method(req, res),
    tokens.url(req, res),
    tokens.status(req, res),
  ].join(' ')
})app.get('/', (req, res) => {
  res.send('foo');
});app.listen(3000, () => console.log('server started'));
```

我们得到了同样的东西:

```
morgan(':method :url :status');
```

传递给`morgan`函数返回值的函数。

## 选择

`morgan`接受`options`对象中的几个属性。

**立即**

`immediate`记录请求而不是响应。不会记录响应中的数据。

**跳过**

确定何时跳过日志记录的函数。

**流**

写入日志行的输出流，默认为`process.stdout`。

## 预定义格式

**合起来**

标准 Apache 组合日志输出:

```
:remote-addr - :remote-user [:date[clf]] ":method :url HTTP/:http-version" :status :res[content-length] ":referrer" ":user-agent"
```

**常见的**

标准 Apache 通用日志输出:

```
:remote-addr - :remote-user [:date[clf]] ":method :url HTTP/:http-version" :status :res[content-length]
```

**开发**

开发用日志。对于服务器错误代码，`:status`令牌将显示为红色，对于客户端错误代码显示为黄色，对于重定向代码显示为青色，对于其他代码显示为无色。

```
:method :url :status :response-time ms - :res[content-length]
```

**短**

比默认值短，包括响应时间。

```
:remote-addr :remote-user :method :url HTTP/:http-version :status :res[content-length] - :response-time ms
```

**微小的**

最小输出:

```
:method :url :status :res[content-length] - :response-time ms
```

# 代币

我们可以创建新的令牌来记录我们需要的字段。

例如，我们可以写:

```
const express = require('express');
const bodyParser = require('body-parser');
const morgan = require('morgan')
const app = express();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));morgan.token('type', (req, res) => req.headers['content-type'])app.get('/', (req, res) => {
  res.send('foo');
});app.listen(3000);
```

然后`:type`将记录`req.headers[‘content-type’]`。

下面是内置在`morgan`中的令牌:

*   `:date[format]` —采用 UTC 格式的日期。可用的格式有:通用长格式的`clf`(如`10/Oct/2000:13:55:36 +0000`)；ISO 8601 格式的`iso`(如`2000–10–10T13:55:36.000`)；RFC 1123 格式的`web`(如`Tue, 10 Oct 2000 13:55:36 GMT`)
*   `:http-version` —请求的 HTTP 版本
*   `:method` —请求的 HTTP 方式
*   `:referreer` —请求的引用者标题。这将使用标准拼写错误的参考者标题(如果存在)，否则将记录参考者
*   `:remote-addr`——请求的远程地址。这将使用`req.ip`。否则标准`req.connection.remoteAddress`值
*   `:remote-user` —基本授权的用户认证部分
*   `:req[header]` —请求的给定`header`。如果不存在，将记录为`-`
*   `:res[header]` —给定的响应`header`。如果不存在，将记录为`-`
*   `:response-time[digits]` —请求进入`morgan`和写入响应头之间的时间，单位为毫秒。
*   `:status` —响应状态。如果请求/响应周期在响应发送到客户端之前完成，则状态将为空
*   `:url` —请求的网址。如果存在，将使用`req.originalUrl`否则将使用`req.url`。
*   `:user-agent` —请求的`User-Agent`头的内容

![](img/2cc6d37fe8dc625f43c64f953597db17.png)

Photo by [Jake Ingle](https://unsplash.com/@ingle_jake?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# morgan.compile(格式)

该方法将格式字符串编译成`format`函数，供`morgan`使用。

格式字符串是表示单个日志行的字符串，可以使用标记语法。

令牌参照`:token-name`。如果令牌接受参数，我们可以将其传递到`[]`。

# 例子

## 简单示例

`morgan`的简单使用如下:

```
const express = require('express');
const bodyParser = require('body-parser');
const morgan = require('morgan')
const app = express();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));app.use(morgan('combined'));app.get('/', (req, res) => {
  res.send('foo');
});app.listen(3000);
```

然后我们会得到类似这样的结果:

```
::ffff:172.18.0.1 - - [28/Dec/2019:01:04:22 +0000] "GET / HTTP/1.1" 304 - "[https://repl.it/languages/express](https://repl.it/languages/express)" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.88 Safari/537.36"
```

## 将日志写入文件

我们可以通过设置`option`对象的`stream`选项将日志写入文件，并将其传递给第二个参数，如下所示:

```
const express = require('express');
const bodyParser = require('body-parser');
const morgan = require('morgan')
const app = express();
const fs = require('fs')
const path = require('path')
const appLogStream = fs.createWriteStream(path.join(__dirname, 'app.log'), { flags: 'a' })app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));app.use(morgan('combined', { stream: appLogStream}));app.get('/', (req, res) => {
  res.send('foo');
});app.listen(3000);
```

然后我们得到:

```
::ffff:172.18.0.1 - - [28/Dec/2019:01:06:44 +0000] "GET / HTTP/1.1" 304 - "[https://repl.it/languages/express](https://repl.it/languages/express)" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.88 Safari/537.36"
```

在`app.log`中。

## 旋转原木

通过设置对象的`interval`属性，我们可以在原木之间旋转。

例如，我们可以写:

```
const express = require('express');
const bodyParser = require('body-parser');
const morgan = require('morgan')
const app = express();
const fs = require('fs')
const path = require('path')
const accessLogStream = fs.createWriteStream(path.join(__dirname, 'app.log'), { flags: 'a' })app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));app.use(morgan('combined', {
  interval: '7d',
  stream: accessLogStream
}));app.get('/', (req, res) => {
  res.send('foo');
});app.listen(3000);
```

每周轮换原木。

## 自定义日志条目格式

我们可以定义自己的令牌并记录如下:

```
const express = require('express');
const bodyParser = require('body-parser');
const morgan = require('morgan')
const app = express();
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));
app.use(morgan(':id :method :url :date[iso]'))morgan.token('id', (req) => req.id);app.get('/', (req, res) => {
  req.id = 1;
  res.send('foo');
});app.listen(3000);
```

然后，由于我们将`req.id`设置为 1，我们得到:

```
1 GET / 2019-12-28T01:11:07.646Z
```

`1`在第一列中，因为我们在`:method`之前的格式字符串中首先指定了`:id`。

# 结论

`morgan`是一个易于使用的快速应用程序日志。它是一个中间件，我们可以指定我们的日志格式和数据，用令牌登录每个条目。

为了记录我们想要的预置令牌，我们可以使用`token`方法定义自己的令牌。