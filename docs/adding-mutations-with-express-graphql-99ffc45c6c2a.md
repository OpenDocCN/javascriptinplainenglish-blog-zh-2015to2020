# 使用 Express GraphQL 添加突变

> 原文：<https://javascript.plainenglish.io/adding-mutations-with-express-graphql-99ffc45c6c2a?source=collection_archive---------4----------------------->

![](img/e5ee00422d1cc1ddd3ed21a8fa504e2d.png)

Photo by [Sangga Rima Roman Selia](https://unsplash.com/@sxy_selia?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们可以用 Express 创建一个简单的 GraphQL 服务器。为此，我们需要`express-graphql`和`graphql`包。

在本文中，我们将看看如何用 Express 和 GraphQL 创建突变和输入类型。

# 突变和输入类型

为了创建突变，我们创建一个具有`Mutation`类型而不是`Query`的模式。

然后很简单，让 API 端点成为顶级`Mutation`类型的一部分，而不是`Query`类型。

突变和查询都可以由根解析器来处理。

然后，我们可以创建一个接受查询和变异的 GraphQL 服务器，如下所示:

```
const express = require('express');
const graphqlHTTP = require('express-graphql');
const { buildSchema } = require('graphql');
const crypto = require('crypto');
const schema = buildSchema(`
  input TodoInput {
    text: String    
  } type Todo {
    id: ID!
    text: String    
  } type Query {
    getTodo(id: ID!): Todo
  } type Mutation {
    createTodo(input: TodoInput): Todo
    updateTodo(id: ID!, input: TodoInput): Todo
  }
`);class Todo {
  constructor(id, { text }) {
    this.id = id;
    this.text = text;
  }
}let todos = {};const root = {
  getTodo: ({ id }) => {
    if (!todos[id]) {
      throw new Error('Todo not found.');
    }
    return new Todo(id, todos[id]);
  },
  createTodo: ({ input }) => {
    const id = crypto.randomBytes(10).toString('hex');
    todos[id] = input;
    return new Todo(id, input);
  },
  updateTodo: ({ id, input }) => {
    if (!todos[id]) {
      throw new Error('Todo not found');
    }
    todos[id] = input;
    return new Todo(id, input);
  },
};const app = express();app.use('/graphql', graphqlHTTP({
  schema: schema,
  rootValue: root,
  graphiql: true,
}));
app.listen(3000, () => console.log('server started'));
```

在上面的代码中，我们通过编写以下内容来定义我们的类型:

```
const schema = buildSchema(`
  input TodoInput {
    text: String    
  } type Todo {
    id: ID!
    text: String    
  } type Query {
    getTodo(id: ID!): Todo
  } type Mutation {
    createTodo(input: TodoInput): Todo
    updateTodo(id: ID!, input: TodoInput): Todo
  }
`);
```

我们创建了输入类型`TodoInput`和`Todo`类型。然后我们用`getTodo`成员创建了`Query`类型，这样我们就可以得到我们的 todo 项。

然后在我们的`Mutation`中，我们添加了`createTodo`和`updateTodo`成员，这样我们就可以添加和更新 todos。

然后我们创建我们的`Todo`类，这样我们可以存储 todo 数据:

```
class Todo {
  constructor(id, { text }) {
    this.id = id;
    this.text = text;
  }
}
```

接下来，我们有了根解析器:

```
const root = {
  getTodo: ({ id }) => {
    if (!todos[id]) {
      throw new Error('Todo not found.');
    }
    return new Todo(id, todos[id]);
  },
  createTodo: ({ input }) => {
    const id = crypto.randomBytes(10).toString('hex');
    todos[id] = input;
    return new Todo(id, input);
  },
  updateTodo: ({ id, input }) => {
    if (!todos[id]) {
      throw new Error('Todo not found');
    }
    todos[id] = input;
    return new Todo(id, input);
  },
};
```

它添加了与我们在模式中指定的相同的函数，以便我们在进行一些查询时可以做一些事情。

在这个例子中，`getTodo`，我们将用给定的`id`返回 todo。将返回找到的 todo。否则，我们抛出一个错误。

在`createTodo`中，我们从查询中获得`input`，然后将 todo 条目添加到我们的`todos`对象中，这是我们用来存储 todo 的假数据库。将返回保存的 todo。

然后我们用`updateTodo`函数通过`id`更新 todo。给定的`id`将被替换为`input`的内容。将返回保存的 todo。如果没有找到给定`id`的 todo，我们抛出一个错误。

然后，当我们转到`/graphql`页面时，我们可以在 GraphiQL 窗口中键入以下内容:

```
mutation {
  createTodo(input: {text: "eat"}) {
    id
    text
  }
}
```

然后我们会得到这样的结果:

```
{
  "data": {
    "createTodo": {
      "id": "c141d1fda69e8d9084bd",
      "text": "eat"
    }
  }
}
```

作为回应。

如果我们查询更新 todo，如下所示:

```
mutation {
  updateTodo(id: "e99ce10750c93793a23d", input: {text: "eat"}) {
    id
    text
  }
}
```

我们会得到这样的结果:

```
{
  "data": {
    "updateTodo": {
      "id": "e99ce10750c93793a23d",
      "text": "eat"
    }
  }
}
```

作为回应。

如果没有找到 todo，那么我们得到:

```
{
  "errors": [
    {
      "message": "Todo not found",
      "locations": [
        {
          "line": 9,
          "column": 3
        }
      ],
      "path": [
        "updateTodo"
      ]
    }
  ],
  "data": {
    "updateTodo": null
  }
}
```

作为回应。

我们可以进行如下的`getTodo`查询:

```
query {
  getTodo(id: "e99ce10750c93793a23d"){
    id
    text
  }
}
```

然后我们得到:

```
{
  "data": {
    "getTodo": {
      "id": "e99ce10750c93793a23d",
      "text": "eat"
    }
  }
}
```

作为回应。

![](img/bc9f51e813916222b13a946da1bd5065.png)

Photo by [Priscilla Du Preez](https://unsplash.com/@priscilladupreez?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以像处理查询一样创建突变。

为了在我们的 GraphQL 服务器中接受变异操作，我们创建了用于存储数据的类型，然后通过用我们的成员填充`Mutation`类型来创建变异。

然后，我们可以使用`buildSchema`函数来构建我们刚刚指定的模式。

然后在我们的根归约器中，我们用我们在类型定义中指定的名字来创建函数。

最后，我们可以查询我们的服务器来运行突变。

## **用简单英语写的 JavaScript 的一个注释:**

我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到 submissions@javascriptinplainenglish.com[给我们，我们会把你添加为作者。](mailto:submissions@javascriptinplainenglish.com)