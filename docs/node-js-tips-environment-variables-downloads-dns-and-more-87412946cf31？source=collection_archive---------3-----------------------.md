# Node.js 提示—环境变量、下载、DNS 等等

> 原文：<https://javascript.plainenglish.io/node-js-tips-environment-variables-downloads-dns-and-more-87412946cf31?source=collection_archive---------3----------------------->

![](img/75011c5064449b82e28e608230159faf.png)

Photo by [Nicolas Tissot](https://unsplash.com/@nft?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# Node.js 需要一个文件夹中的所有文件

我们可以通过创建一个导出所有模块的模块来导出一个文件夹中的所有模块。

然后我们可以一次全部导入。

例如，我们可以写:

`index.js`

```
exports.foo = require("./routes/foo.js");
exports.bar= require("./routes/bar.js");
```

`app.js`

```
const routes = require("./routes");
```

我们也可以写:

```
const fs = require("fs");
const normalizedPath = require("path").join(__dirname, "routes");fs.readdirSync(normalizedPath).forEach((file) => {
  require(`./routes/${file}`);
});
```

# 错误字符串

我们可以通过遍历对象的属性来记录错误。

例如，我们可以写:

```
const error = new Error('error');
const propertyNames = Object.getOwnPropertyNames(error);

for (const prop of propertyNames) {
  const descriptor = Object.getOwnPropertyDescriptor(error, prop);
  console.log(prop, descriptor);
}
```

我们使用`Object.getOwnPropertyNames`方法来获取`error`对象的属性。

然后我们得到每个属性的描述符。

# Node.js 中“process.stdout.write”和“console.log”的区别

`console.log`用格式化的输出调用`process.stdout.write`。

`console.log`的代码只是:

```
console.log = function (d) {
  process.stdout.write(d + '\n');
};
```

除了额外的换行符之外，它们几乎是等效的。

# 在 Mac OS 或 Windows 中将 NODE_ENV 设置为生产/开发

我们可以使用`export`命令在 Mac OS 中设置环境变量。

例如，我们可以通过运行以下命令来设置`NODE_ENV`:

```
export NODE_ENV=production
```

在 Windows 中，我们可以使用`SET`命令:

```
SET NODE_ENV=production
```

我们也可以直接设置`process.env.NODE_ENV`:

```
process.env.NODE_ENV = 'production';
```

我们可以通过运行以下命令来设置运行文件时的环境:

```
NODE_ENV=production node app.js
```

`package.json`中也是如此`scripts`:

```
{
  ...
  "scripts": {
    "start": "NODE_ENV=production node ./app"
  }
  ...
}
```

# 在快速模板中使用要求

`require`包含在 CommonJS 中。

它本身不是浏览器端 JavaScript 的一部分。

为了包含 JavaScript 文件，我们可以使用 Script 标签。

另外，我们可以使用 CommonJS 模块。

或者我们可以使用异步模块。

Browserify、Webpack 和 Rollup 可以将模块捆绑到浏览器可用的构建构件中。

我们还可以在浏览器中本地使用 ES6 模块，将 type 属性设置为`module`。

例如，我们可以写:

`script.js`

```
export const hello = () => {
  return "Hello World";
}
```

然后，我们可以通过编写以下内容来包含`script.js`:

```
<script type="module" src="script.js"></script>
```

# 用回调度量 JavaScript 代码的执行时间

我们可以使用`console.time`和`console.timeEnd`方法来测量一个脚本的时间。

例如，如果我们有:

```
for (let i = 1; i < 10; i++) {
  const user = {
    id: i,
    name: "name"
  };
  db.users.save(user, (err, saved) => {
    if(err || !saved) {
      console.log("error");
    } else {
      console.log("saved");
    }
  });
}
```

我们可以通过编写来调用这些方法:

```
console.time("save");for (let i = 1; i < 10; i++) {
  const user = {
    id: i,
    name: "name"
  };
  db.users.save(user, (err, saved) => {
    if(err || !saved) {
      console.log("error");
    } else {
      console.log("saved");
    }
    console.timeEnd("save");
  });
}
```

我们必须将相同的字符串传递给`time`和`timeEnd`。

# 使用 Express 从 NodeJS 服务器下载文件

我们可以通过快捷方式用`res.download`方法下载一个文件。

例如，我们可以写:

```
const path = reqyire('path');app.get('/download', (req, res) => {
  const file = path.join(`${__dirname}, 'folder', 'img.jpg');
  res.download(file);
});
```

我们只需获取路径并将其传递给`res.download`。

# URl 编码 Node.js 中的字符串

我们可以用`querystring`模块对 Node.jhs 中的字符串进行 URI 编码。

例如，我们可以写:

```
const querystring = require("querystring");
const result = querystring.stringify({ text: "hello world"});
```

然后我们得到`result`的`'text=hello%20world’`。

# 获取 Node.js 中的本地 IP 地址

我们可以用`dns`模块在一个节点 app 中获取一个本地 IP 地址。

例如，我们可以写:

```
const dns = require('dns');
const os = require('os');dns.lookup(os.hostname(), (err, addr, fam) => {
  console.log(addr);
})
```

我们用主机名调用`lookup`来从主机名中获取 IP 地址。

然后`addr`有了 IP 地址。

# 结论

我们可以用`dns`模块获取 IP 地址。

`console`有方法让我们测量代码的计时。

我们可以动态地要求文件。

环境变量可以动态设置。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)和 [**找到它们订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**