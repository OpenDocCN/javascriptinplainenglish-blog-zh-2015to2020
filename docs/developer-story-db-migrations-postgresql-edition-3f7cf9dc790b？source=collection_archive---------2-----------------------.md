# 开发者故事:NodeJS 中的数据库迁移(PostgreSQL 版)

> 原文：<https://javascript.plainenglish.io/developer-story-db-migrations-postgresql-edition-3f7cf9dc790b?source=collection_archive---------2----------------------->

![](img/ddb8b58d79966ae471c86eb46357354f.png)

正如我在[之前的](https://medium.com/@keithvictordawson/developer-story-db-migrations-mongodb-edition-7b36db8f2654)开发者故事条目中提到的，与讨论如何为 MongoDB 数据库创建 NodeJS 数据库迁移的条目内容不同，这篇条目将讨论如何为 Postgres 数据库创建 NodeJS 数据库迁移。在搜索和评估了几个相似但不同的包之后，我选择了 [**db-migrate**](https://www.npmjs.com/package/db-migrate) 和支持 [**db-migrate-pg**](https://www.npmjs.com/package/db-migrate-pg) 包，在撰写本文时，它们的版本分别是 0.11.6 和 1.0.0。 **db-migrate** 包提供了运行数据库迁移的核心功能，而 **db-migrate-pg** 包是一个插件，提供了处理 Postgres 数据库系统的特定功能。

与 MongoDB 迁移脚本文件不同，MongoDB 迁移脚本文件可以在单个文件中包含特定迁移步骤向上和向下的所有代码，这个系统需要创建三个单独的文件，以便实现具有向上和向下逻辑的特定迁移步骤。首先，如果我想编写一个迁移脚本，在我的数据库中创建一个简单的 *users* 表，我将编写以下代码:

```
***migrations/sqls/20190101000000-create_table_users_up.sql***CREATE TABLE users (
  user_id uuid NOT NULL PRIMARY KEY DEFAULT gen_random_uuid(),
  email text NOT NULL,
  password text NOT NULL,
  username text NOT NULL
);
```

相反，如果我想编写一个迁移脚本，从我的数据库中删除那个 *users* 表，我会编写如下代码:

```
***migrations/sqls/20190101000000-create_table_users_down.sql***DROP TABLE users;
```

为了完成 *users* 表的迁移脚本创建过程，我将编写以下代码:

```
***migrations/20190101000000-create_table_users.js***var dbm
var type
var fs = require('fs')
var path = require('path')
var Promiseexports.setup = (options) => {
  dbm = options.dbmigrate
  type = dbm.dataType
  Promise = options.Promise
}exports.up = (db) => {
  const filePath = path.join(__dirname, 'sqls', '20190101000000-create_table_users_up.sql')
  return new Promise((resolve, reject) => {
    fs.readFile(filePath, { encoding: 'utf-8' }, (err, data) => {
      if (err) return reject(err)
      resolve(data);
    })
  })
  .then((data) => {
    return db.runSql(data)
  })
}exports.down = (db) => {
  const filePath = path.join(__dirname, 'sqls', '20190101000000-create_table_users_down.sql')
  return new Promise((resolve, reject) => {
    fs.readFile(filePath, { encoding: 'utf-8' }, (err, data) => {
      if (err) return reject(err)
      resolve(data);
    })
  })
  .then((data) => {
    return db.runSql(data)
  })
}
```

创建了这三个文件并将其放在应用程序目录中的正确目录中，您将能够创建一个 *users* 表，并使用一个迁移脚本删除它。对于上面的文件内容，要记住的关键是 Javascript 文件应该位于应用程序目录顶层的*迁移*目录中，而 SQL 文件应该位于*迁移*目录内的*SQL*目录中。另一件需要注意的事情是，就像在 MongoDB 迁移脚本命名案例中一样，这里所有的文件名都以类似于 ISO 8601 的字符串开始。因为有三个文件对应于一个迁移脚本，所以请确保通过对所有相关文件使用相同的日期字符串将它们全部逻辑分组，并按照您希望构建数据库的正确方式对它们进行排序。例如，确保所有依赖于引用的表都是在它们所依赖的表之后创建的，并将所有种子脚本放在迁移脚本之后。这将确保在运行迁移脚本时不会出现意外。

如果您碰巧浏览了 **db-migrate** 和 **db-migrate-pg** NPM 包的文档，您可能会发现，在像我上面所做的那样创建和删除一个表的情况下，可以在一个迁移脚本文件中编写所有的逻辑。但是我的数据库从来不仅仅需要构建简单的表。我总是需要创建扩展、创建函数，并且除了构建它们之外，还需要播种我的表，所以在那些情况下，我需要使用上面的策略来定义至少一些我的迁移脚本和所有我的播种脚本。这是因为这些包中内置的用于实现单个迁移脚本文件解决方案的功能不够强大，不足以包含满足所有这些需求的功能。结果，因为在编写类似和重复的代码(如我的所有 PostgreSQL 迁移代码)时，我更喜欢一致性而不是简单性，所以我选择使用上述策略编写我的所有迁移脚本。这是为了确保创建和删除表的过程与创建和删除函数或扩展的过程完全相同。

现在，如果我想编写一个种子脚本，将一个测试用户添加到我的 *users* 表中，我将编写以下代码:

```
***migrations/sqls/20200101000000-seed_table_users_up.sql***INSERT INTO users VALUES
  (DEFAULT, 'testuser@test.com', 'test_password', 'Test User');
```

相反，如果我想写一个从我的数据库中删除该用户的脚本，我会写如下:

```
***migrations/sqls/20200101000000-seed_table_users_down.sql***DELETE FROM users;
```

为了完成 *users* 表的种子脚本创建过程，我将编写以下代码:

```
***migrations/20200101000000-seed_table_users.js***var dbm
var type
var fs = require('fs')
var path = require('path')
var Promiseexports.setup = (options) => {
  dbm = options.dbmigrate
  type = dbm.dataType
  Promise = options.Promise
}exports.up = (db) => {
  const filePath = path.join(__dirname, 'sqls', '20200101000000-seed_table_users_up.sql')
  return new Promise((resolve, reject) => {
    fs.readFile(filePath, { encoding: 'utf-8' }, (err, data) => {
      if (err) return reject(err)
      resolve(data);
    })
  })
  .then((data) => {
    return db.runSql(data)
  })
}exports.down = (db) => {
  const filePath = path.join(__dirname, 'sqls', '20200101000000-seed_table_users_down.sql')
  return new Promise((resolve, reject) => {
    fs.readFile(filePath, { encoding: 'utf-8' }, (err, data) => {
      if (err) return reject(err)
      resolve(data);
    })
  })
  .then((data) => {
    return db.runSql(data)
  })
}
```

除了 up 和 down 步骤中引用的两个文件名之外，此种子脚本文件的内容与本条目前面的迁移脚本文件的内容相同。从上面所有迁移和种子脚本文件的内容可以看出，与 MongoDB 迁移过程的迁移和种子脚本相比，PostgreSQL 迁移过程需要编写更多的代码，特别是在执行 SQL 文件之前需要加载它们的地方。这是一个相当多余的过程，导致必须为每个迁移和种子脚本编写大量相同的代码。在大多数情况下，这是一件坏事，通常应该尽可能避免。然而，在我看来，这种冗余代码的具体情况并没有那么糟糕，因为逻辑并不太复杂，只是在将它们传递给执行函数之前加载一些 SQL 文件内容。如果像我一样，您喜欢将 Javascript 和 SQL 代码清楚地隔离在单独的文件中，那么我在上面提供的解决方案是最简单不过的了。创建了这三个文件并将其放在应用程序目录中的正确目录中，您将能够在一个迁移脚本中向*用户*表添加一个测试用户，并从*用户*表中删除所有行。

现在，在我可以在一些方便的 NPM CLI 命令中使用上面的迁移和种子脚本之前，我只需要创建一个配置文件，其中包含有关数据库的详细信息，该数据库将保存我的 *users* 表，如下所示:

```
***config/test_config.json***{
  "defaultEnv": "test",
  "test": {
    "driver": "pg",
    "host": "localhost",
    "schema": "public",
    "database": "postgres",
    "user": "postgres"
  }
}
```

根据这个示例配置文件的内容，需要有一个 Postgres 实例运行在您的本地机器上，并且有一个名为`postgres`的用户可以访问它。通过`defaultEnv`设置，我们不必在 NPM CLI 命令中指定额外的参数，默认情况下将使用这里定义的配置设置。

创建了迁移脚本、种子脚本和配置文件后，剩下唯一要做的事情就是创建一些有用的 NPM CLI 命令。我在所有 Postgres 数据库应用程序中使用的特定 NPM CLI 命令如下:

```
***package.json***{
  ...,
  "scripts": {
    "migrate_down": "db-migrate down -c 1 --config ./config/test_config.json",
    "migrate_up": "db-migrate up -c 1 --config ./config/test_config.json",
    "migrate_up_all": "db-migrate up --config ./config/test_config.json"
  },
  ...,
}
```

根据这些命令，上面的配置文件需要放在应用程序目录顶层的*配置*目录中，但是你可以把它放在你喜欢的任何地方，只要它在这些命令中被正确引用。至此，连接到数据库、创建一个 *users* 表和创建一个 *users* 表所需的一切都完成了。在将这些命令与 **db-migrate** 一起使用时要记住的是，为了安全起见，`migrate_down`命令一次只运行一个迁移文件，而`migrate_up`命令做同样的事情，但方向相反。为了避免运行单独的迁移脚本来完全完成数据库迁移，可以运行`migrate_up_all`命令，以便运行所有迁移文件，直到运行*迁移*目录中文件列表的末尾。不幸的是， **db-migrate** NPM 包中没有 status 命令，所以我无法创建 NPM CLI 命令来检查数据库的迁移状态。为了找出我在迁移过程中的位置，我通常运行一次`migrate_up`命令，然后如果需要的话，我可以向下迁移。

以上是我的个人系统的详细内容，该系统用于干净有效地创建和填充 Postgres 数据库。这是我在看到这样的事情是可能的，并被赋予建立和管理 Postgres 数据库的责任后开发的一个系统。为了增加安全性和关注点的分离，在专业设置中，我也倾向于在开发分支上创建迁移脚本，在完全隔离的种子分支上创建种子脚本，以便现有数据不会受到任何干扰。对于个人项目和其他低风险项目来说，这无疑是过量的，但如果您有严重的数据完整性问题，这是一个很好的实践。既然我已经详细介绍了 SQL 和 NoSQL 数据库的迁移过程，我的下一篇开发者故事将更多地关注服务器开发，因为我目前正在从事我个人项目的这一部分。随着我个人项目的进一步进展，请继续关注我的下一篇详细介绍这一过程和其他内容的文章。