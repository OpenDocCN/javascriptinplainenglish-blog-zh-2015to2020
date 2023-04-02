# 开发者故事:从服务器 API 客户端混淆数据库形状

> 原文：<https://javascript.plainenglish.io/developer-story-obfuscating-database-shape-from-server-api-clients-971d66a1b565?source=collection_archive---------3----------------------->

![](img/b13dc6288d65abbd96f0b234d651e65e.png)

在为基于 web 的客户端界面开发服务器 API 应用程序时，通常会创建一个服务器应用程序来满足管理用户和普通用户客户端的需求。由于管理用户和普通用户服务器 API 应用程序中包含的大多数逻辑是相同的或非常相似的，并且需要更多的时间和精力来维护两个独立的应用程序，而不是一个，所以许多开发团队经常选择使用一个统一的服务器 API 应用程序来服务所有的客户端应用程序。在我的职业生涯中，我创建的服务器 API 应用程序比我记得的还要多，所以我认为我深刻理解在头脑中有一个明智的策略来进行服务器 API 应用程序开发的重要性。

在[之前的](https://medium.com/swlh/developer-story-single-database-interface-442dd6804424)开发者故事条目中，我讨论了在我开发的所有 NodeJS 服务器 API 应用程序中创建单一数据库接口的个人策略。该策略包括为整个服务器 API 应用程序中的所有逻辑创建一个单一的数据库入口点，以便与数据库中的集合或表的所有交互都是一致的。我想在这个开发者故事条目中讨论的另一个与数据库相关的策略是如何模糊支持您的服务器 API 应用程序的数据库的真实形状。任何使用过服务器 API 应用程序的人都知道，这些应用程序通常提供支持四个基本 [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) 功能的功能，这取决于应用程序中实现的授权方案。作为本条目主题的策略将集中在首字母缩略词 Read 中的 R 上。

在开发任何新的服务器 API 应用程序时，API 开发人员总是需要做出许多决定，其中之一就是如何将从数据库中检索到的数据打包并传递给客户端。对于许多开发人员来说，解决这个问题的简单方法是，当它从数据库中出来时，将它传递给客户机。虽然在最初的开发阶段，当您只是试图启动并运行服务器 API 应用程序来帮助客户端开发工作时，这可能是一个可接受的解决方案，但我认为这绝对不是一个可接受的长期解决方案。

我认为简单的传递式解决方案从长期来看不可接受的第一个原因是，数据库保存的数据几乎总是比客户端需要的数据多。例如，数据库通常保存大量元数据类型的信息，这些信息只在数据库的上下文中或对数据库管理员有用，不应该出现在数据库之外的任何上下文中。此外，从安全角度来看，数据库中保存的其他信息可能非常敏感，除非在非常特殊的情况下，并且只有持有正确凭据的用户才能访问这些信息。在将从数据库中检索到的信息传递到客户端之前，清理数据集并过滤掉所有敏感和不必要的信息总是很重要的。

我认为简单的传递式解决方案从长期来看是不可接受的第二个原因是，数据库中存储的数据的实际形状通常与客户机应该使用的形状不同。除非您的数据库只保存单个集合或表，其中包含简单的平面文档或行，而这些文档或行与其他集合或表没有连接，否则在将数据发送到客户端之前，您至少需要对数据进行一点点整形。不能合理地期望客户机总是以数据在数据库中所处的形状接收数据。存储在数据库中的数据通常不具有类似于对客户最有用的形状，因为最好以最有效的方式将数据存储在数据库中，并且当数据存储在数据库中时，以最容易处理该数据的方式存储。API 开发人员的工作是在将数据传递给客户端之前，尽可能地提前执行工作，以简化数据的形状，使其易于理解。

我个人选择的数据库是 MongoDB，所以我在这篇文章中展示的所有代码都是专门针对这个数据库系统的。在[之前的](https://medium.com/javascript-in-plain-english/developer-story-db-migrations-mongodb-edition-7b36db8f2654)开发者故事条目中，我讨论了我使用 MongoDB 构建整个数据库的个人工作流程。我们将通过关注一个公共用户集合的创建来检查这个开发者故事条目的焦点策略。以下是此类集合的模式创建逻辑:

```
***migrations/20200101000000-create_collection_users.js***module.exports = {
  async up(db) {
    return await db.creationCollection('users', {
      validator: {
        $jsonSchema: {
          bsonType: 'object',
          properties: {
            active: {
              bsonType: 'bool',
            },
            created_at: {
              bsonType: 'date',
            },
            details: {
              bsonType: 'object',
              properties: {
                email: {
                  bsonType: 'string',
                  pattern: '[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}',
                },
                password: {
                  bsonType: 'string',
                },
                username: {
                  bsonType: 'string',
                },
              },
            },
            email_confirmed: {
              bsonType: 'bool',
            },
            last_activity: {
              bsonType: 'date',
            },
            last_login: {
              bsonType: 'date',
            },
            token: {
              bsonType: 'string',
            },
          },
        },
      },
      validationLevel: 'strict',
      validationAction: 'error',
    })
  },
  async down(db) {
    return await db.collection('users').drop()
  },
}
```

在这个代码示例中，创建了一个具有两个级别的`users`集合，这意味着该集合不是完全平坦的。当检索存储在该集合中的文档时，它们将包含具有原始值和对象值的字段。文档中的`details`字段包含一个子文档，该子文档包含附加字段。这个集合中的文档中的各种字段需要根据请求信息的人的不同而变平或省略。

介绍完数据库模式后，我的混淆策略的下一个重要部分是使用[视图](https://docs.mongodb.com/manual/core/views/)。为了让这种策略发挥作用，需要为将要访问服务器 API 应用程序的每种类型的客户机提供一个视图。这个 developer story 条目详细描述了服务器 API 应用程序的创建，该应用程序服务于两种类型的客户机，即管理用户客户机和普通用户客户机。因此，需要为`users`系列创建两个单独的视图。以下是`users`集合的常规用户视图的创建逻辑:

```
***migrations/20200101000001-create_view_user_users.js***module.exports = {
  async up(db) {
    return await db.creationCollection('user_users', {
      pipeline: [
        {
          $project: {
            email: '$details.email',
            name: '$details.username',
          },
        },
      ],
      viewOn: 'users',
    })
  },
  async down(db) {
    return await db.collection('user_users').drop()
  },
}
```

这个视图非常简单，因为它只允许用户访问`users`集合中的两个字段`email`和`name`。正如您所看到的，这两个字段被展平并放到结果文档的顶层，而不是保留在它们原来的`details`子文档位置。此外，子文档中的`username`字段在视图中被简单地命名为`name`。这是允许通过常规用户视图看到的仅有的两个字段，因为这是对用户客户端应用程序有用的仅有的两个字段。为了让客户端应用程序与用户一起工作，`users`集合中包含的其他信息都不是必需的。讨论完用户视图后，让我们继续讨论`users`集合的管理员用户视图的创建逻辑:

```
***migrations/20200101000002-create_view_admin_users.js***module.exports = {
  async up(db) {
    return await db.creationCollection('admin_users', {
      pipeline: [
        {
          $project: {
            active: '$active',
            activity: '$last_activity',
            confirmed: '$email_confirmed',
            creation: '$created_at',
            email: '$details.email',
            login: '$last_login',
            name: '$details.username',
          },
        },
      ],
      viewOn: 'users',
    })
  },
  async down(db) {
    return await db.collection('admin_users').drop()
  },
}
```

这个视图基本上与普通用户的视图相同，但是多了几个来自`users`集合的字段。它包含常规用户视图中相同的两个字段，以及可以被视为来自`users`集合的元数据字段的大多数字段。就像在常规用户视图中一样，来自`details`子文档的`email`和`username`字段被带到结果文档的顶层，并且`username`字段的名称被简化为`name`。该视图中被访问的大多数其他字段的名称也被简化为更短的名称。缩短名称是我在数据库混淆策略中喜欢使用的另一种技术，它有两个目的。

第一个目的是使客户机在请求希望在 HTTP GET 响应中接收的特定文档字段时更容易引用这些字段。因为 HTTP GET 响应请求不能包含正文，所以所有的请求信息都必须在 URL 字符串中传递。为客户端应用程序提供一组更短、更简单的字段名，以便在这些请求中引用，这有助于缩短 URL 字符长度。第二个目的是提供进一步混淆数据库真实形状的能力。出于数据库安全的目的，最好不要与外界共享数据库的确切形状。改变从数据库中检索到的数据的形状以及更改字段名会加大客户认为的数据库形状与数据库实际形状之间的距离。

在介绍了数据库模式和视图创建脚本之后，让我们继续讨论直接与数据库交互的服务器 API 应用程序逻辑。正如我在[上一篇关于创建单一数据库接口的](https://medium.com/swlh/developer-story-single-database-interface-442dd6804424)文章中提到的，我更喜欢将我所有的数据库接口逻辑放在一个单一的 *db* 模块中。在那个 *db* 模块中，将会有一个单独的子模块，用于数据库中的每个集合。以下是出现在 *db* 模块内的*用户*子模块中的逻辑的简化版本:

```
***db/users.js***const db = require('./database')module.exports.read = async (query = {}, query_options = {}, multi = false) => {
  const options = Object.assign({}, { readPreference: 'primary' }, query_options)
  const collection = db.retrieveDB().collection('users')
  return multi ? await collection.find(query, options).toArray() : await collection.findOne(query, options)
}module.exports.admin_read = async (query = {}, query_options = {}, multi = false) => {
  const options = Object.assign({}, { readPreference: 'primary' }, query_options)
  const collection = db.retrieveDB().collection('admin_users')
  return multi ? await collection.find(query, options).toArray() : await collection.findOne(query, options)
}module.exports.user_read = async (query = {}, query_options = {}, multi = false) => {
  const options = Object.assign({}, { readPreference: 'primary' }, query_options)
  const collection = db.retrieveDB().collection('user_users')
  return multi ? await collection.find(query, options).toArray() : await collection.findOne(query, options)
}
```

如上所述，这个代码示例只是我自己的代码中通常出现的内容的简化版本。我的所有服务器 API 应用程序代码中总是包含的一些功能没有出现在此代码示例中，因为我只想关注与我的混淆策略相关的部分。该代码示例中的逻辑分为三个独立的部分。

`read()`函数由服务器应用编程接口应用程序内部的所有逻辑使用，要求不受限制地访问数据库中的所有信息。基本上，任何业务逻辑，如果不是特别面向满足直接客户对数据库数据的请求，都会使用这个函数，并在函数参数中指定所有必要的查询和选项信息。`admin_read()`功能仅由专门为管理用户客户端 HTTP GET 请求提供服务的逻辑使用。这将整个管理用户客户端数据检索逻辑流程与该功能及其下的所有内容隔离开来。`user_read()`功能仅由专门服务常规用户客户端 HTTP GET 请求的逻辑使用。这将整个常规用户客户端数据检索逻辑流程与该函数及其下的所有内容隔离开来。

以这种与读取操作相关的方式分离数据库接口逻辑，可以清楚地分离服务器 API 应用程序内部所包含的逻辑。此外，它还确保了如果在专门处理客户端 HTTP GET 请求的逻辑中存在错误，则更容易缩小错误可能位于的位置，或者位于应用程序逻辑中的某个位置，或者位于数据库视图逻辑的更深处。让其他开发人员更容易缩小系统中逻辑可能出现故障的位置通常会让他们的工作更容易，并为他们节省时间和精力。

现在您已经更好地理解了我个人混淆支持服务器 API 应用程序的数据库的策略，我希望您也可以在自己的 API 开发中应用这个策略。以这种方式划分您的逻辑的检索部分，并将重新塑造来自数据库的数据的责任下推到数据库系统本身，确实有助于清理服务器 API 应用程序中必需的逻辑。这也将允许您将更多的注意力集中在应用程序真正应该包含的所有业务逻辑上。请继续关注更多的开发人员故事条目，因为我继续在我的个人项目上取得进一步的进展。