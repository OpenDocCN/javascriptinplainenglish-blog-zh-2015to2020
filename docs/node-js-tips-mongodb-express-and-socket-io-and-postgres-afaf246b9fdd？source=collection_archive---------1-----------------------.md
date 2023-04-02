# Node.js 提示— MongoDB、Express 和 Socket.io 以及 Postgres

> 原文：<https://javascript.plainenglish.io/node-js-tips-mongodb-express-and-socket-io-and-postgres-afaf246b9fdd?source=collection_archive---------1----------------------->

![](img/349e82ea6c5daff75c0390f8d024041e.png)

Photo by [Anne Nygård](https://unsplash.com/@polarmermaid?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 获取 NodeJS 中 Mongo 数据库中插入文档的 id

我们可以从由`insert`方法调用的回调中获得 Mongo 数据库中插入文档的`_id`。

例如，我们可以写:

```
collection.insert(obj, (err, docsInserted) =>{
  console.log(docsInserted);
});
```

我们将`obj`传递给`collection`。

然后我们获取`docsInserted`参数的值，以获得整个对象，包括带有`_id`的 ID。

# Redis 中的数据值更改时通知节点应用程序

我们可以使用 Redis client for Node 来观察数据值的变化。

例如，我们可以写:

```
const redis = require("redis");
const client = redis.createClient();client.subscribe("pubsub");
client.on("message", (channel, message) => {
  console.log(channel, message);
});
```

我们只是调用`createClient`来创建一个 Redis 客户端实例。

然后我们订阅`pubsub`动作来监听发布的变更。

最后，我们监听`'message'`事件来获取消息。

# 使用 Sequelize.js 删除查询

我们可以用 Sequelize 编写一个删除查询:

```
Model.destroy({
  where: {
    id: 123
  }
})
```

其中`Model`是数据表的模型类，它包含我们想要删除的条目。

然后我们调用`destroy`删除条目。

`where`已经到了哪里。`id`是 ID 字段。

它可以用其他标准代替。

# 让 Express.js 应用程序在不同的端口上工作

我们不必硬编码 Express 应用程序监听的端口。

相反，我们可以从环境变量中读取值。

例如，我们可以写:

`app.js`

```
const express = require("express");
const app = express();app.set('port', process.env.PORT || 3000);app.get('/', (req, res) => {
  res.send('hello world');
});app.listen(app.get('port'));
```

然后，我们从环境变量中获取端口，而不是对端口进行硬编码。

`process.env.PORT`是`PORT`环境变量的值。

现在，我们可以通过运行以下命令来设置端口并同时运行我们的应用程序:

```
PORT=8080 node app.js
```

# 使用节点 Postgres 模块连接到 Postgres 数据库

要使用节点 Postgres 模块连接到 Postgres 数据库，我们可以使用`pg.connect`方法。

例如，我们可以写:

```
module.exports = {
  query(text, values, cb) {
    pg.connect((err, client, done) => {
      client.query(text, values, (err, result) => {
        done();
        cb(err, result);
      })
    });
  }
}
```

我们利用`pg.connect`方法连接到数据库。

然后我们使用数据库客户机对象`client`进行查询。

`text`有参数化查询。

`values`有用于查询的值。

当查询完成时，回调被调用。

`result`有了结果。

我们调用我们的`cb`回调，以便当我们调用`query`方法时可以获得结果。

# 在 Express 4 和 express-generator 的/bin/www 中使用 socket.io

我们可以通过将`io`对象添加到`app.js`文件中来使用 socket.io:

```
const express = require("express");
const socketIo = require("socket.io");
const app = express();
const io = socketIo();
app.io = io;io.on("connection", ( socket ) => {
  console.log('connected');
});module.exports = app;
```

我们将`io`对象附加为`app`的属性。

我们用`on`方法监听`'connection'`事件。

然后我们可以通过在`bin/www`中编写以下内容来使用它:

```
//...
const server = http.createServer(app);const io = app.io;
io.attach(server);
```

我们在调用`createServer`之前导入`app`对象，然后调用`io.attach`将 socket.io 监听器附加到 Express 应用程序。

然后在我们的路由文件中，我们可以写:

```
const app = require('express');module.exports = (io) => {
  const router = app.Router(); io.on('connection', (socket) => { 
    //...
  });
  return router;
}
```

然后我们将`app.js`改为:

```
const express = require("express");
const socketIo = require("socket.io");
const app = express();
const io = socketIo();
app.io = io;
io.on("connection", ( socket ) => {
  console.log('connected');
});const routes = require('./routes/index')(io);//...module.exports = app;
```

![](img/53ca7a12fd2e125d4997665007d199d3.png)

Photo by [Peyton](https://unsplash.com/@riley_shannon?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以从`insert`回调中获得插入的 MongoDB 文档的 ID。

我们可以通过将`io`对象附加到`app`来将 socket.io 合并到由 express-generator 生成的 Express 应用程序中。

此外，通过连接到数据库并使用创建连接后返回的客户端对象，我们可以将 Postgres 数据库与节点应用程序一起使用。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**