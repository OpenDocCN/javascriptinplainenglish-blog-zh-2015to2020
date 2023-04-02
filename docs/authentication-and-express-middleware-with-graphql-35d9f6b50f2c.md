# 使用 GraphQL 的认证和快速中间件

> 原文：<https://javascript.plainenglish.io/authentication-and-express-middleware-with-graphql-35d9f6b50f2c?source=collection_archive---------5----------------------->

![](img/b6de47d993435c922274b22eb9632d18.png)

Photo by [Mitch Lensink](https://unsplash.com/@lensinkmitchel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们可以用 Express 创建一个简单的 GraphQL 服务器。为此，我们需要`express-graphql`和`graphql`包。

在本文中，我们将研究如何在 Express GraphQL 中使用中间件。

# 快速中间件

如果我们使用`express-graphql`用 Express 构建我们的 GraphQL 服务器，我们可以像往常一样使用 Express 中间件。

在任何解析器中，`request`对象都可以作为第二个参数。

例如，如果我们想在解析器中获得请求的主机名，我们可以写:

```
const express = require('express');
const graphqlHTTP = require('express-graphql');
const { buildSchema } = require('graphql');const schema = buildSchema(`
  type Query {
    hostname: String
  }
`);const loggingMiddleware = (req, res, next) => {
  console.log(req.hostname);
  next();
}const root = {
  hostname(args, request) {    
    return request.hostname;
  }
};const app = express();
app.use(loggingMiddleware);
app.use('/graphql', graphqlHTTP({
  schema: schema,
  rootValue: root,
  graphiql: true,
}));
app.listen(3000, () => console.log('server started'));
```

在上面的代码中，我们像往常一样创建了模式来获取应用程序的主机名。

然后我们添加了我们的`loggingMiddleware`来记录主机名。它调用`next`，所以我们可以使用我们的`graphqlHTTP`中间件。

然后在我们的`root`解析器中，我们添加了一个`hostname`方法，它将`request`参数作为第二个实参，这个实参有 Express request 对象。

这是我们可以从`request`返回`hostname`属性的地方，这样我们就可以在响应中返回它。

这意味着我们可以像使用 REST APIs 一样继续使用中间件来处理身份验证。

Express 的常用认证选项包括 Passport、`express-jwt`和`express-session`。

# 证明

我们可以在 Express GraphQL 服务器中使用`jsonwebtoken` ,如下所示，通过 JSON web token 添加身份验证。

为此，我们首先通过运行以下命令来安装`jsonwebtoken`:

```
npm i jsonwebtoken
```

然后，我们可以将它包含在我们的应用程序中，然后添加到我们的 Express GraphQL 服务器，如下所示:

```
const express = require('express');
const graphqlHTTP = require('express-graphql');
const { buildSchema } = require('graphql');
const jwt = require('jsonwebtoken');
const unless = require('express-unless');const schema = buildSchema(`
  type Query {
    hostname: String
  }
`);const root = {
  hostname(args, request) {
    return request.hostname;
  }
};const verifyToken = (req, res, next) => {  
  jwt.verify(req.headers.authorization, 'secret', (err, decoded) => {
    if (err){      
      return res.send(401);
    }
    next();
  });
}
verifyToken.unless = unless;const app = express();
app.post('/auth', (req, res) => {
  const token = jwt.sign({ foo: 'bar' }, 'secret');
  res.send(token);
})app.use(verifyToken.unless({ path: ['/auth'] }));
app.use('/graphql', graphqlHTTP({
  schema: schema,
  rootValue: root,
  graphiql: true,
}));
app.listen(3000, () => console.log('server started'));
```

在上面的代码中，我们用`verifyToken`中间件来验证由`auth`路由发出的令牌。

在`verifyToken`中，我们调用`jwt.verify`来验证我们传递到`Authorization`头中的令牌。如果我们得到一个错误，那么我们发送一个 401 响应。

否则，我们调用`next`继续下一个中间件我们的路线。

在`auth`路由中，我们通过调用`jwt.sign`来发布我们的认证令牌，第一个参数是我们想要的任何内容，第二个参数是令牌的秘密。

此外，我们通过使用`express-unless`中间件将`verifyToken`中间件从`auth`路线中排除。

我们通过将`unless`分配给`verifyToken.unless`做到了这一点。

现在，当我们想要通过`/graphql`路由向 GraphQL 服务器发出请求时，我们必须将我们的 auth 令牌传递给`Authorization`头。

这使我们的 GraphQL 请求更加安全。然而，如果我们要在现实世界中使用 JSON web 令牌，我们应该有一个加密的秘密。

# 结论

我们可以使用 Express 中间件进行日志记录、身份验证或任何我们需要它们做的事情。

为了包含中间件，我们像往常一样调用`app.use`方法。

我们可以使用带有`express-unless`包的中间件来排除路由。

为了通过 JSON web 令牌添加身份验证，我们可以使用`jsonwebtoken`包为我们的 Express GraphQL 服务器添加令牌发布和验证功能。