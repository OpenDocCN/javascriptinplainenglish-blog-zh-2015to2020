# 开发者故事:NodeJS 中的数据库迁移(MongoDB 版)

> 原文：<https://javascript.plainenglish.io/developer-story-db-migrations-mongodb-edition-7b36db8f2654?source=collection_archive---------3----------------------->

![](img/2dd4553b4a23b6801f93d8e99e4b63f0.png)

对于我的开发者故事中的这个条目，我想改变一下，从我到目前为止所写的冗长的[头脑](https://medium.com/@keithvictordawson/developer-diary-the-beginning-13448dbbb0c6) [转储](https://medium.com/@keithvictordawson/developer-story-step-1-db-7e2cf8291eb9)中走出一步，在那里我仅仅讨论了我对这个项目和总体软件开发的想法和感受。我想深入挖掘我实际工作的内容，并分享更多关于我为这个项目编写的代码的细节，以及我为什么编写这些代码背后的思考过程。在某些情况下，这可能包括我在写作时刚刚学到的东西，而在其他时候，它可能包括已经成为我长期发展过程的一部分的东西。这个特定开发者故事条目的内容将属于后一种类型。

在我职业生涯的早期，在我获得对数据库结构或数据库中实际包含的数据的任何控制或影响之前，我的那些负责监督系统这一关键部分的同事总是谈论它，好像它是一个巨大的、单一的、不可克服的挑战和负担，他们甚至不想考虑，更不用说进去接触或以任何方式干扰。这种思维方式对我来说从来没有意义，因为我认为如果你不信任你的数据库系统来做这些事情，那你为什么还要使用它们呢？我也经常想知道可以做些什么来改进系统，以消除使这种思维方式在我工作过的所有公司，似乎是整个软件开发社区中如此普遍的所有东西。

当我在香港的几家不同的公司工作时，我第一次看到了这种情况是如何在一个用于建立数据库结构的迁移脚本系统和用于用实际有用的数据填充该结构的种子脚本的帮助下得到缓解的。我在 Postgres 数据库和 MongoDB 数据库上都成功地实现了这一点，因此我知道这样的系统可以在 SQL 和非 SQL 环境中工作。我来到韩国后，在一家公司找到了一份工作，在那里，我不仅要设计和填充数据库，还要管理它，我清楚地知道如何完成所有这些与数据库相关的任务，同时将它们给我带来的压力和焦虑降至最低。

为了完成构建 MongoDB 数据库并填充数据的任务，我需要做的第一件事是找到一个健壮的系统来处理迁移过程。因为我在 NodeJS 工作，所以我搜索了为此目的构建的 NPM 包。在搜索和评估了几个相似但不同的包之后，我使用的是 [**migrate-mongo**](https://www.npmjs.com/package/migrate-mongo) ，在我写这篇文章的时候，它的版本是 7.0.1。这个包非常适合我的需求，因为它提供了我在迁移数据库时需要的所有特性，仅此而已。作为一个额外的好处，它需要我编写最少的代码，这意味着我可以将所有的注意力集中在将必要的逻辑实际构建到我的迁移和种子脚本中的任务上，而不必担心迁移过程中的任何其他事情。

每一个单独的迁移脚本在以后的编写和阅读中都不会更简单或更直接。如果我想编写一个迁移脚本，用一个简单的模式在我的数据库中创建一个简单的*用户*集合，我会编写如下代码:

```
***migrations/20190101000000-create_collection_users.js***module.exports = {
  async up(db) {
    return await db.creationCollection('users', {
      validator: {
        $jsonSchema: {
          bsonType: 'object',
          required: [ 'email', 'password', 'username' ],
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

这个例子甚至比大多数人可能需要的还要复杂，因为我更喜欢在我所有使用 MongoDB 数据库的应用程序中使用 MongoDB 的模式验证功能，而许多使用 MongoDB 的人可能不需要或不想要这样的功能。在这些情况下，每个函数中只需要一行代码，一行用于创建集合，一行用于删除集合。所以这个迁移脚本将被 **migrate-mongo** 用来创建或销毁 *users* 集合，这取决于您在迁移过程中的发展方向。

填充这个集合的种子脚本也几乎一样简单，甚至比我上面的例子还要简单。如果我想编写一个种子脚本，将一个测试用户添加到我的 *users* 集合中，我会编写以下代码:

```
***migrations/20200101000000-seed_collection_users.js***module.exports = {
  async up(db) {
    return await db.collection('users').insertMany([
      {
        email: 'testuser@test.com',
        password: 'test_password',
        username: 'Test User',
      }
    ], {})
  },
  async down(db) {
    return await db.collection('users').deleteMany({})
  },
}
```

所以这个种子脚本将被 **migrate-mongo** 用来向 *users* 集合添加一个用户，或者从 *users* 集合中删除所有用户，这取决于您在迁移过程中的发展方向。

现在，在我可以在一些方便的 NPM CLI 命令中使用上面的迁移和种子脚本之前，我只需要创建一个配置文件，其中包含有关将保存我的*用户*集合的数据库的详细信息，如下所示:

```
***config/test_config.js***module.exports = {
  mongodb: {
    url: 'mongodb://localhost:27017',
    databaseName: 'test_db',
  },
  migrationsDir: 'migrations',
  changelogCollectionName: 'changelog',
}
```

根据这个示例配置文件的内容，您的本地机器上需要有一个运行在端口 27017 上的 MongoDB 实例。需要注意的最重要的部分是，迁移和种子脚本都需要位于应用程序目录顶层的*迁移*目录中。另外，需要注意的最后一件重要事情是 **migrate-mongo** 按照这些脚本的名称在 *migrations* 目录中出现的顺序运行所有迁移和种子脚本。如果您看一下我在上面提供的迁移和种子脚本的示例名称，您可以看到我以这样的方式命名它们，即在迁移数据库时，迁移脚本将在种子脚本运行之前运行。正如您可能注意到的，文件名开头的数字是[ISO 8601](https://en.wikipedia.org/wiki/ISO_8601)——类似于精确到秒的特定日期的字符串。我总是为我的所有迁移和种子脚本建立一致的编号格式，以便它们不会混淆，并且在运行任何种子脚本之前，所有迁移脚本都以适当的顺序运行。

创建了迁移脚本、种子脚本和配置文件后，剩下唯一要做的事情就是创建一些有用的 NPM CLI 命令。我在所有 MongoDB 数据库应用程序中使用的特定 NPM CLI 命令如下:

```
***package.json***{
  ...,
  "scripts": {
    "migrate_down": "./node_modules/.bin/migrate-mongo down --file ./config/test_config.js",
    "migrate_up": "./node_modules/.bin/migrate-mongo up --file ./config/test_config.js",
    "migrate_status": "./node_modules/.bin/migrate-mongo status --file ./config/test_config.js"
  },
  ...,
}
```

根据这些命令，上面的配置文件需要位于应用程序目录顶层的 *config* 目录中，但是您可以将它放在您喜欢的任何地方，只要它在这些命令中被正确引用。至此，连接到数据库、创建一个*用户*集合以及播种已经创建的*用户*集合所需的一切都完成了。在将这些命令与 **migrate-mongo** 一起使用时要记住的是，为了安全起见，`migrate_down`命令一次只运行一个迁移文件，而`migrate_up`命令运行所有迁移文件，直到到达*迁移*目录中文件列表的末尾。最后，`migrate_status`命令将打印出所有迁移脚本的列表，每个脚本都有一个指示符，指明它是正在等待迁移还是已经完成了迁移。

以上是我的个人系统的详细内容，该系统用于干净有效地创建和填充 MongoDB 数据库。这是我在看到这样的事情是可能的，并被赋予构建和管理 MongoDB 数据库的责任之后开发的一个系统。为了增加安全性和关注点的分离，在专业设置中，我也倾向于在开发分支上创建迁移脚本，在完全隔离的种子分支上创建种子脚本，以便现有数据不会受到任何干扰。对于个人项目和其他低风险项目来说，这无疑是大材小用，但是如果您有严重的数据完整性问题，这是一个很好的实践。由于这个开发者故事条目已经足够长了，我将在下一个条目中提供与此处相同的过程和代码细节，以便在我的个人项目取得进一步进展时，请继续关注。