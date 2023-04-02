# Node.js 提示—关闭连接、模板和日期

> 原文：<https://javascript.plainenglish.io/node-js-tips-closing-connections-templates-and-dates-c170e365a6df?source=collection_archive---------9----------------------->

![](img/4d9d4335490f5f4ba414ad51a4478f0d.png)

Photo by [Alex Russell-Saw](https://unsplash.com/@alexrussellsaw?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 使用 Mongoose 的 2 个字段进行自定义验证

为了在 Mongoose 中使用 2 个字段进行自定义验证，我们可以创建一个自定义模式和一个验证函数，并将其作为对象中字段的值传递给`Schema`构造函数。

例如，我们可以写:

```
const dateRangeSchema = {
  startDate: { type: Date },
  endDate: { type: Date }
};const checkDates = (value) => {
  return value.endDate < value.startDate; 
}const schema = new Schema({
  dateRange: { type: dateRangeSchema, validate: checkDates }
});
```

我们创建了带有`startDate`和`endDate`字段的`dateRangeSchema`。

此外，我们创建了 w `checkDates`函数来返回日期范围。

然后我们将两者都传递给对象来指定字段的数据类型。

我们还可以通过编写以下代码向`validate`传递回调:

```
schema.pre('validate', function(next) {
  if (this.startDate > this.endDate) {
    next(new Error('start date is after the end date'));
  } else {
    next();
  }
});
```

我们从模型中获取`startDate`和`endDate`值，如果开始日期大于结束日期，则调用`next`时出错。

否则，我们调用`next`,什么也不做，照常进行。

# 格式化来自 MongoDB 的日期

我们可以用`toDateString`方法格式化来自 MongoDB 的日期。

例如，我们可以写:

```
Schema
  .virtual('date')
  .get(function() {
    return this._id.generationTime.toDateString();
  });
```

我们也可以使用 moment.js 库，让我们的生活变得更加轻松。

例如，我们可以写:

```
Schema
  .virtual('date')
  .get(function() {
    return moment(this._id.generationTime).format("YYYY-MM-DD HH:mm");
  });
```

我们调用了`format`方法，将从`moment`函数创建的 moment 对象格式化为我们想要的日期格式。

它以字符串的形式返回。

# Jade/Pug 模板中类名的变量

我们可以通过编写以下代码将字符串插入到 Jade/Pug 模板中:

```
div(class="language-#{session.language}")
```

# 在 Express 应用程序中监听所有接口，而不是仅监听本地主机

我们通过传入`'0.0.0.0'`作为`listen`的第二个参数来监听 Express 应用程序中的所有接口。

例如，我们可以写:

```
const express = require('express');
const app = express();
app.listen(3000, '0.0.0.0');
```

# 在 Javascript 逻辑中访问 EJS 变量

如果我们有一个呈现 EJS 模板的路径，我们可以呈现从`render`方法传入的变量。

例如，我们可以写:

```
app.get("/post/:title, (req, res) => {
  //...
  res.render("post", { title, description });
}
```

为了创造我们的路线，

然后在我们的 EJS 模板中，我们可以写:

```
<% if (title) { %>
     <h2>Post</h2>
     <script>
        const postTitle = <%= title %>            
     </script>
<% } %>
```

我们从第二个参数中的对象获得`title`。

然后我们将`title`插入到模板中。

现在我们可以把它赋给一个变量。

# 结束 Express.js POST 响应

我们调用`res.end`结束响应来结束响应。

例如，我们可以写:

```
res.end('hello');
```

或者我们可以渲染一个模板，然后调用`res.end`:

```
res.render('some.template');
res.end();
```

# 当进程被终止时，正常关闭 Express 服务器

我们可以监听`SIGTERM`和`SIGINT`信号，然后在这些信号的处理程序中运行我们的代码，并在回调中关闭服务器。

例如，我们可以写:

```
const express = require('express');const app = express();app.get('/', (req, res) => res.send('hello'});const server = app.listen(3000);setInterval(() => server.getConnections((err, connections) =>  {
  console.log(`${connections} connections currently open`)
}), 1000);process.on('SIGTERM', shutDown);
process.on('SIGINT', shutDown);let connections = [];server.on('connection', connection => {
  connections.push(connection);
  connection.on('close', () => {
    connections = connections.filter(curr => curr !== connection)
  });
});const shutDown = () => {
  server.close(() => {
    process.exit(0);
  }); setTimeout(() => {
    process.exit(1);
  }, 10000); connections.forEach(curr => curr.end());
  setTimeout(() => connections.forEach(curr => curr.destroy()), 5000);
}
```

我们用`process.on`方法收听`SIGTERM`和`SIGINT`信号。

我们传入`shutDown`函数，在这里我们调用`server.close`来关闭连接。

如果连接没有成功关闭，我们也会在 10 秒后退出，代码为 1。

连接存储在`connections`数组中，并调用`end`关闭每个连接。

我们也调用`destroy`在 5 秒钟后破坏连接。

连接来自于`connection`事件处理程序，在那里我们将新的连接对象推入到`connections`数组中。

我们还用`filter`方法添加了`close`处理程序。

![](img/5bd787244ada98b02b785a4da7703c91.png)

Photo by [Koushik Chowdavarapu](https://unsplash.com/@koushikc?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用 Mongoose 同时验证多个字段。

此外，我们可以优雅地关闭连接。

我们还可以格式化来自 MongoDB 的日期。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**