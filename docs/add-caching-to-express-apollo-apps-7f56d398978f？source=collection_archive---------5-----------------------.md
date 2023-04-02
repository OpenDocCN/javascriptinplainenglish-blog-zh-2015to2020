# 向 Express Apollo 应用程序添加缓存

> 原文：<https://javascript.plainenglish.io/add-caching-to-express-apollo-apps-7f56d398978f?source=collection_archive---------5----------------------->

![](img/61e507bf3b86bda5a6ca7e57ef6c273d.png)

Photo by [Joshua Hoehne](https://unsplash.com/@mrthetrain?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Apollo 服务器作为节点包提供。我们可以用它来创建一个接受 GraphQL 请求的服务器。

在本文中，我们将看看如何添加缓存来提高我们的应用程序的性能。

# 定义缓存提示

Apollo Server 为我们提供了一种机制来控制各个类型和字段的缓存控制参数。

它可以用`@cacheControl`指令静态控制，也可以用`info.cacheControl.setCacheHint` API 动态控制。

# 在我们的模式中静态添加缓存提示

我们使用`@cacheControl`指令在模式中静态添加缓存提示，如下所示:

```
const express = require('express');
const { ApolloServer, gql } = require('apollo-server-express');
const books = [
  {
    title: 'JavaScript for Dummies',
    author: 'Jane Smith',
  },
  {
    title: 'JavaScript Book',
    author: 'Michael Smith',
  },
];
const typeDefs = gql`
  type Book @cacheControl(maxAge: 240){
    title: String
    author: String
  }
  type Query {
    books: [Book] @cacheControl(maxAge: 30)
  }
`;
const resolvers = {
  Query: {
    books: () => books,
  },
};
const app = express();
const server = new ApolloServer({ typeDefs, resolvers });
server.applyMiddleware({ app });
app.listen(3000, () => console.log('server started'));
```

在上面的代码中，我们将带有`maxAge`参数的`cacheControl`指令传递给了类型`Book`和字段`books`。

类型提示适用于返回该类型对象的所有字段。

缓存年龄以秒为单位。这意味着`Book`被缓存 240 秒，而`books`被缓存 30 秒。

# 在解析器中动态添加缓存提示

我们可以通过编写以下代码在解析器中添加动态缓存提示:

```
const express = require('express');
const { ApolloServer, gql } = require('apollo-server-express');
const books = [
  {
    title: 'JavaScript for Dummies',
    author: 'Jane Smith',
  },
  {
    title: 'JavaScript Book',
    author: 'Michael Smith',
  },
];
const typeDefs = gql`
  type Book @cacheControl(maxAge: 240){
    title: String
    author: String
  }
  type Query {
    books: [Book] @cacheControl(maxAge: 30)
  }
`;
const resolvers = {
  Query: {
    books: (parent, args, context, info) => {
      info.cacheControl.setCacheHint({ maxAge: 60, scope: 'PRIVATE' });
      return books;
    },
  },
};
const app = express();
const server = new ApolloServer({ typeDefs, resolvers });
server.applyMiddleware({ app });
app.listen(3000, () => console.log('server started'));
```

在上面的代码中，我们有`resolvers`的追随者值:

```
const resolvers = {
  Query: {
    books: (parent, args, context, info) => {
      info.cacheControl.setCacheHint({ maxAge: 60, scope: 'PRIVATE' });
      return books;
    },
  },
};
```

我们通过编写以下内容来设置缓存提示:

```
info.cacheControl.setCacheHint({ maxAge: 60, scope: 'PRIVATE' });
```

我们将查询结果设置为缓存 60 秒。

# 设置默认值`maxAge`

此外，我们可以通过在传递给`ApolloServer`构造函数的对象中添加`cacheControl.defaultMaxAge`属性来设置缓存的默认`maxAge`。

例如，我们可以写:

```
const express = require('express');
const { ApolloServer, gql } = require('apollo-server-express');
const books = [
  {
    title: 'JavaScript for Dummies',
    author: 'Jane Smith',
  },
  {
    title: 'JavaScript Book',
    author: 'Michael Smith',
  },
];
const typeDefs = gql`
  type Book @cacheControl(maxAge: 240){
    title: String
    author: String
  }
  type Query {
    books: [Book] @cacheControl(maxAge: 30)
  }
`;
const resolvers = {
  Query: {
    books: (parent, args, context, info) => {
      info.cacheControl.setCacheHint({ maxAge: 60, scope: 'PRIVATE' });
      return books;
    },
  },
};
const app = express();
const server = new ApolloServer({
  typeDefs, resolvers,
  cacheControl: {
    defaultMaxAge: 5,
  },
});
server.applyMiddleware({ app });
app.listen(3000, () => console.log('server started'));
```

我们有:

```
const server = new ApolloServer({
  typeDefs, resolvers,
  cacheControl: {
    defaultMaxAge: 5,
  },
});
```

将缓存的默认`maxAge`设置为 5 秒。

策略的`maxAge`是请求的所有字段中的最小值`maxAge`。

所有根字段和非标量字段的默认`maxAge`为 0。

![](img/6eaf72088dd3b3974478679fad779c0c.png)

Photo by [Joshua Hoehne](https://unsplash.com/@mrthetrain?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 将完整响应保存到缓存中

我们可以使用`apollo-server-plugin-response-cache`插件将完整的响应保存到缓存中。

例如，我们可以如下使用它:

```
const express = require('express');
const { ApolloServer, gql } = require('apollo-server-express');
const responseCachePlugin = require('apollo-server-plugin-response-cache');const books = [
  {
    title: 'JavaScript for Dummies',
    author: 'Jane Smith',
  },
  {
    title: 'JavaScript Book',
    author: 'Michael Smith',
  },
];
const typeDefs = gql`
  type Book{
    title: String
    author: String
  }
  type Query {
    books: [Book]
  }
`;
const resolvers = {
  Query: {
    books: () => books
  },
};
const app = express();
const server = new ApolloServer({
  typeDefs, resolvers,
  plugins: [responseCachePlugin()],
});
server.applyMiddleware({ app });
app.listen(3000, () => console.log('server started'));
```

我们要做的就是加上:

```
plugins: [responseCachePlugin()],
```

然后我们将启用缓存。

要为不同的用户分别缓存结果，我们可以编写以下代码:

```
const express = require('express');
const { ApolloServer, gql } = require('apollo-server-express');
const responseCachePlugin = require('apollo-server-plugin-response-cache');const books = [
  {
    title: 'JavaScript for Dummies',
    author: 'Jane Smith',
  },
  {
    title: 'JavaScript Book',
    author: 'Michael Smith',
  },
];
const typeDefs = gql`
  type Book @cacheControl(scope: PRIVATE){
    title: String
    author: String
  }
  type Query {
    books: [Book]
  }
`;
const resolvers = {
  Query: {
    books: () => books
  },
};
const app = express();
const server = new ApolloServer({
  typeDefs, resolvers,
  plugins: [responseCachePlugin({
    sessionId: (requestContext) => (requestContext.request.http.headers.get('sessionid') || null),
  })],
});
server.applyMiddleware({ app });
app.listen(3000, () => console.log('server started'));
```

在上面的代码中，我们添加了:

```
@cacheControl(scope: PRIVATE)
```

并且:

```
plugins: [responseCachePlugin({
    sessionId: (requestContext) => (requestContext.request.http.headers.get('sessionid') || null),
  })],
```

根据`sessionid`为用户单独缓存`Book`。

# 结论

我们可以用各种方式缓存项目。

为了在固定的时间内缓存类型和字段，我们可以使用`@cacheControl`指令。

除了在解析器中使用`info.cacheControl.setCacheHint`之外，我们还可以使用`responseCachePlugin`来缓存结果。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物: [**AI in Plain English**](https://medium.com/ai-in-plain-english) ，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****