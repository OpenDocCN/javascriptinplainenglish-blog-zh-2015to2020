# Node.js 提示—MongoDB 连接、获取和导出

> 原文：<https://javascript.plainenglish.io/node-js-tips-mongodb-connections-fetch-and-exports-4847e121a724?source=collection_archive---------6----------------------->

![](img/c014e609d0588fc74c32c2d54fc225a2.png)

Photo by [Michael Browning](https://unsplash.com/@michaelwb?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 从 Fetch API 返回的 ReadableStream 对象中检索数据

我们可以通过调用转换函数将数据转换成我们想要的数据类型，从一个`ReadableStream`对象中检索数据。

例如，我们写道:

```
fetch('https://api.agify.io/?name=michael')
  .then(response => response.json())
  .then((data) => {
    console.log(data);
  });
```

我们用`response.json()`调用完成了到 JSON 的转换。

然后我们可以在下一次`then`回调中得到`data`。

# 在 Node.js 中的文件之间共享变量

我们可以通过创建模块来共享变量。

例如，我们可以写:

`module.js`

```
const name = "foo";
exports.name = name;
```

`app.js`

```
const module = require('./module');
const name = module.name;
```

我们通过将`name`常量设置为`exports`的属性来导出它。

然后我们用`app.js`中的`require`导入。

我们可以像使用其他财产一样使用它。

# 在 Node.js 中使用 fs 和 async / await

如果我们先将`async`和`await`转换成承诺，我们可以将它们与`fs`方法一起使用。

例如，我们可以写:

```
const fs = require('fs');
const util = require('util');const readdir = util.promisify(fs.readdir);const read = async () => {
  try {
    const names = await readdir('/foo/bar/dir');
    console.log(names);
  } catch (err) {
    console.log(err);
  }
}
```

我们用`util.promisify`将读取目录的`fs.readdir`方法转换成 promise 版本。

然后我们可以配合`async`和`await`使用。

# Mongoose 和单个 Node.js 项目中的多个数据库

我们可以在单个节点项目中连接到多个 MongoDB 数据库。

例如，我们可以写:

`fooDbConnect.js`

```
const mongoose = require('mongoose');
mongoose.connect('mongodb://localhost/foo');
module.exports = exports = mongoose;
```

`bazDbConnect.js`

```
const mongoose = require('mongoose');
mongoose.connect('mongodb://localhost/baz');
module.exports = exports = mongoose;
```

我们用每个文件中的`connect`连接到不同的数据库，并导出连接。

然后在`db.js`中，我们写道:

```
const mongoose = require("./fooDbConnect");
```

连接到`foo`数据库。

# 将 module.exports 用作构造函数

我们可以用`module.exports`导出一个构造函数。

例如，我们可以写:

`person.js`

```
function Person(name) {
  this.name= name;
}Person.prototype.greet = function() {
  console.log(this.name);
};module.exports = Person;
```

然后我们可以通过编写以下内容来导入它:

`app.js`

```
const Person = require("./person");
const person = new Person("james");
```

我们在`app.js`中导入`Person`构造函数，然后在最后一行调用它。

# 使用 Node.js 读取文本文件

我们可以用`fs.readFile`方法读取文本文件。

例如，我们可以写:

```
const fs = require('fs');

fs.readFile('./foo.txt', 'utf8', (err, data) => {
  if (err) {
    console.log(err);
  }
  console.log(data);
});
```

我们将文件路径作为第一个参数传入。

编码是第二个参数。

文件读取操作完成时调用的回调是第三个参数。

`data`有文件而`err`有错误对象则是遇到了错误。

# 跨节点模块重用到 MongoDB 的连接

我们可以导出 MongoDB 连接，以便在其他节点应用程序模块中使用它们。

例如，我们可以写:

`dbConnection.js`

```
const MongoClient = require( 'mongodb' ).MongoClient;
const url = "mongodb://localhost:27017";let _db;module.exports = {
  connect(callback) {
    MongoClient.connect( url, { useNewUrlParser: true }, (err, client) => {
      _db  = client.db('some_db');
      return callback(err, client);
    });
  },

  getDb() {
    return _db;
  }
};
```

我们创建了`connect`函数来连接 MongoDB 数据库。

连接对象被设置为一个变量。

它需要一个回调函数，我们可以用它来得到错误。

然后我们创建了`getDb`函数来获取 MongoDB 连接。

然后我们可以通过写来使用`connect.js`:

```
const dbConnection = require('dbConnection');

dbConnection.connect((err, client) => {
  if (err) {
    console.log(err);
  }
} );
```

我们通过导入之前创建的`dbConnection`模块连接到文件中的数据库。

然后我们用我们想要的回调来调用`connect`。

我们用`dbConnection.js`中的`err`和`client`调用它，所以这就是我们回调的参数。

![](img/c41c6c9a09240e12d1b7df5e8f6745e9.png)

Photo by [Alan J. Hendry](https://unsplash.com/@imedianamibia?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

MongoDB 连接可以在模块之间共享。

在使用数据之前，我们必须将来自`fetch`函数的原始响应转换成我们想要的数据类型。

`fs`方法可以转化为承诺。

我们可以用`module.exports`导出构造函数。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**