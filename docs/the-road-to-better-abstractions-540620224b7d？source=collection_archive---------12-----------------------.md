# 通向更好抽象的道路

> 原文：<https://javascript.plainenglish.io/the-road-to-better-abstractions-540620224b7d?source=collection_archive---------12----------------------->

## 重构 JavaScript:集合管道

![](img/63226bbbe7faa5be36098c660c658d44.png)

Photo by [Jonathan Fox](https://unsplash.com/@foxy619?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/road-map?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

[我最近开始写](https://medium.com/swlh/refactoring-javascript-collection-pipelines-3ebc2e63abee)关于重构 JavaScript 使用集合管道而不是循环。我大胆地宣称这将导致更干净的代码，但我还没有做的是为**为什么**我认为这会导致更干净的代码做一个清晰的论证。

对我来说，获得一个任意对象的集合，通过一系列单独的*转换*和*过滤器*并从另一端获得一个新的集合，是一件真正令人满意的事情。看到一个工作流被分解成离散的、通常是单行的步骤，没有副作用，是编写代码的好方法。

# 功能构建模块

`map()`、`filter()`和`reduce()`是 JavaScript 中处理集合(数组)的强大高阶函数。我将它们称为功能构建模块，因为:

*   它们都返回新的集合，不会改变原来的集合，
*   他们只做一件事，而且
*   *一般情况下*应该不会有副作用。

你会惊讶地发现这三个特征可以简化你的代码。

让我们检查这三种收集方法中的每一种，并尝试看看为什么它们可以帮助我们清理代码。

## 地图()

地图在我看来是最重要的采集方式之一。Map 用于*将集合中的每个项目*转换成其他项目。给定一些项目集合和一个函数，map 会将该函数应用于每个项目，并返回一个相同大小的新的*集合。*

地图是如此强大的抽象，因为它有非常明确的意图。它获取一个项目集合，然后*将*映射到另一个项目集合。每个数组中的项目之间有 1:1 的映射关系。当你看到一个`map()`被使用时，你可以把它内化为`transform`，事实上，转换数据是我在代码中使用 map 的第一个地方。考虑下面的代码示例，其中我查询 DynamoDB(我最近选择的持久层)并返回一个项目:

```
// src/user/queries/findAllUsers.tsexport const findAllUsers = async (tennantId: string): Promise<User[]> => {
  const params = {
    TableName: process.env.DYNAMODB_TABLE,
    KeyConditionExpression: 'pk = :pk AND begins_with(sk, :sk)',
    ExpressionAttributeValues: {
      ':pk': tennantId,
      ':sk': 'user:',
    },
  };

  const { Items } = await dynamoDBClient.query(params).promise();

  return Items ? Items.map((item: DynamoDBUser) => userFromDynamoDBItem(item)) : [];
};
```

作为开发人员，您可能已经编写过 100 次这样的查询，如果不是用 DynamoDB，那么就是用某种形式的 SQL 或 NoSQL。除非您使用某种形式的 [ActiveRecord](https://www.martinfowler.com/eaaCatalog/activeRecord.html) 实现，否则您可能不喜欢将数据库模式耦合到数据的域表示。因此，我们想要获取一个数据集合——在本例中是一个*用户*的数据库表示，并将它转换成一个不同的表示。这里的`userFromDynamoDBItem`完全按照它所说的去做——它接受一个 DynamoDBItem 并将其转换成一个用户。

为了完整起见，这里是我们的*用户的接口定义—* 当我们在应用程序逻辑中处理数据时，我们希望数据看起来是这样的。

```
export interface User {
  id: string;
  givenName: string;
  familyName: string;
  email: string;
}
```

这就是我们的数据库表示的样子——你可以明白为什么我们要在将数据传递给应用程序之前对其进行转换。

```
export interface DynamoDBUser extends BaseDynamoDBRecord {
  email: string;
  givenName: string;
  familyName: string;
}export interface BaseDynamoDBRecord {
  pk: string;
  sk: string;
  __context__: Record<string, string>;
  gsi1pk?: string;
  gsi1sk?: string;
  gsi2pk?: string;
  gsi2sk?: string;
  type: string;
  ttl?: number;
}
```

难道我们不能用一个标准的循环来转换这些数据吗？

是的，我们可以，但是，有一个方法来转换数据，这是一致的和明确的意图，减少认知开销，并提高我们的代码可读性。

这是一个相当自负的例子，但是想象一下，你的代码中有这样的功能，有一天你的产品负责人找到你说“我们想记录我们在任何查询中从数据库中检索的每个用户的 userId”。

这可能是一个奇怪的要求，但它不是不可能的。如果我们使用一个循环来完成这个转换，我们可以很容易地在循环中抛出一个语句，记录转换的每次迭代。这听起来很吸引人，但实际上这是一件非常糟糕的事情。通过这样做，我们增加了转换函数的责任。

最好将这个需求添加到一个新的收集方法中，这次我们将使用一个`forEach`

```
*const* users = Items
  .map((item: DynamoDBUser): User => userFromDynamoDBItem(item))
  .forEach((user: User) => console.log(user.id)) 
```

在这个例子中，我们已经非常清楚地表达了我们的意图。首先，我们将用户转换成他们的域表示，然后我们遍历集合并记录 ID。这清楚地表明，日志记录是一种明确的意图，而不是开发人员留下的调试步骤。

尽管到目前为止我还没有在我的系列中的任何地方提到过`forEach`，我还是想用这个例子来说明它在`map()`的世界中仍然有它的位置。一般来说，我们应该只`map`如果我们是*转换*和项目。如果不是，那么`forEach`就是正确的选择。

## 过滤器()

`filter()`方法用于*过滤掉*您不希望包含在集合中的任何元素。您给`filter()`一个谓词(返回`boolean`的表达式)，如果谓词返回`true`，您保留该项，如果它返回`false`，您删除它。

`filter()`和`map()`的主要区别在于，filter 并不转换或改变集合中的任何单个项目，而是通过从集合中删除项目来改变整个集合。返回的项不仅与原始数组中的项类型相同，而且是相同的项。

过滤很容易理解，所以我们的例子看起来有点琐碎，不过:

```
*interface Article* {
  id: *string*;
  title: *string*;
  body: *string*;
  status: 'PUBLISHED' | 'DRAFT'
}

*const* publicshedArticles: *Article*[] = 
  articles.filter((article: *Article*) => article.status === 'PUBLISHED');
```

正如我们所看到的，我们收集了一些文章，并且只保留那些已经发表的文章。关于`filter`的一个好处是，因为它非常简单，所以它在[自由点编程风格](https://stackoverflow.com/questions/944446/what-is-point-free-style-in-functional-programming)中工作得非常好。我们可以将上面的过滤器重写为无点过滤器，如下所示:

```
*const* isPublished = article => article.status === 'PUBLISHED'

*const* publishedArticles: *Article*[] = articles.filter(isPublished);
```

同样，所有这些都可以通过 humble 循环来实现，但是，我将再次指出，由于更明确的意图声明，可读性得到了提高。我们可以看到单词`filter`，并且知道随后的任何东西都会给我们一个集合，小于或等于原始集合的大小，同时保持我们的单个项目不变。

## 减少()

`reduce`操作用于获取一些项目集合，然后*将其减少为一个值。它不知道该值是多少；它可以是一个对象、一个数组、一个数字或者一个字符串——这无关紧要。*

我已经在这里写了 `[reduce()](https://medium.com/better-programming/the-power-of-reduce-87bfdb7de04)` [的](https://medium.com/better-programming/the-power-of-reduce-87bfdb7de04)[功能——甚至展示了如何通过`reduce().`实现`map()`和`filter()`](https://medium.com/better-programming/the-power-of-reduce-87bfdb7de04)

在这个例子中，我展示了如何使用一个 reduce 函数来解决 Canva 的 HackerRank 算法挑战。

除了对值求和，`reduce()`最常见的用例之一是连接字符串。鉴于我已经在这里提到了 DynamoDB，我不妨展示一个使用`reduce()`从一个对象动态生成 DynamoDB 更新表达式的例子

```
*const* ExpressionAttributeNames = {prop1: 'prop1', prop2: 'prop2', prop3: 'prop3'}

*const* UpdateExpression = ***Object***.values(ExpressionAttributeNames)
  .reduce((acc, curr) => `${acc} #${curr} = :${curr},`, 'SET').slice(0, -1);

*console*.log(UpdateExpression);
*// SET #prop1 = :prop1, #prop2 = :prop2, #prop3 = :prop3*
```

这里，我们获取一个对象，并动态创建一个表示 DynamoDB 更新表达式语法的字符串。如果对 DynamoDB 不熟悉，也不用太担心。看看我们如何在一个简单的 reduce 函数中从`ExpressionAttributeNames`到`SET #prop1 = :prop1, #prop2 = :prop2, #prop3 = :prop3`。您可以使用类似的逻辑来生成任何表达式、csv 或您可能需要的任何其他字符串。

如果你使用 DynamoDB 并且想要一个关于如何使用我的动态更新表达式工具的完整例子，你可以查看完整的要点

虽然 reduce 非常强大，但要理解它通常很困难。在大多数情况下，您真正想要的是构建在 reduce 之上的更高层次的抽象。在上面的要点中，我们将 reduce 函数包装在一个叫做`DynamicUpdateExpressionFromObject`的更高层次的抽象中。在解决 Canva 面试问题的例子中，我们将把 reduce 封装在一个抽象出实现的函数中。

# 包扎

一旦你熟悉了`map()`、`filter()`和`reduce()`，我保证你会被你的代码的美丽和简单所吸引。将您的业务逻辑视为数据流经的管道，这是一种非常诱人的思考代码的方式。它帮助您将所有副作用推到代码的边缘，减少测试路径，为开发人员将额外的责任偷偷放入您的函数提供任何缝隙，并极大地提高您代码的 [*圈复杂度*](https://en.wikipedia.org/wiki/Cyclomatic_complexity#:~:text=Cyclomatic%20complexity%20is%20a%20software,in%201976.) 。

如果你想学习**如何**实际重构现有代码，请关注我即将出版的电子书，它将详细介绍如何重构，并将包含更多真实世界的例子和应用。你可以在这里以 10 美元的折扣价预订这本书。

## **简明英语 JavaScript**

你知道我们有三份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)找到一切的链接！