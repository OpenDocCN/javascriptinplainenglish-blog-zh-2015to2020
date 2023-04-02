# 使用 Express Apollo 创建枚举类型

> 原文：<https://javascript.plainenglish.io/creating-enum-types-with-express-apollo-6c59665e95d4?source=collection_archive---------3----------------------->

![](img/c189d027090adce4ec67569589fb1055.png)

Photo by [Micheile Henderson @micheile010 // Visual Stories [nl]](https://unsplash.com/@micheile?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Apollo Server 可用作节点软件包。我们可以用它来创建一个接受 GraphQL 请求的服务器。

在本文中，我们将了解如何使用 Express Apollo 创建枚举类型。

# Enums

枚举类似于标量类型，但是它们只能采用模式中定义的一些可能的值。

当我们必须在几个选项中做出选择时，它们很有用。它还为我们提供了 GraphQL 游乐场等工具中的自动完成功能。

例如，我们可以定义如下:

```
const express = require('express');
const { ApolloServer, gql } = require('apollo-server-express');const typeDefs = gql`
  enum AllowedColor {
    RED
    GREEN
    BLUE
  } type Query {
    pickColor(color: AllowedColor): String
  }
`;const resolvers = {
  Query: {
    pickColor(_, { color }) {
      return color;
    }
  }
};const app = express();
const server = new ApolloServer({ typeDefs, resolvers });
server.applyMiddleware({ app });app.listen(3000, () => console.log('server started'));
```

在上面的代码中，我们用可能值`RED`、`GREEN` 或`BLUE`创建了`AllowedColor`枚举类型。

然后我们添加了带有`pickColor`查询的`Query`类型，该查询接受一个`color`参数并返回一个`String`。

我们这样做是为了:

```
const typeDefs = gql`
  enum AllowedColor {
    RED
    GREEN
    BLUE
  } type Query {
    pickColor(color: AllowedColor): String
  }
`;
```

然后，我们使用`resolvers`在`pickColor`解析器函数中获取参数，如下所示:

```
const resolvers = {
  Query: {
    pickColor(_, { color }) {
      return color;
    }
  }
};
```

分解器返回`color`。

# 内部值

我们可以通过在冒号后放一个值来设置枚举的值。

例如，我们可以编写以下内容:

```
const express = require('express');
const { ApolloServer, gql } = require('apollo-server-express');const typeDefs = gql`
  enum AllowedColor {
    RED
    GREEN
    BLUE
  } type Query {
    pickColor(color: AllowedColor): String
  }
`;const resolvers = {
  AllowedColor: {
    RED: '#f00',
    GREEN: '#0f0',
    BLUE: '#00f',
  },
  Query: {
    pickColor(_, { color }) {
      return color;
    }
  }
};const app = express();
const server = new ApolloServer({ typeDefs, resolvers });
server.applyMiddleware({ app });app.listen(3000, () => console.log('server started'));
```

在上面的代码中，我们添加了:

```
AllowedColor: {
  RED: '#f00',
  GREEN: '#0f0',
  BLUE: '#00f',
}
```

到`resolvers`对象，将 ENUM 键映射到字符串值。

现在当我们进行以下查询时:

```
{
  pickColor(color: RED)
}
```

我们得到:

```
{
  "data": {
    "pickColor": "#f00"
  }
}
```

如果我们为`color`传入一个没有在项目中列出的值，那么我们会得到一个错误。

![](img/2f010b9b8e12bcd85e923be892a0de75.png)

Photo by [History in HD](https://unsplash.com/@historyhd?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以在类型定义字符串中定义枚举。

为了定义一个，我们使用`enum`关键字，后跟枚举类型的名称。然后，我们可以选择将枚举键映射到我们的`resolvers`对象中的值。

一旦我们这样做了，那么只有当我们将参数的类型设置为枚举类型时，我们才能传入枚举的可能值。

【JavaScript 简单明了的一句话:我们一直对帮助提升高质量内容感兴趣。如果您有文章想提交给 JavaScript，请用您的中用户名在[submissions@javascriptinplainenglish.com](mailto:submissions@javascriptinplainenglish.com)发邮件给我们，我们会将您添加为作者。