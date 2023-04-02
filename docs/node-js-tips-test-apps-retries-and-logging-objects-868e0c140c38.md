# Node.js 提示—测试应用程序、重试和记录对象

> 原文：<https://javascript.plainenglish.io/node-js-tips-test-apps-retries-and-logging-objects-868e0c140c38?source=collection_archive---------9----------------------->

![](img/39153b7b07f6f37efa6ed20293c04bad.png)

Photo by [Antonio Grosz](https://unsplash.com/@angro?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 如何检查路径是绝对的还是相对的

我们可以用`path`模块的`isAbsolute`方法检查路径是否是绝对的。

例如，我们可以写:

```
const path = require('path');
//...if (path.isAbsolute(somePath)) {
  //...
}
```

我们通过`somePath`中地方法来检查它。

如果是绝对路径，它返回`true`，否则返回`false`。

# 在 Winston 中记录 JavaScript 对象和数组

我们可以通过设置`prettyPrint`属性来用 Winston 美化对象。

例如，我们可以写:

```
const level = 'debug';const logger = new winston.Logger({
  transports: [
    new winston.transports.Console({
      name: 'debug-console',
      level,
      prettyPrint(object) {
        return JSON.stringify(object);
      },
      handleExceptions: true,
      json: false,
      colorize: true
    })
  ],
  exitOnError: false
});
```

我们将`prettyPrint`设置为一个函数，该函数返回 stringified 对象来打印对象内容。

此外，我们可以用`%j`标签记录 JSON。

例如，我们可以写:

```
logger.log("info", "an object %j", obj);
```

其中`obj`是一个对象。

# 在 Mocha 的每个钩子之后从内部检测测试失败

我们可以检查`afterEach`钩子中的`this.currentTest.state`属性来检查刚刚运行的测试的状态。

例如，我们可以写:

```
afterEach(function() {
  if (this.currentTest.state === 'failed') {
    // ...
  }
});
```

如果`state`是`'failed'`，那么它失败了。

# 在 Node.js 中设置 Cookie 值

我们可以用`res.cookie`方法在响应中设置一个 cookie。

例如，我们可以写:

```
const cookieParser = require('cookie-parser');
//...app.use(cookieParser());app.get('/foo', (req, res) => {
  res.cookie('foo', 'baz');
});
```

第一个参数是键，第二个是值。

我们还包括`cookieParser`。

然后我们可以传入一个秘密作为参数来签署 cookie。

例如，我们可以写:

```
const cookieParser = require('cookie-parser');
//...app.use(cookieParser('secret'));app.get('/foo', (req, res) => {
  res.cookie('foo', 'baz');
});
```

我们传入一个秘密的密钥串来签署一个 cookie。

# 如何对快速路由器路由进行单元测试

我们可以通过导出 Express 应用程序对 Express 路由器路由进行单元测试。

然后我们可以通过将导出的 app 传入 Supertest 的`request`函数来使用 Supertest。

例如，如果我们有以下快速应用程序:

`app.js`

```
const app = express();
// ...
const router = require('../app/router')(app);module.exports = app;
```

然后我们可以通过编写以下代码来编写我们的测试:

```
const chai = require('chai');
const should = chai.should();
const sinon = require('sinon');
const request = require('supertest');
const app = require('app');describe('person route', () => {
  request(app)
    .get('/api/persons')
    .expect('Content-Type', /json/)
    .expect('Content-Length', '4')
    .expect(200, "ok")
    .end((err, res) => {
      if (err) throw err;
    });
});
```

我们传入从`app.js`导入的`app`。

然后我们通过调用`get`发出 GET 请求。

我们使用`expect`来检查响应头和状态代码。

然后我们调用`end`来完成请求，并在回调中得到带有`res`的响应。

# 重复请求，直到一个成功，没有阻塞

为了重复请求直到成功，我们可以使用`async.retry`方法。

例如，我们可以写:

```
const async = require('async');
const axios = require('axios');const makeRequest = async (uri, callback) => {
  try {
    const result = await axios.get(uri);
    callback(null, result);
  } catch (err) {
    callback(err);
  }
};const uri = 'http://www.test.com/api';async.retry({
    times: 5,
    interval: 200
  },
  (callback) => {
    return makeRequest(uri, callback)
  },
  (err, result) => {
    if (err) {
      throw err; 
    }
  });
```

我们创建了`makeRequest`函数，它在完成后调用`callback`。

然后我们将它传递给带有选项的`async.retry`方法。

`times`是尝试的次数。

`interval`具有以毫秒为单位的重试间隔。

第二个参数是要调用的异步函数。

最后一个参数是请求成功或重试次数用尽时的回调。

`result`有结果了。`err`有误差。

![](img/e6f6074531160b3f512a29497ec289b4.png)

Photo by [Dan Muir](https://unsplash.com/@danonline1995?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用`async.retry`方法来重试异步函数。

我们可以打印出用 Winston 记录的 JavaScript 对象。

要使用 Supertest 测试 Express 应用程序，我们必须导出 Express 应用程序并将其用于 Supertest。

Express 可以在响应中设置 cookies。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**