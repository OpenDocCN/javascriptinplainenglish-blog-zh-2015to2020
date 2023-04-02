# Node.js 最佳实践—错误处理

> 原文：<https://javascript.plainenglish.io/node-js-best-practices-error-handling-e121cff5e52a?source=collection_archive---------5----------------------->

![](img/e842f3838f6e35ceea9b23f782c0a2d1.png)

Photo by [Andrew Spencer](https://unsplash.com/@iam_aspencer?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# 处理错误

如果一段代码有可能出错，我们应该处理这个错误。

例如，如果我们改变了承诺，我们应该调用`catch`,并向其传递一个错误处理程序回调。

我们可以写:

```
doSomething()
  .then(doNextStage)
  .then(doMoreThins)
  .then(updateInterestedParties)
  .then(cleanUp)
  .catch(errorHandler);
```

我们有传递了`errorHandler`回调函数的`catch`方法。

当链中的任何承诺抛出错误时，将调用回调。

# 确保我们的应用程序自动重启

我们应该确保我们的应用程序自动重启。

这样，当我们的应用程序遇到未处理的错误时，我们就不必自己去做了。

我们需要一个像 Nodemon、forever 或 PM2 这样的流程管理器来做这件事。

最后两个用于生产用途。

Nodemon 更多的是用于开发。

例如，我们可以通过以下方式在全球范围内安装 PM2:

```
npm install pm2 -g
```

然后，我们可以启动我们的节点应用程序:

```
pm2 start app.js
```

# 将我们的应用集群化，以提高性能和可靠性

我们可以对我们的应用程序进行处理，这样我们就有了一个以上的实例。

通过这种方式，我们可以将工作分配给所有内核。

我们需要这样做，因为节点应用程序在单个进程中运行。

PM2 让我们通过跑步轻松做到这一点:

```
pm2 start app.js -i max
```

这使我们能够运行与服务器中存在的内核数量相等的多个进程。

这些进程不共享内存或资源。

例如，它们都有自己的数据库连接。

因此，我们应该使用 Redis 之类的东西来跨所有实例保持会话状态。

# 预先要求所有依赖关系

让我们的`require`调用都在顶部是个好主意，这样我们就不必在代码的各个部分寻找它们了。

此外，我们可以更早地发现他们的任何问题。

例如，我们可以写:

```
const datastore = require("db")(someConfig);app.get("/my-service", (request, response) => {
  db.get(req.query.someKey)
  // ...
});
```

我们在顶部调用`require`,这样我们可以更早地运行它们。

如果它们有任何错误，它们将在开始时被抛出。

此外，`require`是同步的，所以如果它需要很长时间运行，它会耽误我们的应用程序。

如果有这样的问题，如果我们早点要求他们，我们会早点发现他们。

# 使用日志库来增加错误的可见性

任何环境下都可能发生错误。

我们希望它们是可见的，这样我们就可以修复它们。

`console.log`在生产环境中非常有限。

一个更好的日志库可以让我们更好地控制如何记录日志。

我们可以有各种类型的日志记录或将数据保存在某个地方。

一些好的图书馆包括 Loggly 和 Winston。Winston 是基本日志记录的基础包。

Node-Loggly 使用 Winston 进行日志记录。

这样，我们将在一个可搜索的中心位置看到错误。

![](img/bf9a72f64fcb5d963cd10d7015370599.png)

Photo by [Matt Briney](https://unsplash.com/@mbriney?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

通过对我们的代码进行一些修改，可以使错误处理变得容易。

我们应该捕捉错误并记录下来，这样我们就可以修复它们。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**