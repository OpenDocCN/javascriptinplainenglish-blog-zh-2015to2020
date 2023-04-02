# 使用 Express 和 Apollo Server 创建和使用数据源

> 原文：<https://javascript.plainenglish.io/creating-and-using-data-sources-with-express-and-apollo-server-d3c84758b135?source=collection_archive---------8----------------------->

![](img/45f0884301fd4e5b3761d5d9e8b5e16b.png)

Photo by [Jeremiah Lawrence](https://unsplash.com/@jrlawrence?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Apollo 服务器作为节点包提供。我们可以用它来创建一个接受 GraphQL 请求的服务器。

在本文中，我们将研究如何创建 REST 数据源来从外部数据源获取数据。

# REST 数据源

数据源是封装从服务获取数据的类，内置了对缓存、重复数据删除和错误处理的支持。

我们可以用它来与我们的后端交互，剩下的工作由 Apollo Server 来完成。

创建和使用`RESTDataSource`的一个简单例子如下:

```
const express = require('express');
const { ApolloServer, gql, SchemaDirectiveVisitor } = require('apollo-server-express');
const { RESTDataSource } = require('apollo-datasource-rest');class JokesAPI extends RESTDataSource {
  constructor() {
    super();
    this.baseUrl = 'https://api.icndb.com/';
  } async getRandomJoke() {
    return this.get(`${this.baseUrl}/jokes/random`)
  }
}const typeDefs = gql`
  type Joke {
    id: Int,
    joke: String
  } type JokeResponse {
    value: Joke    
  } type Query {
    joke: JokeResponse
  }
`;const resolvers = {
  Query: {
    async joke(parent, args, { dataSources }) {
      return dataSources.jokesAPI.getRandomJoke();
    },
  },
};const app = express();
const server = new ApolloServer({
  typeDefs, resolvers,
  dataSources: () => {
    return {
      jokesAPI: new JokesAPI(),
    };
  },
});
server.applyMiddleware({ app });app.listen(3000, () => console.log('server started'));
```

在上面的例子中，我们有`JokesAPI`数据源:

```
class JokesAPI extends RESTDataSource {
  constructor() {
    super();
    this.baseUrl = 'https://api.icndb.com/';
  } async getRandomJoke() {
    return this.get(`${this.baseUrl}/jokes/random`)
  }
}
```

在那里，我们有一个`getMovie`方法，它用电影对象返回一个承诺。

由`RESTDataSource`提供的`get`方法让我们向服务器发出 GET 请求。

然后，我们将类型定义如下:

```
const typeDefs = gql`
  type Joke {
    id: Int,
    joke: String
  } type JokeResponse {
    value: Joke    
  } type Query {
    joke: JokeResponse
  }
`;
```

我们有一个简单的带有字符串字段的类型。然后，我们将数据源添加到我们的服务器，它将在解析器的第三个参数中可用，如下所示:

```
const server = new ApolloServer({
  typeDefs, resolvers,
  dataSources: () => {
    return {
      moviesAPI: new MoviesAPI(),
    };
  },
});
```

最后，我们通过编写以下代码来添加解析器:

```
const resolvers = {
  Query: {
    async joke(parent, args, { dataSources }) {
      return dataSources.jokesAPI.getRandomJoke();
    },
  },
};
```

返回电影。`dataSources`就是我们传递给`ApolloServer`构造函数的那个。

然后，当我们进行以下查询时:

```
{
  joke {
    value {
      joke
    }
  }
}
```

我们得到:

```
{
  "data": {
    "joke": {
      "value": {
        "joke": "Chuck Norris does not code in cycles, he codes in strikes."
      }
    }
  }
}
```

作为回应。

![](img/3fd1fc5dd2961dc559fd7d7169cd12ff.png)

Photo by [Hrithik Bachchas](https://unsplash.com/@iamhrithikb?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# HTTP 请求

我们可以创建对服务器进行 HTTP 的方法。我们可以使用`this.post`发出 POST 请求，`this.put`发出 PUT 请求，`this.patch`发出 PATCH 请求，`this.delete`发出 DELETE 请求。

第一个参数是 URL，第二个参数是有效负载。

例如，我们可以像下面的`postTodo` 方法一样发出 POST 请求:

```
class TodosAPI extends RESTDataSource {
  constructor() {
    super();
    this.baseUrl = 'https://jsonplaceholder.typicode.com/';
  } async postTodo(todo) {        
    return this.post(`${this.baseUrl}/todos`, todo)
  }
}
```

# 拦截提取

我们可以添加`willSendRequest`钩子，在将请求发送到服务器之前拦截它们。

例如，我们可以如下使用它:

```
const express = require('express');
const { ApolloServer, gql, SchemaDirectiveVisitor } = require('apollo-server-express');
const { RESTDataSource } = require('apollo-datasource-rest');class JokesAPI extends RESTDataSource {
  constructor() {
    super();
    this.baseUrl = 'https://api.icndb.com/';
  } willSendRequest(request) {
    request.headers.set('Authorization', this.context.token);
  } async getRandomJoke() {
    return this.get(`${this.baseUrl}/jokes/random`)
  }
}const typeDefs = gql`
  type Joke {
    id: Int,
    joke: String
  } type JokeResponse {
    value: Joke    
  } type Query {
    joke: JokeResponse
  }
`;const resolvers = {
  Query: {
    async joke(parent, args, { dataSources }) {
      return dataSources.jokesAPI.getRandomJoke();
    },
  },
};const app = express();
const server = new ApolloServer({
  typeDefs, resolvers,
  dataSources: () => {
    return {
      jokesAPI: new JokesAPI(),
    };
  },
  context: () => ({ token: 'foo' })
});
server.applyMiddleware({ app });app.listen(3000, () => console.log('server started'));
```

在上面的代码中，我们返回了令牌，即:

```
context: () => ({ token: 'foo' })
```

然后在`willSendRequest`方法中，我们可以如下调用`request.headers.set`来附加我们附加到请求的`token`,如下所示:

```
willSendRequest(request) {
  request.headers.set('Authorization', this.context.token);
}
```

`this.context`我们返回的对象是:

```
context: () => ({ token: 'foo' })
```

其余与第一个例子相同。

# 动态解析 URL

我们可以通过在我们的`RESTDataSource`类中为 URL 添加一个 getter 来设置请求的基本 URL，如下所示:

```
get baseURL() {
  if (this.context.env === 'development') {
    return 'http://api.icndb.com/jokes/random';
  }
}
```

# 结论

我们可以创建自己的类来扩展`RESTDataSource`类，向其他服务器发出请求以获取和设置数据。

它有发出 HTTP 请求的方法，并且可以访问我们服务器的`context`来获取各种数据。

如果我们在传递给`ApolloServer`构造函数的对象中设置了`dataSources`属性，我们就可以访问这个类的实例。

我们可以通过添加一个`willSendRequest`方法来添加标题，该方法有一个`request`对象，在这里我们可以调用`headers`属性上的`set`。

## **用简单英语写的 JavaScript 笔记**

我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到[**submissions@javascriptinplainenglish.com**](mailto:submissions@javascriptinplainenglish.com)给我们，我们会把你添加为作者。

我们还推出了三种新的出版物！为我们的新出版物献上一点爱心吧，请跟随他们:[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**