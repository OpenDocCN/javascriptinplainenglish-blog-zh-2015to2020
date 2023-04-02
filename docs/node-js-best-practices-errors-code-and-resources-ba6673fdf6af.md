# Node.js 最佳实践—错误、代码和资源

> 原文：<https://javascript.plainenglish.io/node-js-best-practices-errors-code-and-resources-ba6673fdf6af?source=collection_archive---------7----------------------->

![](img/9836806d8e760596aa322bfae14c4005.png)

Photo by [Scott Webb](https://unsplash.com/@scottwebb?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# 正确处理错误和异常

我们应该在我们的 Express 应用程序上正确处理错误和异常。

在同步代码或异步函数中，我们可以使用 try-catch:

```
async function foo() {
  try {
    const baz = await bar()
    return baz
  } catch (err) {
    console.error(err);
  }
}
```

如果我们想捕捉中间件中的错误，我们可以创建自己的中间件来捕捉错误。

例如，我们可以写:

```
function errorHandler(err, req, res, next) {
  console.error(err)
  res.status(err.status || 500).send(err.message)
}router.use(errorHandler)
```

我们有`errorHandler`中间件来捕捉在它之前添加的其他中间件的错误。

然后我们可以用`errorHandler`调用`router.use`或`app.use`来处理错误。

# 小心内存泄漏

我们应该注意内存泄漏，这样我们的应用程序才不会耗尽内存。

如果持续增加内存使用量，那就不好了。

这意味着应用程序一直在使用内存。

有像 [Sematext Agent Express](https://github.com/sematext/sematext-agent-express#configure-agent) 模块这样的应用程序，可以让我们观察应用程序的 CPU 和内存使用情况。

借助该产品包，我们可以将其与以下产品集成:

```
const { stMonitor, stLogger, stHttpLoggerMiddleware } =
require('sematext-agent-express')
stMonitor.start() const express = require('express')
const app = express()
app.use(stHttpLoggerMiddleware)
```

当我们的应用程序启动时，我们只需调用`stMonitor.start()`开始用 Sematext 进行监控。

Scout 等其他工具也向我们展示了每行代码的内存使用情况。

我们可以看到哪些使用了更多的内存，以及使用更多内存的次数。

这些还可以监控每个请求的性能问题。

# Java Script 语言

我们可以改进 Express 应用程序的 JavaScript 代码，使其更易于测试和维护。

# 纯函数

纯函数是让 let 返回某个东西，不改变任何外部状态的函数。

如果我们向它们传递相同的参数，那么它们将总是返回相同的值。

这使得他们的行为可以预测，简化了每个人的生活。

我们可以创建新的对象，而不是用 pie 函数改变现有的对象。

JavaScript 中纯函数的一些例子包括数组实例的`map`和`filter`方法等等。

# 对象参数

为了使用参数，我们应该减少函数中的参数数量。

一个简单的方法是给我们的函数添加一个对象参数。

然后我们可以使用析构将属性析构为变量。

这样，我们就不必担心参数的顺序。

例如，我们可以写:

```
const foo = ({ a, b, c }) => {
  const sum = a + b + c;
  return sum;
}
```

我们只需将`a`、`b`和`c`参数作为变量使用。

# 编写测试

测试对于捕捉回归非常有用。

这样，如果我们改变我们的代码，我们可以放心，如果现有的测试通过，我们没有杀死任何现有的代码。

用 JavaScript，有很多测试框架，包括 Mocha，Chai，Jasmine，Jest。

我们可以用它们中的任何一个来进行测试。

有了柴，我们可以写:

```
const chai = require('chai')
const expect = chai.expectconst foo = require('./src/foo')describe('foo', function () {
  it('should be a function', function () {
    expect(foo).to.be.a('function')
  })
})
```

导入`foo`文件并用 Chai 对其进行测试。

![](img/ae797275ff40384a498be1c5f3af23ba.png)

Photo by [Alex Machado](https://unsplash.com/@alexmachado?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该适当地处理异常。

此外，我们可以使用测试来防止错误。

纯函数和对象参数也有助于我们编写更干净的代码。

## **简明英语 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**