# 使用压缩中间件压缩 Express.js 响应

> 原文：<https://javascript.plainenglish.io/compress-express-responses-with-the-compression-middleware-e9d784065065?source=collection_archive---------3----------------------->

![](img/956f04ab7bf2122da425bb91ad941bbd.png)

Photo by [Abigail Lynn](https://unsplash.com/@shmabbss?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

压缩我们的回答是提高我们的 Express 应用程序性能的一个很好的方法。

为此，我们可以使用`compression`中间件。

在本文中，我们将研究压缩数据的可用选项，以及如何用它发送压缩响应。

# 添加中间件

我们可以通过运行以下命令来安装该软件包:

```
npm install compression
```

然后，我们可以将它包括在内:

```
const compression = require('compression');
```

# 选择

`compression`中间件接受一个`options`参数，该参数是一个具有以下属性的对象:

## 组块大小

压缩输出的块大小。默认为 16384 字节或`zlib.Z_DEFAULT_CHUNK`，其中`zlib`为`zlib`节点包。

## 过滤器

决定应该压缩哪种响应的函数。该函数被称为`filter(req, res)`,如果响应类型应该被压缩，则应该返回`true`,如果不应该，则返回`false`。

默认功能使用`compressible`模块检查`res.getHeader('Content-Type')`是否可压缩。

## 水平

要应用的压缩级别压缩级别越高，完成的时间就越长。

值的范围从 0(无压缩)到 9(最高压缩级别)。-1 可用于表示默认压缩级别，相当于级别 6。

## 内存级别

这设置了应该为压缩分配多少内存，并且是一个从 1(最小级别)到 9(最大级别)的整数。

默认为`zlib.Z_DEFAULT_MEMLEVEL`或 8。

## 战略

这将设置压缩算法。该值仅在压缩比上有所不同，并不影响压缩输出的内容。

以下是可用的:

*   `zlib.Z_DEFAULT_STRATEGY` —用于正常数据
*   `zlib.Z_FILTERED` —用于过滤器产生的数据。过滤后的数据主要由随机分布的小值组成。这种算法可以更好地压缩这类数据。该算法强制进行更多的霍夫曼编码。
*   `zlib.Z_FIXED` —用于防止使用动态霍夫曼代码
*   `zlib.Z_HUFFMAN_ONLY` —仅强制霍夫曼编码
*   `zlib.Z_RULE` —将匹配距离限制为 1，这提供了仅与 Huffman 一样好的压缩率，但为 PNG 数据提供了更好的压缩

## 阈值

响应会考虑压缩前响应正文大小的字节阈值。默认`1kb`。该值可以是字节数或由`bytes`模块应用的任何字符串。

## 窗口位

默认值是`zlib.Z_DEFAULT_WINDOWBITS`或 15。

## 。过滤器

默认的过滤函数，我们可以用它来构造一个自定义的过滤函数来扩展默认的操作。

## 保留冲洗

添加了一个`res.flush`方法，强制将部分压缩的响应刷新到客户端。

![](img/04a917d634444cc3d87d1a33bee0ff18.png)

Photo by [Ryan Stone](https://unsplash.com/@rstone_design?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 例子

例如，我们可以如下使用它:

```
const express = require('express');
const bodyParser = require('body-parser');
const compression = require('compression');const app = express();
const shouldCompress = (req, res) => {
  if (req.headers['x-no-compression']) {
    return false
  }
  return compression.filter(req, res);
}app.use(bodyParser.json());
app.use(compression({
  filter: shouldCompress,
  level: 7,
}));app.get('/', (req, res) => {
  const foo = 'foo';
  res.send(foo.repeat(3000));
});app.listen(3000);
```

在上面的代码中，我们添加了`compression`中间件并指定了一些选项。

我们指定当`x-no-compression`头不存在时压缩我们的响应。

此外，我们改变了压缩级别。

有了`x-no-compression`头，我们的响应是 9KB。通过删除`x-no-compression`头启用压缩后，响应只有 402 字节。

正如我们所见，差异是巨大的。这是因为文本是重复的，所以它可以只保留其中的一部分，然后重复它，而不是存储整个字符串。

# 服务器发送的事件

我们可以用服务器发送的事件来压缩响应。为此，我们在输出窗口被缓冲后压缩内容。

一旦我们有了这些，我们就可以把数据发送给客户端。

我们可以使用`res.flush()`方法将任何存在的东西发送给客户端。

例如，我们可以写:

```
const express = require('express');
const bodyParser = require('body-parser');
const compression = require('compression');const app = express();
const shouldCompress = (req, res) => {
  if (req.headers['x-no-compression']) {
    return false
  }
  return compression.filter(req, res);
}app.use(bodyParser.json());
app.use(compression({
  filter: shouldCompress,
  level: 7,
}));app.get('/', (req, res) => {
  res.setHeader('Content-Type', 'text/event-stream');
  res.setHeader('Cache-Control', 'no-cache');
  const timer = setInterval(() => {
    res.write('foo');
    res.flush()
  }, 2000) res.on('close', () => {
    clearInterval(timer)
  })
});app.listen(3000);
```

向客户端发送一个文本事件流，然后我们应该在屏幕上看到每 2 秒钟添加一次的`foo`。

# 结论

`compression`中间件对于压缩常规响应和服务器发送的事件输出非常有用。

我们可以设置压缩级别、块大小等选项。

此外，如果我们想要压缩服务器端事件，我们应该调用`res.flush`将已经缓冲的内容压缩后发送给客户端。