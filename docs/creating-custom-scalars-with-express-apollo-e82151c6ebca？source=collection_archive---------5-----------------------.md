# 用 Express Apollo 创建自定义标量

> 原文：<https://javascript.plainenglish.io/creating-custom-scalars-with-express-apollo-e82151c6ebca?source=collection_archive---------5----------------------->

![](img/4272a86199836bc76b7d3f920c321cf6.png)

Photo by [Dean David](https://unsplash.com/@deandavid?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Apollo 服务器作为节点包提供。我们可以用它来创建一个接受 GraphQL 请求的服务器。

在本文中，我们将研究如何使用 Express Apollo Server 创建自定义标量类型。

# 自定义标量

要创建自定义标量，我们可以使用`scalar`关键字来创建它。

# 使用包

我们可以通过从现有的包中添加一个来添加自定义标量。

例如，我们可以通过使用`graphql-type-json`包添加一个 JSON 标量类型。

那么我们可以如下使用它:

```
const express = require('express');
const { ApolloServer, gql } = require('apollo-server-express');
const GraphQLJSON = require('graphql-type-json');const typeDefs = gql`
  scalar JSON type Foo {
    aField: JSON
  } type Query {
    foo: Foo
  }
`;const resolvers = {
  JSON: GraphQLJSON,
  Query: {
    foo() {
      return { aField: { foo: 'bar' } };
    }
  }
};const app = express();
const server = new ApolloServer({ typeDefs, resolvers });
server.applyMiddleware({ app });app.listen(3000, () => console.log('server started'));
```

在上面的代码中，我们有:

```
const typeDefs = gql`
  scalar JSON type Foo {
    aField: JSON
  } type Query {
    foo: Foo
  }
`;
```

定义标量 JSON 类型的。然后我们创建了一个有`aField`字段的`Foo`类型，然后我们有一个`Query`类型，带有一个返回`Foo`对象的`foo`查询。

然后我们创建一个`resolvers`对象，如下所示:

```
const resolvers = {
  JSON: GraphQLJSON,
  Query: {
    foo() {
      return { aField: { foo: 'bar' } };
    }
  }
};
```

我们有解析为`GraphQLJSON`类型的`JSON`解析和返回`Foo`对象的`foo`查询。

然后，当我们在浏览器中转至`/graphql`并执行以下查询时:

```
{
  foo {
    aField 
  }
}
```

我们回来了:

```
{
  "data": {
    "foo": {
      "aField": {
        "foo": "bar"
      }
    }
  }
}
```

作为回应。

![](img/16e6bf2bfcb8992cde215fc640d61eb9.png)

Photo by [Caglar Araz](https://unsplash.com/@lelacag?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 自定义`GraphQLScalarType`实例

我们也可以使用`GraphQLScalarType`构造函数来创建我们自己的标量类型。

例如，我们可以这样写:

```
const express = require('express');
const { ApolloServer, gql } = require('apollo-server-express');
const { GraphQLScalarType, Kind } = require('graphql');const CustomScalarType = new GraphQLScalarType({
  name: 'CustomScalar',
  description: 'Description',
  serialize(value) {
    return value;
  },
  parseValue(value) {
    return value;
  },
  parseLiteral(ast) {
    switch (ast.kind) {
      case Kind.Int:
        return ast.value.toString();
      default:
        return ast.value
    }
  }
});const typeDefs = gql`
  scalar CustomScalar type Foo {
    aField: CustomScalar
  } type Query {
    foo: Foo
  }
`;const resolvers = {
  CustomScalar: CustomScalarType,
  Query: {
    foo() {
      return { aField: 123 };
    }
  }
};const app = express();
const server = new ApolloServer({ typeDefs, resolvers });
server.applyMiddleware({ app });app.listen(3000, () => console.log('server started'));
```

在上面的代码中，我们编写了自己的标量类型`CustomScalar`:

```
const CustomScalarType = new GraphQLScalarType({
  name: 'CustomScalar',
  description: 'Description',
  serialize(value) {
    return value;
  },
  parseValue(value) {
    return value;
  },
  parseLiteral(ast) {
    switch (ast.kind) {
      case Kind.Int:
        return ast.value.toString();
      default:
        return ast.value
    }
  }
});
```

我们用`name`属性来命名我们的标量类型，用`description`来描述我们的类型。

然后我们添加了一些方法来用`serialize`方法序列化类型。它通过转换返回我们想要序列化给定的`value`的任何内容。

为了解析标量的值，我们添加了`parseValue`方法。我们可以随心所欲地解析`value`参数。

`parseLiteral`方法接受一个`ast`对象，它是抽象语法树。它为我们获取被解析的对象类型的`kind`属性。我们也解析树结构并返回我们想要的。

然后我们用自定义标量类型创建`typeDefs`常量，如下所示:

```
const typeDefs = gql`
  scalar CustomScalar type Foo {
    aField: CustomScalar
  } type Query {
    foo: Foo
  }
`;
```

然后我们创建了下面的`resolvers`:

```
const resolvers = {
  CustomScalar: CustomScalarType,
  Query: {
    foo() {
      return { aField: 123 };
    }
  }
};
```

通过将自定义标量类型映射到`CustomScalar` resolve 来使用它。然后我们创建一个`Query`解析器来返回我们想要的对象，它的类型是`Foo`。`Foo`有一个`aField`字段，这是我们的`CustomScalar`类型。

然后，当我们转到`/graphql`页面时，我们可以运行以下查询:

```
{
  foo {
    aField 
  }
}
```

我们得到了:

```
{
  "data": {
    "foo": {
      "aField": 123
    }
  }
}
```

# 结论

我们可以用 GraphQL 创建定制的标量类型。这让我们可以按照自己喜欢的方式序列化和解析值。

为了创建一个，我们用关键字`scalar`创建我们的类型定义，以表明我们正在创建一个标量类型。

然后，我们使用`GraphQLCustomScalarType`构造函数，并为一个对象提供解析和序列化值的方法以及我们的类型名称。

接下来，我们创建我们的`resolvers`，它包括我们的定制类型和查询数据的查询类型。

此外，我们可以从定制包中获得一个。

最后，我们可以使用自定义类型进行查询。

## **用简单的英语写的 JavaScript 的注释:**

我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的 Medium 用户名给我们发邮件到[submissions@javascriptinplainenglish.com](mailto:submissions@javascriptinplainenglish.com)，我们会把你添加为作者。