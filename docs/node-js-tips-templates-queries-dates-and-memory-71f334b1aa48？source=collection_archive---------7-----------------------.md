# Node.js 提示—模板、查询、日期和内存

> 原文：<https://javascript.plainenglish.io/node-js-tips-templates-queries-dates-and-memory-71f334b1aa48?source=collection_archive---------7----------------------->

![](img/ef415d1c132e548bf8af9d58d607fc01.png)

DFFPhoto by [Sarah Olive](https://unsplash.com/@taffy_apple?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 从 fs.readFile 获取数据

我们可以从`fs`模块调用`readFile`方法来踩踏文件。

例如，我们可以写:

```
fs.readFile('./foo.txt', (err, data) => {
  if (err) {
    throw err;
  }
  console.log(data);
});
```

我们传入路径，然后将其作为二进制文件读取。

`data`有文件内容，`err`是遇到错误时设置的错误对象。

还有一种`fs.readFileSync`方法:

```
const text = fs.readFileSync('./foo.txt', 'utf8');
console.log(text);
```

# 在 Express 中获取 URL 参数

要在 Express 应用程序中获取 URL 参数，我们可以使用`req.params`属性来获取 URL 参数。

例如，我们可以写:

```
app.get('/tags/:tagId', (req, res) => {
  res.send(req.params.tagId);
});
```

然后，如果我们向`/tags/2`发出 GET 请求，我们将得到 2 作为`req.params.tagId`的值。

为了从查询参数中获取一个值，我们可以使用`req.query`属性:

```
app.get('/foo', (req, res) => {
  res.send(req.query.tagId);
});
```

那么如果我们向`/foo?tagId=2`发出请求，`req.query.tagId`将会是 2。

# 呈现基本 HTML 视图

我们可以通过使用`include`在 Jade 视图中包含 HTML 文件来呈现 HTML。

例如，我们可以在`views/index.jade`中写:

```
include plainHtml.html
```

然后在`views/plainHtml.html`中，我们可以编写普通的 HTML”

```
<!DOCTYPE html>
...
```

我们用以下内容渲染`index.jade`:

```
res.render(index);
```

# NodeJS 支持导入/导出 ES6 (ES2015)模块

如果我们的应用运行在节点 13.2.0 或更高版本上，我们可以通过向`package.json`添加以下内容来启用 ES6 模块支持:

```
{
  "type": "module"
}
```

对于 Node 13.1.0 或更低版本，我们可以编写:

```
node -r esm main.js
```

我们通过使用`esm`打开 ES 模块支持。

我们只是运行`npm i esm`来将`esm`添加到我们的节点项目中。

# 使用下划线. js 作为模板引擎

我们可以使用`template`方法来传递 HTML 模板字符串。

例如，我们可以写:

```
const template  = _.template("<h1>hello <%= name %></h1>");
```

`template`是一个带占位符的函数。

那么`name`就是我们可以传入的占位符。

例如，我们可以写:

```
const str = template({ name: 'world' });
```

那么`str`就是`'<h1>hello world</h1>’`。

`<%= %>`表示打印模板中的一些值。

`<% %>`执行一些代码。

`<%- %>`打印一些带有转义 HTML 的值。

# 运行 npm start 时，启动脚本丢失错误

为了解决这个错误，我们可以将`start`脚本添加到`package.json`中，把它放在`scripts`部分。

例如，我们可以写:

```
"scripts": {
  "start": "node app.js"
}
```

当我们运行`npm start`时，带有键`start`的脚本将会运行。

# Node.js 堆内存不足

我们可以通过设置`max-old-space-size`选项来增加节点应用的内存限制。

例如，我们可以写:

```
node --max-old-space-size=2048 app.js
```

将内存限制增加到 2 GB。

我们也可以在`NODE_OPTIONS`环境变量中设置它:

```
export NODE_OPTIONS=--max_old_space_size=2048
```

# 所有异步 forEach 回调完成后的回调

我们可以使用 for-await-of 循环来遍历承诺。

例如，我们可以写:

```
const get = a => fetch(a);const getAll = async () => {
  const promises = [1, 2, 3].map(get); for await (const item of promises) {
    console.log(item);
  }
}
```

这将通过一个循环依次运行每个承诺。

这从 ES2018 开始提供。

# 将 UTC 日期格式化为“YYYY-MM-DD hh:mm:ss”字符串

我们可以使用`toISOString`方法将`Date`实例格式化为‘YYYY-MM-DD hh:MM:ss’格式的日期字符串。

例如，我们可以写“

```
const str = new Date()
  .toISOString()
  .replace(/T/, ' ')
  .replace(/\..+/, '');
```

我们用空字符串替换`T`，用空字符串替换点之后的文本。

然后我们得到`'2020–06–04 16:15:55'`作为结果。

![](img/a4c8dd8995f977247bcf1d56a345f2eb.png)

Photo by [Melissa Regina](https://unsplash.com/@tadmint?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

下划线有一个`template`方法。

`readFile`可以读取文件。

我们可以在 Express 中获得查询参数。

可以增加节点应用程序的内存限制。

Jade 模板可以包含 HTML 局部视图。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)和 [**找到它们订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**