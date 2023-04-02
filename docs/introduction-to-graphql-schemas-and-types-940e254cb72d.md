# GraphQL 模式和类型介绍

> 原文：<https://javascript.plainenglish.io/introduction-to-graphql-schemas-and-types-940e254cb72d?source=collection_archive---------2----------------------->

![](img/dc414a019048bac6c60d583c9ab91033.png)

Photo by [Adam Griffith](https://unsplash.com/@aggriffith?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

GraphQL 是我们的 API 的一种查询语言，也是一个服务器端运行时，通过使用数据的类型系统来运行查询。

在本文中，我们将研究 GraphQL 类型系统，以及它如何描述可以查询哪些数据。

# 模式和类型

GraphQL 请求是关于在对象上选择字段的。

在一个查询中，我们有一个根对象，然后我们从该对象中选择一个字段，如果它一直存在于底层，则选择该层以下的字段。

例如，如果我们有以下查询:

```
{
  person {
    name
  }
}
```

然后我们会得到这样的结果:

```
{
  "data": {
    "person": {
      "name": "Jane"
    }
  }
}
```

作为回应。

GraphQL 查询与结果非常匹配，因此我们可以预测查询将返回什么，而不需要知道太多关于服务器的信息。

但是对我们需要的数据有一个准确的描述仍然是有用的。

这就是显式类型有用的地方。

# 类型语言

GraphQL 服务可以用任何语言编写。因此，类型必须以与语言无关的方式定义。

# 对象类型和字段

我们可以如下定义 GraphQL 数据类型:

```
type Person {
  name: String!
  addresses: [Address!]!
}
```

在上面的代码中，我们用关键字`type`创建了一个新的类型。在类型中，我们有`name`字段，它是`String`，还有`addresses` ，它是`Address` es 的数组，这是另一种类型。

`String`是内置的标量类型。感叹号表示该字段不可为空。因此，`String!`是一个不可空的字符串。

`[Address!]!`是`Address`对象的数组。它也是不可空的，所以当我们查询`addresses`时，我们总是可以期待一个包含零个或更多项的数组。由于`Address!`也是不可空的，我们总是可以期望数组的每一项都是一个`Address`对象。

# 争论

GraphQL 对象类型的每个字段可以有零个或多个参数。

例如，我们可以这样写:

```
type Person {
  name: String!
  height(unit: HeightUnit = METER): Float
}
```

在上面的代码中，`Person`中的`height`字段将`unit`作为参数。`unit`属于`HeightUnit`类型，默认设置为`METER`。

`height`返回一个浮点数。

# 查询和变异类型

模式中有两种特殊的类型。他们是`Query`和`Mutation`。

每个 GraphQL 服务都有一个`query`类型，可能有也可能没有`mutation`类型。

这些类型与任何其他对象类型相同，但它们很特殊，因为它们定义了每个 GraphQL 查询的入口点。

如果我们有如下的查询:

```
query {
  person {
    name
  }
  robot(id: "2000") {
    name
  }
}
```

然后，GraphQL 服务需要具有带`person`和`robot`字段的查询类型:

```
type Query {
  person(name: String): Person
  robot(id: ID!): Robot
}
```

我们用同样的方式定义突变类型。我们在`Mutation`类型上定义字段，这些字段可以作为我们可以在查询中调用的根突变字段。

![](img/d3649e18401adb85155807f84bd0ba8a.png)

Photo by [sarandy westfall](https://unsplash.com/@sarandywestfall_photo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 标量类型

标量字段是所有对象类型最终包含的内容。它们是最基本的字段类型。

GraphQL 自带以下标量类型:

*   `Int` —一个有符号的 32 位整数
*   `Float` —有符号的双精度浮点值
*   `String`—UTF-8 字符序列
*   `Boolean` — `true`或`false`
*   `ID` —用于获取对象或作为缓存关键字的唯一标识符。它的序列化方式与字符串相同，但它不适合人类阅读

大多数 GraphQL 服务实现也允许我们指定定制的标量类型。

我们可以定义如下:

```
scalar Date
```

然后由使用来决定如何序列化、反序列化和验证它们。

# 枚举类型

枚举类型是标量的特殊类型，仅限于一组特定的允许值。

它让我们验证该类型的任何参数都是允许的值之一。

此外，它通过类型系统传达一个字段总是有限的一组值中的一个。

我们可以如下定义枚举:

```
enum Fruit {
  APPLE
  ORANGE
  GRAPE
}
```

那么如果我们有`Fruit`，我们期望它是`APPLE`、`ORANGE` 或者`GRAPE`。

# 结论

我们可以定义类型，以便验证提交给 GraphQL 服务器的数据。

为此，如果是对象类型，我们用`type`关键字创建一个类型。

我们还可以创建标量类型，这是最基本的类型，可以包含在其他类型中或直接引用。我们可以用关键字`scalar`来定义它们。

为了定义一个具有固定值的类型，我们可以定义一个具有几个可能的常数值的`enum`类型。

感叹号表示类型不可为空。