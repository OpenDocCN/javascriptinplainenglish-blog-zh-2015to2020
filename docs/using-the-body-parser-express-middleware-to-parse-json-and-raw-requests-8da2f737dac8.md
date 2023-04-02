# 使用 body-parser Express 中间件来解析 JSON 和原始请求

> 原文：<https://javascript.plainenglish.io/using-the-body-parser-express-middleware-to-parse-json-and-raw-requests-8da2f737dac8?source=collection_archive---------5----------------------->

![](img/001b77352e450cbbca59fdf20edab100.png)

Photo by [Fabian Burghardt](https://unsplash.com/@fabuchao?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

默认情况下，Express 4.x 或更高版本没有提供任何解析请求体的功能。因此，我们需要添加一些东西来做到这一点。

在本文中，我们将看看如何使用`body-parser`中间件来处理 JSON 和原始体。

# 添加主体解析器包

`body-parser`是一个节点包，我们可以将它添加到我们的 Express 应用程序中来解析请求体。

它不支持多部分主体，所以我们必须使用其他中间件包来解析它们。

但是，它可以解析 JSON 主体、原始请求主体、文本和 URL 编码的主体。

为了安装这个包，我们运行:

```
npm install body-parser
```

然后，我们可以添加以下内容:

```
const bodyParser = require('body-parser');
```

# 解析 JSON 主体

我们可以通过调用 JSON 方法来解析 JSON 主体。它接受一个带有一些属性的可选对象。

这些选项包括:

*   `inflate` —当设置为`true`时，压缩的请求体将膨胀。否则，他们会被拒绝。
*   `limit` —控制最大请求正文大小。如果是数字，那就用字节来衡量。如果它是一个字符串，那么它可以被解析成多个字节。
*   `reviver` —这是作为第二个参数传递给`JSON.parse`的函数，用于将值映射到我们想要的值
*   `strict` —仅在设置为`true`时接受对象和数组。否则，它会接受`JSON.parse`接受的任何东西。默认为`true`。
*   `type` —这用于确定它将解析什么媒体类型。它可以是字符串、字符串数组或函数。如果不是函数，那么就直接传入`type-is`库。否则，如果调用函数的数据类型返回真值，则解析请求
*   `verify` —这是一个带有签名`(req, res, buf, encoding)`的函数，其中`buf`是原始请求体的缓冲对象。解析可以通过在函数中抛出一个错误来中止。

例如，我们可以如下使用它:

```
const express = require('express');
const bodyParser = require('body-parser');
const app = express();
const options = {
  inflate: true,
  limit: 1000,
  reviver: (key, value) => {
    if (key === 'age') {
      if (value < 50) {
        return 'young'
      }
      else {
        return 'old';
      }
    }
    else {
      return value;
    }
  }
};
app.use(bodyParser.json(options));app.post('/', (req, res) => {
  res.send(req.body);
});app.listen(3000);
```

然后，当我们向`/`发出 POST 请求时，我们得到:

```
{
    "name": "foo",
    "age": "young"
}
```

作为响应，因为我们检查了 reviver 函数中的`age`字段，并根据`value`返回`'young'`或`'old'`。否则，我们就把`value`原样归还。

请求体被解析并设置为`req.body`的值。

![](img/49ab7fb51cfaac8a191ed096e82d181a.png)

Photo by [Laura College](https://unsplash.com/@laura_college?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 解析原始几何体

我们可以将原始体解析为缓冲区。支持`gzip`和`deflate`编码的自动膨胀。

包含解析数据的解析后的`body`被填充到`request`对象上，即它将被设置为`req.body`的值。

它采用一个可选的`option`对象，该对象具有以下属性:

*   `inflate` —当设置为`true`时，压缩的请求体将膨胀。否则，他们会被拒绝。
*   `limit` —控制最大请求正文大小。如果是数字，那就用字节来衡量。如果它是一个字符串，那么它可以被解析成多个字节。
*   `type` —这用于确定它将解析什么媒体类型。它可以是字符串、字符串数组或函数。如果不是函数，那么就直接传入`type-is`库。否则，如果调用函数的数据类型返回真值，则解析请求
*   `verify` —这是一个带有签名`(req, res, buf, encoding)`的函数，其中`buf`是原始请求体的缓冲对象。解析可以通过在函数中抛出一个错误来中止。

例如，我们可以如下使用它:

```
const express = require('express');
const bodyParser = require('body-parser');
const app = express();
const options = {
  inflate: true,
  limit: 1000,
  type: 'text/plain'
};
app.use(bodyParser.raw(options));app.post('/', (req, res) => {
  res.send(req.body);
});app.listen(3000);
```

然后，当我们用主体`foo`向`/`发送一个 POST 请求时，我们得到回复`foo`。

这是因为我们指定了`text/plain`作为解析原始数据的类型。

它还足够智能，可以解析多种身体类型。我们可以通过传入一组`type`字符串来实现，如下所示:

```
const express = require('express');
const bodyParser = require('body-parser');
const app = express();
const options = {
  inflate: true,
  limit: 1000,
  type: ['text/plain', 'text/html']
};
app.use(bodyParser.raw(options));app.post('/', (req, res) => {
  res.send(req.body);
});app.listen(3000);
```

然后当我们用主体`<b>foo</b>`向`/`发出 POST 请求时。然后我们回到`<b>foo</b>`。

# 结论

我们可以使用`body-parser`中间件来解析 JSON 和原始文本体。

它还采用了各种选项来让我们控制是否膨胀压缩的请求体、将 JSON 值映射到其他内容、限制请求体的大小等等。

对于原始请求体，我们可以使用`body-parser`来指定数据的类型，这样我们就可以将其解析为该类型的对象，并将其设置为`req.body`。