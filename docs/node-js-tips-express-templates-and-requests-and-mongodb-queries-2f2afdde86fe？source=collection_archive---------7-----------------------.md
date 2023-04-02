# Node.js 提示— Express 模板和请求，以及 MongoDB 查询

> 原文：<https://javascript.plainenglish.io/node-js-tips-express-templates-and-requests-and-mongodb-queries-2f2afdde86fe?source=collection_archive---------7----------------------->

![](img/66457d73e19f04c214c029e8a8214cae.png)

Photo by [Anton Chernyavskiy](https://unsplash.com/@antonchernyavskiy?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 关闭 Express 服务器

如果我们需要关闭 Express 服务器，我们只需在它上面调用`close`来关闭它。

它是从`listen`返回的，是一个 HTTP 服务器。

例如，我们可以写:

```
const server = app.listen(3000);
server.close();
```

# MongoDB 和 Mongoose 中的全文搜索

我们可以通过在模式的标识符上创建一个索引来搜索 Mongoose。

索引将加快搜索速度。

例如，我们可以写:

```
const schema = new Schema({
  name: String,
});schema.index({
  name: 'text',
});
```

然后我们可以写:

```
Model.find({
    $text: {
      $search: searchString
    }
  })
  .skip(20)
  .limit(10)
  .exec((err, docs) => {
    //...
  });
```

去做搜索。

我们使用`$search`操作符对条目进行搜索。

`skip`跳过参数中给出的条目数。

`limit`限制争论的数量。

# 在 Express 中获取发出请求的域

我们可以使用`req.get`从`host`头中获取主机名。

我们也可以对跨来源请求使用 thr `origin`头。

为了得到`host`头，我们写:

```
const host = req.get('host');
```

我们写道:

```
const origin = req.get('origin');
```

为了得到原点。

# 在客户端 JavaScript 上访问 Express.js 局部变量

我们可以从模板中传递数据，然后在那里解析它。

为此，我们写道:

```
script(type='text/javascript').
 const localData =!{JSON.stringify(data)}
```

我们对数据进行了字符串化，这样它将被插入到模板中。

# 用 Jade/Pug 有条件地添加类

要有条件地添加一个类，我们可以这样写:

```
div.collapse(class=typeof fromEdit === "undefined" ? "edit" : "")
```

我们将 JavaScript 表达式放在括号内，它会为我们插入类名。

# 将数据库配置存储在节点或 Express 应用程序中

我们可以将配置存储在一个 JSON 文件中。

例如，我们可以写:

```
const fs = require('fs'),
configPath = './config.json';
const parsed = JSON.parse(fs.readFileSync(configPath, 'UTF-8'));
exports.config = parsed;
```

我们用`readFileSync`读取配置，然后对返回的文本字符串调用`JSON.parse`。

# 用 Mongoose 从文档中排除一些字段

为了排除一些返回的字段，我们在`tranform`方法中转换它，通过在它上面使用`delete`来删除我们想要的属性。

例如，我们可以写:

```
UserSchema.set('toJSON', {
  transform(doc, ret, options) {
    delete ret.password;
    return ret;
  }
});
```

我们调用`set`来添加一个方法，用`transform`方法来转换数据。

`ret`有返回结果。

然后我们在上面调用`delete`。

# 在 HTTP 和 HTTPS 上监听单个 Express 应用程序

我们可以用一个快捷应用程序收听 HTTP 和 HTTPS。

要侦听 HTTP 请求，我们只需读取私有文件和证书文件。

然后我们可以用它们创建一个服务器。

例如，我们可以写:

```
const express = require('express');
const https = require('https');
const http = require('http');
const fs = require('fs');
const  app = express();const options = {
  key: fs.readFileSync('/path/key.pem'),
  cert: fs.readFileSync('/path/cert.pem'),
  ca: fs.readFileSync('/path/ca.pem')
};http.createServer(app).listen(80);
https.createServer(options, app).listen(443);
```

我们用`readFileSync`读入文件。

然后我们将整个事情传递给`https.createServer`方法来创建我们的服务器。

我们仍然需要`http.createServer`来监听 HTTP 请求。

# 获取从 Express 中的表单传递的数据

我们可以用`body-parser`包从 Express 中的一个表单获取数据。

我们使用它附带的`urlencoded`中间件。

例如，我们可以写:

```
const bodyParser = require('body-parser');
app.use(bodyParser.urlencoded({ extended: true }));app.post('/game', (req, res) => {
  res.send(req.body);
});
```

一旦我们运行了`urlencoded`中间件，它将解析来自 POST 请求的表单数据。

`extended`意味着我们也可以在请求体中解析 JSON。

然后在我们的路由中，用`req.body`就可以得到数据。

# Jade /Pug 模板引擎中的循环

我们可以像 JavaScript `for`循环一样用 Pug 写一个循环。

例如，我们写道:

```
- for (let i = 0; i < 10; i) {
  li= array[i]
- }
```

![](img/2a6a0ea2cf342832c1f76df5c4541c31.png)

Photo by [Krista Joy Montgomery](https://unsplash.com/@irishkristajoy?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以和帕格玩圈圈游戏。

此外，我们可以从 JSON 文件中读取 config。

Express 可以监听 HTTP 和 HTTPS 请求。

我们可以在查询 MongoDB 数据时转换它们。

为了添加文本搜索功能，我们可以添加一个索引来加速搜索，并使用`$search`操作符。

# 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**