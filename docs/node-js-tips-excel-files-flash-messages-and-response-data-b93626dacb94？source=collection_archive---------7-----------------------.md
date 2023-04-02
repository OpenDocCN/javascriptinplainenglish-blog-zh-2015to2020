# Node.js 提示— Excel 文件、快速消息和响应数据

> 原文：<https://javascript.plainenglish.io/node-js-tips-excel-files-flash-messages-and-response-data-b93626dacb94?source=collection_archive---------7----------------------->

![](img/b162e567b10aebaddcc9facdd3092573.png)

Photo by [ben o'bro](https://unsplash.com/@benobro?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 正确导出 Node.js 中的异步函数

我们可以像导出其他函数一样导出一个异步函数。

例如，我们可以写:

```
async function doSomething() {
  // ...
}

module.exports.doSomething = doSomething;
```

我们像对待其他函数一样导出`doSomething`。

# Base64 Nodejs 中的 ReadFile

我们可以通过使用`readFileSync`方法将文件读入 base64 字符串。

例如，我们可以写:

```
const fs = require('fs');
const str = fs.readFileSync('/path/to/file.jpg', { encoding: 'base64' });
```

我们用一个对象传入第二个参数。

它将`encoding`属性设置为`'base64'`，这样我们就可以将文件解析成 base64 字符串。

它被送回并分配给`str`。

# 用 Mongoose 查找和更新子文档

我们可以用`findOneAndUpdate` 方法找到并更新一个子文档。

例如，我们可以写:

```
Person.findOneAndUpdate(
  { "_id": personId, "address._id": address._id },
  { 
    "$set": {
      "address.$": address
    }
  },
  (err, doc) => {
    console.log(doc);
  }
);
```

我们调用带有几个参数的`findOneAndUpdate`方法。

第一个是查找要更新的对象的查询。

我们用它来查询父文档和子文档。

我们通过 ID 查询`person`文档，用`address._id`查询`address`。

第二个参数是为设置地址子文档的`address.$`设置值的`$set`方法。

`$`是位置操作变量。

它用于获取与给定位置匹配的子文档。

一旦操作完成，回调就会得到结果。

# 如果一个测试失败，跳过 Spec 中后续的 Mocha 测试

Mocha 有一个`bail`操作，如果一个测试失败，就跳过剩余的测试。

我们也可以简称`b`。

# 从 Axios API 返回数据

为了从 Axios 发出的 HTTP 请求中获取数据，我们可以在`then`回调中返回`response.data`对象。

例如，我们可以写:

```
axios.get(url)
  .then(response => {
     return response.data;
  })
  then(data => {
    res.json(data);
  })
```

请求成功后，我们用响应数据调用`res.json`。

# 阅读获取承诺的正文

我们可以通过使用`json`方法将响应对象转换为 JSON 对象来获得 Fetch 承诺的主体。

例如，我们可以写:

```
fetch('https://api.ipify.org?format=json')
  .then(response => response.json())
  .then‌​(data => console.log(data))
```

我们调用`response.json()`来获取 JSON。

# 用 Node 解析 XLSX 并创建 JSON

为了使在节点应用程序中解析 XLSX 文件变得容易，我们可以使用`xlsx`包。

我们可以通过书写来使用它:

```
const XLSX = require('xlsx');
const workbook = XLSX.readFile('spreadsheet.xlsx');
const [sheetName] = workbook.SheetNames;
const json = XLSX.utils.sheet_to_json(workbook.Sheets[sheetName])
```

我们使用`xlsx`包装。

然后我们可以使用`readFile`来读取给定路径的电子表格。

属性从 XLSX 文件中获取工作表名称。

在我们的例子中，我们得到了第一个名字。

然后我们调用`sheet_to_json`来转换具有给定名称的工作表。

我们通过使用`workbook.Sheets`对象获取工作表。

# 检查 NodeJS 中的 JSON 是否为空

我们可以用`Object.keys`方法检查 JSON 对象是否为空。

例如，我们可以写:

```
Object.keys(obj).length === 0;
```

其中`obj`是我们要检查的 JSON 对象。

# 在 Express 中发送即时消息

即时消息是在会话期间存储的消息。

我们可以使用`connect-flash`中间件向模板发送 flash 消息。

为了安装这个包，我们运行:

```
npm install connect-flash
```

然后我们可以写:

```
const flash = require('connect-flash');
const app = express();app.configure(function() {
  app.use(express.cookieParser(''));
  app.use(express.session({ cookie: { maxAge: 60000 }}));
  app.use(flash());
});app.get('/flash', (req, res) => {
  req.flash('info', 'flash message');
  res.redirect('/');
});app.get('/', (req, res) => {
  res.render('index', { messages: req.flash('info') });
});
```

我们用`req.flash`设置了 flash 消息。

第一个参数是信息的关键。

第二个参数是消息本身。

如果我们像在`'/'`路线上那样使用带有一个参数的`req.flash`，我们就可以通过键获得消息。

![](img/8404950b9be4472e5c06f90dbef6843d.png)

Photo by [Rebecca Hobbs](https://unsplash.com/@luna_and_boo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以像导出其他函数一样导出异步函数。

`connect-flash`包让我们存储会话期间可用的消息。

`xlsx`包让我们从 XLSX 文件中解析数据。

子文档可以用 Mongoose 更新。

我们可以获得各种 HTTP 客户端的响应数据。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**