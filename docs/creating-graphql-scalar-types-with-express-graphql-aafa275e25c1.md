# 使用 Express-GraphQL 创建 GraphQL 标量类型

> 原文：<https://javascript.plainenglish.io/creating-graphql-scalar-types-with-express-graphql-aafa275e25c1?source=collection_archive---------0----------------------->

![](img/f479325796c5844ef9d930d2e412ffa0.png)

Photo by [Filip Mroz](https://unsplash.com/@mroz?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

使用 GraphQL，我们可以像创建对象类型和其他复合数据类型一样创建标量类型。

在本文中，我们将看看如何用 Express-GraphQL 创建标量类型。

# GraphQLScalarType

我们可以用`GraphQLScalarType`构造函数创建一个 GraphQL 标量类型。要创建标量类型，我们必须给它一个名称、一个可选的描述以及序列化和解析值的方法。

构造函数接受一个具有以下字段的对象:

*   `name` —我们要定义为字符串的标量类型的名称
*   `description` —描述我们的类型的可选字符串
*   `serialize`——一个函数，接受一个`value`参数，这个参数可以是任何东西，然后返回我们想要转换成的东西以便传输
*   `parseValue` —一个可选的函数，带有一个`value`参数，这个参数可以是任何值，然后根据我们的意愿返回解析后的值
*   `parseLiteral` —一个采用抽象语法树对象的函数，抽象语法树对象是文字的值，然后我们返回我们想要将值转换成的内容。

# 例子

例如，我们可以编写以下代码来创建一个标量类型，并在响应中返回它:

```
const express = require('express');
const graphqlHTTP = require('express-graphql');
const graphql = require('graphql');const dateValue = (value) => {
  if (value instanceof Date) {
    return +value;
  }
}const DateType = new graphql.GraphQLScalarType({
  name: 'Date',
  serialize: dateValue,
  parseValue: dateValue,
  parseLiteral(ast) {
    return dateValue(ast.value);
  }
});const queryType = new graphql.GraphQLObjectType({
  name: 'Query',
  fields: {
    currentDate: {
      type: DateType,
      resolve: () => {
        return new Date();
      }
    }
  }
});const schema = new graphql.GraphQLSchema({ query: queryType });const app = express();
app.use('/graphql', graphqlHTTP({
  schema: schema,
  graphiql: true,
}));app.listen(3000, () => console.log('server started'));
```

在上面的代码中，我们将`dateValue`函数定义如下:

```
const dateValue = (value) => {
  if (value instanceof Date) {
    return +value;
  }
}
```

在这个函数中，我们检查`value`是否是从`Date`构造函数创建的，如果是，那么我们返回转换成时间戳的`Date`对象。

然后我们用它来创建一个名为`Date`的新标量类型，如下所示:

```
const DateType = new graphql.GraphQLScalarType({
  name: 'Date',
  serialize: dateValue,
  parseValue: dateValue,
  parseLiteral(ast) {
    return dateValue(ast.value);
  }
});
```

我们传入了名字，是`'Date'`。然后我们用带有`dateValue`的`serialize`属性返回时间戳作为标量类型的序列化版本。

同样，我们用相同的函数将`Date`对象解析成一个 UNIX 时间戳。

在`parseLiteral`函数中，我们将值传递给`dateValue`函数，将`Date`对象转换成 UNIX 时间戳。

然后，我们创建我们的查询类型，以便我们可以向用户返回时间戳响应，如下所示:

```
const queryType = new graphql.GraphQLObjectType({
  name: 'Query',
  fields: {
    currentDate: {
      type: DateType,
      resolve: () => {
        return new Date();
      }
    }
  }
});
```

我们创建了`Query`对象类型，这样我们就可以查询它，然后在`fields`属性中，我们返回一个`currentDate`字段，它属于我们之前定义的`Date`标量类型。

在`resolve`方法中，我们只返回`new Date()`，它将自动解析为响应中的 UNIX 时间戳，因为这是我们决定在`dateValue`函数中序列化它的方式。

那么当我们进行如下查询时:

```
{
  currentDate
}
```

我们会得到这样的结果:

```
{
  "data": {
    "currentDate": 1579464244268
  }
}
```

其中的`currentDate`被当前的 UNIX 时间戳替换。

![](img/89e440d893adba259b074af629cea45c.png)

Photo by [Vlad Tchompalov](https://unsplash.com/@tchompalov?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以通过使用`GraphQLScalarType`构造函数创建一个标量类型。

要定义一个，我们必须传入函数来序列化和解析我们想要的值。