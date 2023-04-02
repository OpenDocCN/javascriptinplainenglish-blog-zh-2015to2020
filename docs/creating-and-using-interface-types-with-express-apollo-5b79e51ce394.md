# 使用 Express Apollo 创建和使用接口类型

> 原文：<https://javascript.plainenglish.io/creating-and-using-interface-types-with-express-apollo-5b79e51ce394?source=collection_archive---------4----------------------->

![](img/6551ca09c5051855097af580888b082f.png)

Photo by [niculcea florin](https://unsplash.com/@florinnicu?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Apollo 服务器作为节点包提供。我们可以用它来创建一个接受 GraphQL 请求的服务器。

在本文中，我们将了解如何在 Apollo Server 和 Express 应用程序中创建和使用接口类型。

# 接口类型

接口类型让我们通过使用抽象类型来构建模式。抽象类型不能直接在模式中使用，但是它可以用作创建其他类型的构建块。

我们可以用它来创建共享相同属性的多种类型。

例如，我们可以创建接口并按如下方式使用它们:

```
const express = require('express');
const { ApolloServer, gql } = require('apollo-server-express');const data = {
  '1': { title: 'Math Textbook', author: 'Jane Smith', classes: ['Math'] },
  '2': { title: 'Coloring Book', author: 'Joe Smith', colors: ['red'] },
}const typeDefs = gql`
  interface Book {
    title: String
    author: String
  } type TextBook implements Book {
    title: String
    author: String
    classes: [String]
  } type ColoringBook implements Book {
    title: String
    author: String
    colors: [String]
  } type Query {
    schoolBooks(id: Int!): Book
  }
`;const resolvers = {
  Book: {
    __resolveType(obj, context, info) {
      if (obj.classes) {
        return 'TextBook';
      } if (obj.colors) {
        return 'ColoringBook';
      } return null;
    },
  },
  Query: {
    schoolBooks: (_, { id }) => data[id]
  },
};const app = express();
const server = new ApolloServer({ typeDefs, resolvers });
server.applyMiddleware({ app });app.listen(3000, () => console.log('server started'));
```

在上面的代码中，我们首先创建了带有对象的`data`对象，这些对象具有与我们稍后将创建的类型相对应的各种字段:

```
const data = {
  '1': { title: 'Math Textbook', author: 'Jane Smith', classes: ['Math'] },
  '2': { title: 'Coloring Book', author: 'Joe Smith', colors: ['red'] },
}
```

然后我们通过书写来创建我们的类型:

```
const typeDefs = gql`
  interface Book {
    title: String
    author: String
  } type TextBook implements Book {
    title: String
    author: String
    classes: [String]
  } type ColoringBook implements Book {
    title: String
    author: String
    colors: [String]
  } type Query {
    schoolBooks(id: Int!): Book
  }
`;
```

在上面的代码中，我们有包含`title`和`author`字段的`Book`接口类型，这意味着实现`Book`接口的其他类型也必须包含相同类型的字段。

因此，我们有了带有`title`和`author`字符串字段的`TextBook`和`ColoringBook`字段。`TextBook`也有字符串数组`classes`字段，`ColoringBook`有`colors`字符串数组字段。

关键字`implements`表示一个类型正在实现一个接口。

然后在我们的`Query`类型中，我们添加了接受 Int `id`参数并返回一个`Book`的`schoolBooks`字段。

在我们的`resolvers`对象中，我们在带有接口名称的属性中使用带有`__resolveType`方法的`Book`类型来检查`obj`参数的结构，并相应地返回类型名称。

`Query`对象有一个`schoolBooks`解析器，它接受一个`id`参数并返回给定`id`的数据。

最后，在 GraphQL Playground 页面中，我们通过转到`/graphql`页面打开该页面，我们使用内联片段运行一个查询，就像我们使用联合类型一样，如下所示:

```
{
  schoolBooks(id: 1) {
    __typename
    ...on ColoringBook {
      title
      author
      colors
    }
    ...on TextBook {
      title
      author
      classes
    }
  }
}
```

然后我们得到:

```
{
  "data": {
    "schoolBooks": {
      "__typename": "TextBook",
      "title": "Math Textbook",
      "author": "Jane Smith",
      "classes": [
        "Math"
      ]
    }
  }
}
```

作为回应。

如果我们将`id`改为 2，我们得到:

```
{
  "data": {
    "schoolBooks": {
      "__typename": "ColoringBook",
      "title": "Coloring Book",
      "author": "Joe Smith",
      "colors": [
        "red"
      ]
    }
  }
}
```

相反。

![](img/58fa296f5c6ec21922cb2c2d96e53a16.png)

Photo by [Dan Meyers](https://unsplash.com/@dmey503?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们创建可以作为其他具体类型的抽象类型的接口。

当一个类型实现一个接口时，这意味着接口中的字段和类型都必须包含在内。这减少了出错的机会，并鼓励代码重用。

像往常一样，我们将接口添加到类型定义中，然后我们可以用关键字`implements`创建实现接口的类型。

然后在我们的解析器中，我们将`__resolveType`方法添加到具有我们的接口类型名称的对象中，以根据传递到方法中的对象的结构返回类型名称。

在`Query`属性中，我们添加带有我们想要添加的参数的字段，并返回我们想要返回的、与我们指定的类型结构相匹配的对象。

【JavaScript 简单英语提示:我们总是乐于帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，请用你的 Medium 用户名在[submissions@javascriptinplainenglish.com](mailto:submissions@javascriptinplainenglish.com)给我们发电子邮件，我们会将你添加为作者。