# 使用 Express GraphQL 创建和使用数据类型

> 原文：<https://javascript.plainenglish.io/create-and-use-data-types-with-express-graphql-518da213383b?source=collection_archive---------10----------------------->

![](img/ce2aedf6e8511aca345869bbdd2e882b.png)

Photo by [Stephen Dawson](https://unsplash.com/@srd844?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们可以用 Express 创建一个简单的 GraphQL 服务器。为此，我们需要`express-graphql`和`graphql`包装。

在本文中，我们将研究如何创建和使用我们自己的 GraphQL 数据类型。

# 对象类型

在许多情况下，我们不想接受并从 API 返回一个数字或一个字符串。我们可以创建自己的数据类型来接受和返回我们想要从 API 中得到的任何东西。

有了`express-graphql`包，我们可以在一个字符串中定义我们的数据类型，然后将其传递给`buildSchema`函数。

例如，我们可以编写以下代码来定义我们的类型，构建一个模式，并将我们的解析器添加到我们的代码中:

```
const express = require('express');
const graphqlHTTP = require('express-graphql');
const { buildSchema } = require('graphql');
const schema = buildSchema(`
  type RandomDie {
    numSides: Int!
    rollOnce: Int!
    roll(numRolls: Int!): [Int]
  } type Query {
    getDie(numSides: Int): RandomDie
  }
`);class RandomDie {
  constructor(numSides) {
    this.numSides = numSides;
  } rollOnce() {
    return 1 + Math.floor(Math.random() * this.numSides);
  } roll({ numRolls }) {
    const output = [];
    for (let i = 0; i < numRolls; i++) {
      output.push(this.rollOnce());
    }
    return output;
  }
}const root = {
  getDie: ({ numSides }) => {    
    return new RandomDie(numSides || 6);
  },
};const app = express();app.use('/graphql', graphqlHTTP({
  schema: schema,
  rootValue: root,
  graphiql: true,
}));
app.listen(3000, () => console.log('server started'));
```

在上面的代码中，我们通过编写以下内容来定义我们的模式:

```
const schema = buildSchema(`
  type RandomDie {
    numSides: Int!
    rollOnce: Int!
    roll(numRolls: Int!): [Int]
  }
  type Query {
    getDie(numSides: Int): RandomDie
  }
`);
```

我们用`numSides`场和`rollOnce``roll`法定义了`RandomDie`型。

然后我们定义了我们的`getDie`查询，以允许访问我们在`RandomDie`类型中定义的成员。

然后我们定义了我们的`RandomDie`类，我们将在我们稍后定义的`getDie`解析器中使用它:

```
class RandomDie {
  constructor(numSides) {
    this.numSides = numSides;
  }
  rollOnce() {
    return 1 + Math.floor(Math.random() * this.numSides);
  }
  roll({ numRolls }) {
    const output = [];
    for (let i = 0; i < numRolls; i++) {
      output.push(this.rollOnce());
    }
    return output;
  }
}
```

在这个类中，我们创建了`rollOnce`和`roll`方法，我们将返回结果。

最后，我们定义`getDie`解析器如下:

```
const root = {
  getDie: ({ numSides }) => {    
    return new RandomDie(numSides || 6);
  },
};
```

我们从参数中获取`numSides`，然后在实例化时将其传递给`RandomDie`构造函数。

然后在`/graphql`页面，我们可以在图形化界面中进行如下查询:

```
{
  getDie(numSides: 6) {
    rollOnce
    roll(numRolls: 3)
    numSides
  }
}
```

我们应该得到如下的回应:

```
{
  "data": {
    "getDie": {
      "rollOnce": 3,
      "roll": [
        6,
        4,
        5
      ],
      "numSides": 6
    }
  }
}
```

请注意，我们访问字段和调用没有参数的方法的方式与使用`rollOnce`和`numSides`的方式相同。

与传统的 REST APIs 相比，这种定义对象的方式为我们提供了一些优势。我们可以只进行一次查询来获得我们需要的东西，而不是使用一个 API 请求来获得关于一个对象的基本信息，以及多个请求来了解关于该对象的更多信息。

这节省了带宽，提高了性能，并简化了客户端逻辑。

![](img/ad7d4167cefbf2abbca7373c4fe3ec37.png)

Photo by [Webaroo.com.au](https://unsplash.com/@webaroo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以通过将它与模式的其他部分放在一起来创建新的类型。然后我们可以使用`buildSchema`函数来构建模式。

一旦我们这样做了，我们就创建了一个类来将类型字段映射到类成员中。然后，我们可以在解析器中实例化该类。

最后，我们可以通过发送类名作为查询名，然后发送成员名和参数(如果需要的话)来发出请求。

## **用简单的英语写的 JavaScript 的注释:**

我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到 submissions@javascriptinplainenglish.com[给我们，我们会把你添加为作者。](mailto:submissions@javascriptinplainenglish.com)