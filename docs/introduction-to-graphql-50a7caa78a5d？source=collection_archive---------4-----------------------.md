# GraphQL 简介

> 原文：<https://javascript.plainenglish.io/introduction-to-graphql-50a7caa78a5d?source=collection_archive---------4----------------------->

![](img/9fb7d5c36a56a65a00af4712400cf8d2.png)

Photo by [Luke Chesser](https://unsplash.com/@lukechesser?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

GraphQL 是我们的 API 的一种查询语言，也是一个服务器端运行时，通过使用数据的类型系统来运行查询。

在本文中，我们将了解如何对 GraphQL API 进行基本查询。

# 定义 API

我们通过定义类型和这些类型的字段来定义 API，并为每个类型上的每个字段提供函数。

例如，如果我们有以下类型:

```
type Query {
  person: Person
}
```

然后我们必须为相应的类型创建一个函数来返回数据:

```
function Query_person(request) {
  return request.person;
}
```

# 进行查询

一旦我们运行了 GraphQL 服务，我们就可以发送 GraphQL 查询来验证并在服务器上执行。

例如，我们可以进行如下查询:

```
{
  person {
    firstName
  }
}
```

那么我们可能会得到如下的 JSON:

```
{
  "person": {
    "firstName": "Joe"
  }
}
```

# 查询和突变

查询用于从 GraphQL 服务器获取数据，而变异用于操作存储在服务器上的数据。

例如，以下是获取人名的查询:

```
{
  person {
    name
  }
}
```

然后，我们可能会从服务器获得以下 JSON:

```
{
  "data": {
    "person": {
      "name": "Joe"
    }
  }
}
```

字段`name`返回一个`String`类型。

如果我们想获得更多的数据，我们可以随意更改查询。例如，如果我们编写以下查询:

```
{
  person {
    name
    friends {
      name
    }
  }
}
```

那么我们可能会得到如下的响应:

```
{
  "data": {
    "person": {
      "name": "Joe",
      "friends": [
        {
          "name": "Jane"
        },
        {
          "name": "John"
        }
      ]
    }
  }
}
```

上面的例子中的`friends`是一个数组。从查询的角度来看，它们看起来是一样的，但是服务器知道根据指定的类型返回什么。

# 争论

我们可以向查询和变异传递参数。如果我们向查询传递参数，我们可以做更多的事情。

例如，我们可以如下传入一个参数:

```
{
  person(id: "1000") {
    name    
  }
}
```

然后我们会得到这样的结果:

```
{
  "data": {
    "person": {
      "name": "Luke"
    }
  }
}
```

从服务器上。

使用 GraphQL，我们可以向嵌套对象传递参数。例如，我们可以写:

```
{
  person(id: "1000") {
    name
    height(unit: METER)
  }
}
```

那么我们可能会得到以下响应:

```
{
  "data": {
    "person": {
      "name": "Luke",
      "height": 1.9
    }
  }
}
```

在这个例子中，`height`字段有一个`unit`，它是一个枚举类型，代表一组有限的值。

`unit`可能是米也可能是英尺。

![](img/b7a32c546a59a5d9f02274b45a7e7286.png)

Photo by [Giorgio Tomassetti](https://unsplash.com/@gtomassetti?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 碎片

我们可以定义片段来重用复杂的查询。

例如，我们可以定义一个片段并按如下方式使用它:

```
{
  leftComparison: person(episode: FOO) {
    ...comparisonFields
  }
  rightComparison: person(episode: BAR) {
    ...comparisonFields
  }
}
​
fragment comparisonFields on Character {
  name
  appearsIn
  friends {
    name
  }
}
```

在上面的代码中，我们定义了`comparisonFields`片段，它包含了我们希望包含在每个查询中的字段列表。

然后我们有了`leftComparison`和`rightComparison`查询，它们通过使用`...`操作符包含了`comparisonFields`片段的字段。

然后我们会得到这样的结果:

```
{
  "data": {
    "leftComparison": {
      "name": "Luke",
      "appearsIn": [
        "FOO",
        "BAR"
      ],
      "friends": [
        {
          "name": "Jane"
        },
        {
          "name": "John"
        }
      ]
    },
    "rightComparison": {
      "name": "Mary",
      "appearsIn": [
        "FOO",
        "BAR"
      ],
      "friends": [
        {
          "name": "Mary"
        },
        {
          "name": "Alex"
        }
      ]
    }
  }
}
```

# 在片段中使用变量

我们可以将变量传入片段，如下所示:

```
query PersonComparison($first: Int = 3){
  leftComparison: person(episode: FOO) {
    ...comparisonFields
  }
  rightComparison: person(episode: BAR) {
    ...comparisonFields
  }
}
​
fragment comparisonFields on Character {
  name
  appearsIn
  friends(first: $first) {
    name
  }
}
```

那么我们可能会得到这样的结果:

```
{
  "data": {
    "leftComparison": {
      "name": "Luke",
      "appearsIn": [
        "FOO",
        "BAR"
      ],
      "friends": [
        {
          "name": "Jane"
        },
        {
          "name": "John"
        }
      ]
    },
    "rightComparison": {
      "name": "Mary",
      "appearsIn": [
        "FOO",
        "BAR"
      ],
      "friends": [
        {
          "name": "Mary"
        },
        {
          "name": "Alex"
        }
      ]
    }
  }
}
```

作为回应。

操作类型可以是查询、变异或订阅，并描述我们打算做什么操作。除非我们使用查询速记语法，否则它是必需的。在这种情况下，我们不能为操作提供名称或变量定义。

对于我们的操作来说，操作名是一个有意义且明确的名称。多工序单据中需要。但是鼓励使用它，因为它有助于调试和服务器端日志记录。

用名字很容易识别操作。

# 结论

GraphQL 是一种查询语言，它让我们能够以一种清晰的方式向服务器发送请求。它的工作方式是将带有操作类型和名称的嵌套对象以及任何变量发送到服务器。

然后服务器会返回我们想要的响应。

操作类型包括获取数据的查询和对服务器上的数据进行更改的突变。