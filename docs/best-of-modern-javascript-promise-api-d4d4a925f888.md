# 现代 JavaScript 的精华— Promise API

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-promise-api-d4d4a925f888?source=collection_archive---------1----------------------->

![](img/c36eb5067126232037b92ce4b4da395c.png)

Photo by [Obi Onyeador](https://unsplash.com/@thenewmalcolm?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究一些 JavaScript promise 方法。

# `finally`

`finally`方法让我们在承诺链中运行代码，而不管承诺链的结果如何。

例如，我们可以写:

```
asyncFn()
  .then(function(value1) {
    //...
  })
  .then(function(value2) {
    //...
  })
  .finally(function() {
    // ...
  });
```

我们调用`finally`,这样我们就可以运行一个回调函数，不管结果如何。

这对于运行清理代码之类的事情很有用。

# ES6 承诺库

我们可以使用许多有前途库。

其中许多都符合 ES6 API。

它们包括:

*   [ES6-Promises](https://github.com/jakearchibald/es6-promise) —小 ES6 promise polyfill
*   "[仅限原生承诺(NPO)](https://github.com/getify/native-promise-only) —小型 ES6 承诺多填充
*   [谎言](https://github.com/calvinmetcalf/lie)——小 ES6 无极聚划算
*   " [RSVP.js](https://github.com/tildeio/rsvp.js/) " —更大的承诺库
*   "[蓝鸟](https://github.com/petkaantonov/bluebird) " —更大的无极库
*   [Q.Promise](https://github.com/kriskowal/q#using-qpromise) —更大的 Promise 库
*   " [ES6 垫片](https://github.com/paulmillr/es6-shim) " —标准聚合填料
*   " [core-js](https://github.com/zloirock/core-js) " —标准聚合填料

# ES6 承诺 API

ES6 promise API 有不同的部分。

最基础的部分是`Promise`构造函数。

它接受一个回调，该回调将`resolve`和`reject`函数作为参数。

`resolve`用传入的值解析承诺。

如果`then`被调用，它将被转发给`then`回调函数。

`reject`用给定值拒绝承诺。

`reject`的参数通常是一个`Error`实例。

我们可以通过书写来使用它:

```
const p = new Promise(function(resolve, reject) {
  //...
});
```

# 静态`Promise`方法

API 中有几个静态承诺方法。

它们包括`Promise.resolve`和`Promise.reject`方法。

`Promise.resolve`将任意值转换为承诺。

我们向它传递一个参数，以返回一个带有解析值的值。

我们可以用以下方式调用`Promise.resolve`:

```
Promise.resolve(1)
```

`Promise.reject`让我们创建一个被拒绝的新承诺。

我们可以用:`Promise.reject`来称呼:

```
Promise.reject(1)
```

写承诺有几种方法。

`Promise.all`和`Promise.race`让我们以不同的方式兑现多个承诺。

`Promise.all`接受一个 iterable，如果承诺数组中的所有元素都被满足，则 iterable 被满足。

如果 iterable 中的任何承诺被拒绝，则拒绝值是承诺中的第一个拒绝值。

`Promise.race`也接受一个 iterable，它处理返回的承诺。

它将返回一个承诺，该承诺解析为第一个解析的值。

# `Promise Instance M`方法

Promise 实例有多种方法。

`Prototype.prototype.then(onFulfilled, onRejected)`方法接受一个`onFulfilled`和`onRejected`回调。

`onFulfilled`在解决承诺时立即调用。

`onRejected`当承诺被拒绝时调用。

`then`返回一个新的承诺，该承诺解析为承诺中返回的值。

如果`then`回调抛出异常，那么返回的承诺被拒绝。

如果`onFulfilled`被省略，则完成的结果被转发到下一个`then`。

如果`onRejected`被省略，则拒绝被转发到下一个`then`。

让我们抓住第一个被拒绝的承诺的错误。

和`p.then(null, onRejected)`一样。

![](img/b4c4fb05ec3b500e6b76afa439a16f78.png)

Photo by [Aniket Bhattacharya](https://unsplash.com/@aniket940518?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

Promise API 是一个标准 API，它让我们可以轻松地编写异步代码。

它们可以被链接起来，我们可以用它们来捕捉错误。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**