# Node.js 提示— CORS、Excel 文件和 CSV

> 原文：<https://javascript.plainenglish.io/node-js-tips-cors-excel-files-and-csvs-7bc900a61c12?source=collection_archive---------5----------------------->

![](img/a98ffc74ba38715f0636f43f833c2cf5.png)

Photo by [Viktor Talashuk](https://unsplash.com/@viktortalashuk?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 使用 Node.js 读取 Excel 文件

有几个软件包可以让我们在一个节点应用程序中读取 Excel 文件。

我们可以使用的一个包是`node-xlsx`包。

为了使用它，我们写:

```
const xlsx = require('node-xlsx');
const path = require('path');
const obj = xlsx.parse(path.join(__dirname, '/myFile.xlsx'));
```

我们可以传入一个路径来解析来自这个路径的 Excel 文件。

此外，我们可以写:

```
const xlsx = require('node-xlsx');
const path = require('path');
const obj = xlsx.parse(fs.readFileSync(path.join(__dirname, '/myFile.xlsx'));
```

解析一个缓冲区，这个缓冲区由`readFileSync`返回。

ExcelJS 是另一个我们可以用来解析工作簿的包。

例如，我们可以写:

```
const ExcelJS = require('exceljs');
const path = require('path');
const workbook = new Excel.Workbook();
const filename = path.join(__dirname, '/myFile.xlsx');
workbook.xlsx.readFile(filename);
  .then(() => {
    // use workbook
  });
```

从工作簿中读取。

此外，我们可以写:

```
const workbook = new Excel.Workbook();
stream.pipe(workbook.xlsx.createInputStream());
```

从流中读取。

# 在 Express 中将变量传递给 JavaScript

我们可以通过向`res.render`传递第二个参数来传递变量来表达模板。

例如，我们可以写:

```
app.get('/foo.js', (req, res) => {
  res.set('Content-Type', 'application/javascript');
  res.render('foo', { foo: { bar: 'baz' } });
});
```

然后在`foo`模板中，我们可以写:

```
<script>
  const foo = <%- JSON.stringify(foo) %>;
</script>
```

我们使用 EJS 模板，所以我们用`<%- %>`插入变量。

此外，我们必须调用`JSON.stringify`将它转换成一个字符串，这样我们就可以对它进行插值。

# 写入 Node.js 中的 CSV

我们可以使用`csv-stringify`包从嵌套数组中创建一个 CSV 字符串。

然后我们可以使用`fs.writeFile`将 CSV 字符串写入文件。

例如，我们可以写:

```
import stringify from 'csv-stringify';
import fs from 'fs';let data = [];
let columns = {
  id: 'id',
  name: 'name'
};for (let i = 0; i < 100; i++) {
  data.push([i, `name ${i}`]);
}stringify(data, { header: true, columns }, (err, output) => {
  if (err) throw err;
  fs.writeFile('file.csv', output, (err) => {
    if (err) throw err;
    console.log('csv saved.');
  });
});
```

我们为列创建了一个带有`columns`数组的嵌套数组。

`columns`中的键是字段名。`columns`中的值是标题名称。

然后，我们将数据填充到`data`数组中。

接下来，我们调用`stringify`将标题和数据转换成一个 CSV 字符串。

第一个参数是数据。

那么 2nd 就是一个有一些选项的对象。

`header`设置为`true`意味着我们显示一些表头。

`columns`用于设置列字段。

这些列由每行中每个项目的位置填充。

对于`data`中的每一行，第一个条目是`id`，第二个是`name`。

当 CSV 转换完成时调用回调。

`output`有 CSV 字符串。

然后我们可以`fs.writeFile`把字符串写到一个文件中。

# Reload Express.js 路由更改，无需手动重启服务器

当我们使用 Nodemon 修改代码时，我们可以让 Express 应用程序自动重启。

一旦安装完毕，我们可以用`nodemon`而不是`node`运行我们的应用程序。

要安装它，我们运行:

```
npm install -g nodemon
```

然后我们运行:

```
nodemon app.js
```

去使用它。

# 修复快速应用程序的“预检响应中的访问控制允许标题不允许请求标题字段授权”错误

我们可以添加`cors`中间件来允许跨来源的请求。

那么这个错误应该会消失。

我们可以写:

```
const express = require('express');
const cors = require('cors');
const app = express();
app.use(cors());
app.options('*', cors());
```

我们使用`cors`中间件来添加`Access-Control-Allow-Origin`和`Access-Control-Allow-Methods`头。

此外，我们用`cors`中间件添加了所需的选项路由。

我们允许所有路由接受带有`'*'`的选项请求。

![](img/ab63f0f35a8fa53a810685fc334696d5.png)

Photo by [Bonnie Kittle](https://unsplash.com/@bonniekdesign?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

有几个读取 Excel 文件的包。

我们可以用 Express 把变量传递给模板。

此外，我们可以使用库轻松地将 CSV 写入文件。

为了让我们的 Express 应用程序监听跨来源的请求，我们需要`cors`中间件。