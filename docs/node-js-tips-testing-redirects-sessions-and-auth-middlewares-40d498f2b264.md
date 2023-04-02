# Node.js 提示—测试重定向、会话和授权中间件

> 原文：<https://javascript.plainenglish.io/node-js-tips-testing-redirects-sessions-and-auth-middlewares-40d498f2b264?source=collection_archive---------3----------------------->

![](img/6448b24150d7c805fc94064a4bd9b183.png)

Photo by [Marc Sendra Martorell](https://unsplash.com/@marcsm?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 在 Express 中输入每条路线之前，使用中间件检查授权

我们可以在路由中添加一个中间件，在输入路由之前检查是否有正确的凭证。

例如，我们可以写:

```
const protectedMiddlewares = [authChecker, fetchUser];
const unprotectedMiddlewares = [trackVisistorCount];app.get("/", unprotectedMiddlewares, (req, res) => {
  //...
})app.get("/dashboard", protectedMiddlewares, (req, res) => {
  // ...
})app.get("/auth", unprotectedMiddlewares, (req, res) => {
  //...
})app.put("/auth", unprotectedMiddlewares, (req, res) => {
  //...
})
```

我们为受保护的`dashboard`路线提供了一系列中间件。

我们有一系列用于无保护路由的中间件。

中间件可能如下所示:

```
const authChecker = (req, res, next) => {
  if (req.session.auth || req.path === '/auth') {
    next();
  } else {
    res.redirect("/auth");
  }
}
```

我们有检查路由和会话路径的`authChecker`中间件。

如果有一个会话集，那么我们调用下一个中间件，这最终会导致一个受保护的路由处理器。

否则，我们改走`auth`路线。

# 测试在节点中使用 Mocha 和 Supertest 重定向的请求

要测试重定向到另一个路由的请求，并在 Mocha 测试运行程序中运行 Supertest，我们可以编写；

```
it('should log out the user', (done) => {
  request(app)
    .post('/login')
    .type('form')
    .field('user', 'username')
    .field('password', 'password')
    .end((err, res) => {
      if (err) { return done(err); }
      request(app)
        .get('/')
        .end((err, res) => {
          if (err) { return done(err); }
          res.text.should.include('profile');
          done();
        });
    });
});
```

我们通过检查第二个`end`回调中的响应文本来测试一个`login`路由的重定向。

为此，我们用用户名和密码发出了一个 POST 请求。

然后在重定向完成后调用回调。

我们可以从响应中获取文本，看看它是否是`profile`路线中的文本。

# 使用 Express 发送附加 HTTP 标头

我们可以用 response 对象的`setHeader`方法发送我们想要的任何响应头。

例如，我们可以写:

```
app.use((req, res, next) => {
  res.setHeader("Access-Control-Allow-Origin", "*");
  return next();
});
```

我们将`Access-Control-Allow-Origin`设置为`*`以允许来自所有来源的请求。

# 在 Express 中使用会话

为了在 Express 应用程序中处理会话，我们可以使用表达式中间件。

例如，我们可以这样使用它:

```
const express = require('express');
const session = require('express-session');const app = express();app.use(session({
  resave: false,
  saveUninitialized: false,
  secret: 'secret'
}));app.get('/', (req, res) => {
  if (req.session.views) {
    ++req.session.views;
  } else {
    req.session.views = 1;
  }
  res.send(`${req.session.views} views`);
});app.listen(3000);
```

我们使用了带有几个选项的`express-session`包。

`session`是一个函数，它返回一个中间件来让我们处理会话。

`resave`表示如果会话未被修改，我们不会保存会话，因为它是`false`。

`saveUninitialized`意味着如果是`false`，我们不会创建会话，直到有东西被存储。

`secret`是签名会话的秘密。

然后我们可以在`req.session`属性中存储我们想要的任何数据。

它会持续到会话结束。

我们只是在到达`/`路线时不断增加`views`计数。

缺省值是内存存储，只有在没有多个实例的情况下才应该使用它。

否则，我们需要使用持久会话。

# 使用 Socket.io 获取连接的客户端用户名列表

通过使用`clients`方法，我们可以获得连接到或未连接到名称空间的客户端列表。

例如，我们可以写:

```
const clients = io.sockets.adapter.rooms[roomId]; 
for (const clientId of Object.keys(clients)) {
  console.log(clientId); 
  const clientSocket = io.sockets.connected[clientId];
}
```

我们得到一个对象，用客户端 id 作为键，用`io.socket.adapter.rooms`对象。

`roomId`是我们所在房间的 ID。

然后我们可以用`io.socket.connected`对象为客户机获取套接字。

![](img/3997a92b895ee0d42cf4a3703b278e51.png)

Photo by [Jen Theodore](https://unsplash.com/@jentheodore?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用 Socket.io 得到客户端。

此外，我们可以编写自己的中间件来检查凭证，然后再继续路由。

express-session 允许我们在 express 应用程序中处理会话。

我们可以通过检查`end`回调中的响应内容，用 Supertest 测试重定向。

# 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**