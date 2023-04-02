# 使用 Express Apollo 创建和使用联合类型

> 原文：<https://javascript.plainenglish.io/creating-and-using-union-types-with-express-apollo-68c5be1f008a?source=collection_archive---------9----------------------->

![](img/6a9b223c05faccfd35ca5204dd76b488.png)

Photo by [Micheile Henderson @micheile010 // Visual Stories [nl]](https://unsplash.com/@micheile?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Apollo 服务器作为节点包提供。我们可以用它来创建一个接受 GraphQL 请求的服务器。

在本文中，我们将研究如何创建联合类型并在 Apollo Server 和 Express 中使用它们。

# 联合类型

联合类型表示一个字段可以返回多个对象类型，但它本身并不指定特定的字段。

这对于从单个字段返回不相交的数据类型很有用。

例如，我们可以定义一个联合类型并按如下方式使用它:

```
const express = require('express');
const { ApolloServer, gql } = require('apollo-server-express');const data = {
  '1': { title: 'JavaScript for Dummies' },
  '2': { name: 'Jane Smith' },
}const typeDefs = gql`
  union Result = Book | Author type Book {
    title: String
  } type Author {
    name: String
  } type Query {
    search(id: Int!): Result
  }
`;const resolvers = {
  Result: {
    __resolveType(obj, context, info) {
      if (obj.name) {
        return 'Author';
      } if (obj.title) {
        return 'Book';
      } return null;
    },
  },
  Query: {
    search: (_, { id }) => data[id]
  },
};const app = express();
const server = new ApolloServer({ typeDefs, resolvers });
server.applyMiddleware({ app });app.listen(3000, () => console.log('server started'));
```

在上面的代码中，我们用我们将在解析器中返回的数据定义了如下的`data`常量:

```
const data = {
  '1': { title: 'JavaScript for Dummies' },
  '2': { name: 'Jane Smith' },
}
```

然后我们定义我们的类型定义如下:

```
const typeDefs = gql`
  union Result = Book | Author type Book {
    title: String
  } type Author {
    name: String
  } type Query {
    search(id: Int!): Result
  }
`;
```

在上面的代码中，我们使用了`union`关键字来表示联合类型。它被定义为与`Author`型结合的`Book`型。

然后我们像往常一样定义了`Book`和`Author`类型。

最后，我们用`search`查询定义了我们的`Query`类型，它接受一个`id`整数并返回我们上面指定的`Result`类型。

然后在我们的`resolvers`对象中，我们定义了如何将`Result`类型解析为如下类型:

```
const resolvers = {
  Result: {
    __resolveType(obj, context, info) {
      if (obj.name) {
        return 'Author';
      } if (obj.title) {
        return 'Book';
      } return null;
    },
  },
  Query: {
    search: (_, { id }) => data[id]
  },
};
```

在`Result`中，我们有`__resolveType`方法，该方法有`obj`对象，该对象有我们返回给用户的对象。

在那里，我们可以像以前一样检查属性，并相应地返回类型名。

在我们的`Query`对象中，我们有`search`方法来返回数据。

要对联合类型进行查询，我们必须使用内联片段，如下所示，方法是转到`/graphql`页面并键入以下内容:

```
{
  search(id: 1) {
    __typename
    ...on Book {
      title
    }
    ...on Author {
      name
    }
  }
}
```

`...on`表示我们有一个内联片段。我们可以使用它根据返回的内容对字段进行查询。`__typename`字段返回对象的类型名。

那么我们应该得到以下响应:

```
{
  "data": {
    "search": {
      "__typename": "Book",
      "title": "JavaScript for Dummies"
    }
  }
}
```

这是我们定义的`data`对象中的内容。

![](img/411d6b715747cd3148df7017ac868bc3.png)

Photo by [Meriç Dağlı](https://unsplash.com/@meric?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以定义一个联合类型来创建一个不相交的数据类型。

`|`符号和`union`关键字表明一个类型是联合类型。

在联合中结合在一起的类型是普通对象和标量类型。

在 resolvers 对象中，我们用`__resolveType`方法返回给定对象结构的类型名。

要进行返回联合类型的查询，我们必须使用内联片段。我们可以使用`__typename`操作符来获取返回对象的类型名。

【JavaScript 用简单英语写的一句话:我们总是乐于帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到 submissions@javascriptinplainenglish.com[给我们，我们会把你添加为作者。](mailto:submissions@javascriptinplainenglish.com)