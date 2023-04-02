# Node.js 提示—运行命令、响应头、读取文件等等

> 原文：<https://javascript.plainenglish.io/node-js-tips-run-commands-response-headers-read-files-and-more-77f2b98dc0d3?source=collection_archive---------7----------------------->

![](img/59d505246008ac6bfbf0c8aefa2e0429.png)

Photo by [Andreas Strandman](https://unsplash.com/@strandman?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# Node.js 同步执行系统命令

我们可以与`child_process`模块同步运行系统命令。

例如，我们可以写:

```
const execSync = require('child_process').execSync;
const code = execSync('ls');
```

我们运行`execSync`来运行我们想要的命令。

# 去掉标题 X-Powered-By:Express

我们可以使用`app.disable`方法移除`X-Powered-By`标头。

例如，我们可以写:

```
app.disable('x-powered-by');
```

阻止将`x-powered-by`报头添加到响应中。

我们也可以写:

```
app.set('x-powered-by', false);
```

做同样的事情。

# 向运行 JavaScript 文件的 my package.json 文件添加自定义脚本

我们可以将脚本添加到`package.json`文件的`scripts`部分。

例如，我们可以写:

```
"scripts": {
  "start": "node app.js",
},
```

然后我们可以运行`npm start`来运行它。

# 用 Node.js 生成唯一 ID

为了使创建惟一 id 变得容易，我们可以添加`uuid`包来帮助我们创建 UUIDs。

要安装它，我们运行:

```
npm install uuid
```

然后我们可以通过写来使用它:

```
const { v1: uuidv1 } = require('uuid');
const id = uuidv1();
```

创建 v1 UUID。

我们可以写:

```
const { v4: uuidv4 } = require('uuid');
const id = uuidv4();
```

创建 v4 UUID。

我们还可以使用`crypto`模块创建一个唯一的 ID。然而，它不会是 UUID。

例如，我们可以写:

```
const crypto = require("crypto");
const id = crypto.randomBytes(16).toString("hex");
```

`id`会是一个 32 位的字符串。

# 用 Node.js 替换文件中的字符串

我们可以用`readFile`和`writeFile`把内容替换成新的内容。

例如，我们可以写:

```
const fs = require('fs');
const someFile = './file.txt';fs.readFile(someFile, 'utf8', (err, data) => {
  if (err) {
    return console.log(err);
  } const result = data.replace('old string', 'new string');
  fs.writeFile(someFile, result, 'utf8', (err) => {
    if (err) {
      return console.log(err);
    }
  });
});
```

`someFile`是文件的路径。

我们调用`fs.readFile`来读取 UTF-8 编码的文件。

在回调中，我们得到了`data`，它包含了内容。

我们调用`replace`将`data`中的`'old string'`替换为`'new string'`。

然后我们调用`writeFile`用新内容写文件。

我们还需要`utf8`编码来表明它是一个文本文件。

`err`如果在两个回调中都遇到，将会有错误对象。

# 修复“process.env.NODE_ENV 未定义”错误

我们可以通过运行以下命令来设置`NODE_ENV`的值:

```
SET NODE_ENV=development
```

在 Windows 或:

```
export NODE_ENV=development
```

在 Linux 中设置`NODE_ENV`环境变量。

然后，当我们在运行这些命令之后运行我们的应用程序时,`process.env.NODE_ENV`被设置为`'development'`。

此外，我们可以在运行脚本之前，在`scripts`部分放置一个脚本来设置`NODE_ENV`。

例如，我们可以写:

```
"scripts": {
  "start": "set NODE_ENV=development && node app.js"
}
```

在`package.json`。

# 在 Express.js 资产上设置响应头

我们可以调用`res.set`来设置 Express 资产的标题。

例如，我们可以写:

```
res.set('Content-Type', 'text/plain');
```

其中第一个参数是标题键，第二个参数是标题值。

我们还可以在一个对象中传递来设置多个头:

```
res.set({
  'Content-Type': 'text/plain',
  'Content-Length': '123'
})
```

我们一次设置`Content-Type`和`Content-Length`。

`res.header`方法是`res.set`的别名。

要向现有的响应头添加更多的头，我们可以使用`res.append`方法。

例如，我们可以写:

```
res.append('Set-Cookie', 'foo=bar; Path=/; HttpOnly')
```

我们为 cookie 附加了一个响应头。

# 将文本文件读入节点应用程序中的数组

要将文本文件读入节点应用程序中的数组，我们可以使用`readFile`方法。

例如，我们可以写:

```
const fs = require('fs');fs.readFile('file.txt', (err, data) => {
  if (err) {
    return console.log(err);
  }
  const array = data.toString().split("\n");
  for(const a of array) {
    console.log(a);
  }
});
```

为了以同步的方式读取它，我们可以使用`readFileSync`:

```
constfs = require('fs');const array = fs.readFileSync('file.txt').toString().split("\n");
for(const a of array) {
  console.log(a);
}
```

![](img/90c89c2e6d9df97252be877f82e6f83f.png)

Photo by [David Clode](https://unsplash.com/@davidclode?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以在运行节点应用程序之前设置环境变量。

此外，我们可以通过各种方式读取文件。

我们可以使用`child_process`模块在节点应用程序中运行命令。

可以通过多种方式设置响应头。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**