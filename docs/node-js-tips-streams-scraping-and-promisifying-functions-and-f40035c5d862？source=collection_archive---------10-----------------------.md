# Node.js 提示—流、抓取和承诺函数以及配置

> 原文：<https://javascript.plainenglish.io/node-js-tips-streams-scraping-and-promisifying-functions-and-f40035c5d862?source=collection_archive---------10----------------------->

![](img/32105c487325e889a16d62701d65cdd9.png)

Photo by [Tamas Pap](https://unsplash.com/@tamasp?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 在 Node.js 中将缓冲区转换为 ReadableStream

我们可以用`ReadableStreamBuffer`构造函数将缓冲区转换成可读的流。

例如，我们可以写:

```
const { ReadableStreamBuffer } = require('stream-buffers');
const readableStreamBuffer = new ReadableStreamBuffer({
  frequency: 10,
  chunkSize: 2048
});readableStreamBuffer.put(aBuffer);
```

其中`aBuffer`是我们要转换的缓冲区。

`frequency`是组块被抽出的频率。

可以在带有`chunkSize`属性的构造函数中改变大小。

# 使用 Node.js 实时抓取网页

我们可以使用 cheerio 库实时抓取网页。

要安装它，我们运行:

```
npm install cheerio
```

然后我们可以通过写来使用它:

```
const cheerio = require('cheerio');
const $ = cheerio.load('<h1 class="title">Hello world</h1>');$('h1.title').text('Hello James');
$('h1').addClass('welcome');$.html();
```

我们可以获取 HTML 的内容，并像处理 jQuery 一样处理它。

我们可以将它与 Axios 之类的 HTTP 客户端结合起来获得 HTML。

然后我们可以使用 Cheerio 解析它的内容。

例如，我们可以写:

```
const axios = require('axios');
const cheerio = require('cheerio');axios.get('https://example.com')
.then(({ data }) => {
  const $ = cheerio.load(data);
  const text = $('h1').text();
  console.log(text);       
})
```

我们用 Axios 向一个网站发出 GET 请求。

然后我们用 cheerio 和`cheerio.load`解析数据。

然后我们用`text`方法得到`h1`的内容。

任何选择器都可以用来获取数据。

# 用 JavaScript 生成 MD5 文件散列

我们可以用 crypto-js 包生成一个 MD5 散列。

要安装它，我们可以运行:

```
npm install crypto-js
```

然后我们可以写:

```
import MD5 from "crypto-js/md5";
const md5Hash = MD5("hello world");
```

来生成哈希。

# 使用 Node.js 进行服务器端浏览器检测

我们可以从请求中获取`user-agent`头来获取用户代理字符串。

为了解析字符串，我们可以使用 ua-parser-js 包。

要安装它，我们运行:

```
npm install ua-parser-js
```

在我们的 Express 应用程序中，我们可以创建自己的中间件来检查用户代理:

```
const UAParser = require('ua-parser-js');const checkBrowser = (req, res, next) => {
  const parser = new UAParser();
  const ua = req.headers['user-agent'];
  const browserName = parser.setUA(ua).getBrowser().name;
  const fullBrowserVersion = parser.setUA(ua).getBrowser().version; console.log(browserName);
  console.log(fullBrowserVersion);
  next();
}app.all(/*/, checkBrowser);
```

我们用`req.headers[‘user-agent’]`得到`user-agent`头。

然后我们使用`UAParser`构造函数来解析用户代理字符串。

我们调用`getBrowser`来获取浏览器数据。

`name`具有浏览器名称，`version`具有版本。

# 在 Express 应用程序中存储数据库配置的最佳方式

我们可以将配置存储在配置文件中。

例如，我们可以写:

```
const fs = require('fs');
const configPath = './config.json';
const configFile = fs.readFileSync(configPath, 'utf-8')
const parsedConfig = JSON.parse(configFile);
exports.storageConfig = parsedConfig;
```

我们调用`readFileSync`来读取`config.json`。

然后我们解析 JSON 字符串中的数据。

我们导出最后一行中的配置。

# Promisify 节点的 child_process.exec 和 child_process.execFile 函数

我们可以用蓝鸟转换`child_process` `exec`和`execFile`方法。

例如，我们可以写:

```
const util = require('util');
const exec = util.promisify(require('child_process').exec);const listFiles = async () => {
  try {
    const { stdout, stderr } = await exec('ls');
    console.log('stdout:', stdout);
    console.log('stderr:', stderr);
  } catch (e) {
    console.error(e);
  }
}listFiles();
```

我们使用`util`库的`promisify`方法将`exec`方法转换为承诺。

然后我们可以用`ls`命令调用 promised`exec`方法。

然后我们用`stdout`和`stderr`得到全输出。

`stdout`有结果了。`stderr`有错误输出。

![](img/ce0bf44e05ef5d5554f75406b66f5495.png)

Photo by [Alysa Bajenaru](https://unsplash.com/@alysa?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以将一个缓冲区转换成一个可读的流。

我们可以用 HTTP 客户端和 cheerio 抓取网页。

crypto-js 使用 MD5 方法创建 MD5 散列。

来自`child_process`的方法可以转换成承诺。

我们可以在服务器端解析用户代理字符串，解析浏览器版本数据。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**