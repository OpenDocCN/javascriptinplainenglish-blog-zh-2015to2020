# 节点。JS 和关系数据库

> 原文：<https://javascript.plainenglish.io/node-js-and-relational-databases-2198152732b6?source=collection_archive---------5----------------------->

![](img/51d3c5962c046ff8117a0b5203f8e382.png)

Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

数据存储是任何计算机应用程序的重要组成部分。需要存储收集的数据以供以后分析，或者需要检索数据以生成所需的输出。存储内存的方式有很多种——文件存储(包括文本和/或二进制选项)、关系数据库、 [NoSQL](https://en.wikipedia.org/wiki/NoSQL) 系统，我确定我错过了几种。

[SQL](https://en.wikipedia.org/wiki/SQL) 是许多不同关系数据库管理系统(又名 *RDBMS* )共享的标准。实现的 SQL 有时与官方标准不同。如果您在同一个数据库平台内，这些差异不会有太大影响。然而，当您开始在不同的数据库供应商之间，甚至在同一个供应商的不同版本之间转移时，差异就变得很重要了。在关系系统和非关系系统之间转换会带来更多的工作。

您可以使用特定于 DB 的库来与您的数据库对话。例如，您可以做`npm install sqlite3 mysql pgsql,mssql`来引入与 SQLite、MySQL/MariaDB、PostgreSQL 或 Microsoft SQL Server 系统通信的能力——您通常只会看到应用程序使用其中的一个。

使用这个特定的数据库库确实可以工作，并且可以展示数据库系统的全部功能。如果数据库发生了变化——或者更糟糕的是，直到实际的系统安装完成后，您才知道数据库会是什么——那么这个问题很快就会解决。一种类型的数据库的库不能正确地与另一种类型的数据库对话。

如果有一种方法可以让您编写代码，而不必担心应用程序可能会使用哪个数据库系统，这不是很好吗？

进入数据库抽象的概念。简而言之，这是一个在特定数据库库之上呈现一个层的库。这一层将数据库细节从应用程序代码中分离出来。您只需编写一次代码，然后如果/当数据库需要更改时，您只需做一个小的配置更改(并安装可能需要的支持库)。但是您的代码本身——以及它所有的查询优点——不会改变。

这里需要一个免责声明。如果您使用一个数据库抽象系统，然后使用一个非常特定于您的数据库的特性，那么当数据库供应商发生变化时，您的代码肯定需要更改来解决这个问题。

例如，存储过程通常是特定于平台的 SQL 扩展——如果使用该功能，需要在新的数据库系统中重新创建。但这并不意味着不能使用存储过程。他们只是需要有意识地使用这些暗示。

## 抽象层与对象关系映射系统

抽象层和对象关系映射系统之间有一些重叠。两者都提供了一些与数据库对话的自动化方式。ORM 则更进一步，试图理解你的数据库结构。有了抽象层，您可以创建一个 INSERT 语句，该语句可以在任何配置的 DB 系统上工作。使用 ORM，您可以扩展 insert，用一个命令创建新记录和相关记录。

ORM 在某些情况下可能是强大而方便的。我自己的经验表明，它们是有代价的，使用它们会很快导致不必要的工作量。(如果有人感兴趣，我可能会在某个时候发布相关内容)

你可能认为这里应该提到顺序。Sequelize 是一个 ORM，不仅仅是简单的数据库抽象。我对 Sequelize 的经验有限，但它从来就不适合我。

如果我想使用 ORM，我会使用 Objection.JS. Objection 是建立在 Knex.JS 之上的。我试图为一个大型项目实现 Objection，最终不得不退回到只使用 Knex。不是因为异议不可行，而是因为我们的用例打破了异议应该对关系做什么的限制。(这*可能*是开发人员的一些糟糕的设计选择——呃，就是我…)

## 节点。JS 选项

我肯定我错过了一些很棒的选择，但对我来说 [Knex。JS](http://knexjs.org/) 是唯一真正的选择。它是轻量级的，并试图尽可能的非个人化。

Knex 非常简单，非常符合普通 SQL 语言。

```
const records = knex('tasks')
    .where({user_id: 5})
    .orderBy('task_date');
```

这个 JS 代码会产生`select * from tasks where user_id = 5 order by task_date`。括号内有许多选项，并且都可以是动态的。虽然 SQL 很简单，但是 Knex 所代表的功能非常强大。

## Knex 入门。射流研究…

在 [Knex 阅读文档。JS](http://knexjs.org/) 。这是 Knex 信息的事实上的“来源”。

入门就像做一个`npm install`一样简单。

```
mkdir dbProject
cd dbProject
npm init -y
npm  install knex pg
```

这将建立一个练习项目并安装 knex。我们还安装了“pg”库来与 PostgreSQL 数据库对话。如果你打算使用 mysql，那么用“MySQL”替换“pg”。如果您要使用不同的数据库系统，那么只需确保您安装了一个库来与它对话。Knex 文档给出了更多的信息。

现在我们有了 knex，我们需要设置一些东西。我喜欢用 NPM 的剧本来做这件事。所以我修改了我的`package.json`的剧本部分

```
"scripts": {
  "knex": "knex",
  "test": "echo \"Error: no test specified\" && exit 1"
},
```

现在我们可以在项目目录中发出命令`npm run knex init`。这将创建一个`knexfile.js`文件。检查该文件，您将看到一个标准节点模块返回一个具有特定于环境的配置的对象。这允许您使用 SQLite 进行测试，使用 MySQL 进行开发，但是如果您愿意，可以使用 MSSQL 进行生产。(这听起来是个糟糕的组合，但却是可行的)。您可以根据环境的需要改变连接。您可以删除“暂存”和“生产”对象，只使用“开发”对象。

您设置了`client`属性来匹配您正在使用的数据库系统。然后用连接到服务器的详细信息设置`connection`属性——主机名/ip、用户名、密码等。

完成后，您现在可以与数据库对话并对其执行命令。

## 迁移

Knex 使用所谓的“迁移”来创建、删除和更改数据库元素。这些迁移文件可以设置索引、外键，如果需要，甚至可以运行定制的 SQL。这里的目的是，我们可以“应用”尚未应用到我们的数据库的迁移，并更新系统。或者将系统返回到添加有问题的索引(或表)之前的状态。

Knex 为创建和运行迁移提供了一种方便的方法。

```
npm run knex migrate:make create_tasks_table
```

这将在`migrations`目录下生成一个文件。如果目录不存在，则创建该目录。您可以在`knexfile.js`文件中指定创建目录的位置以及如何命名它。生成的文件以`20200807222531_create_tasks_table`的形式命名。即年、月、日、小时、分钟、秒，后跟描述。尽管允许更改名称，但不要改变该名称的格式，除非它已经应用于数据库。(如果您应用更改，然后更改文件名，然后尝试回滚，您将遇到错误，因为原始文件名不再存在。)

该文件的内容如下所示:

```
exports.up = function(knex) {};exports.down = function(knex) {};
```

然后我们可以调用 Knex 模式系统来创建和删除我们的表，无论这个迁移是否被应用。

```
exports.up = function(knex) {
  return knex.schema.createTable('tasks', table => {
    table.increments('id')
    table.string('name')
 })
};exports.down = function(knex) {
  return knex.schema.dropTable('tasks')
};
```

需要注意的是`up`和`down`方法必须返回一个承诺。请参见[模式文档](http://knexjs.org/#Schema-Building)了解您在这里可以做什么的更多细节。

现在我们可以运行`npm run knex migrate:latest`将更改应用到数据库。如果您的连接已经设置好并且没有错误，那么您的数据库现在应该有一个 tasks 表。但是它也有“迁移”和“迁移锁”表。这些有助于跟踪应用了哪些迁移。

## 在模块中使用 Knex

最后，为了在我们的应用程序中使用 Knex，我们需要调用它。

我通常创建一个`_connection.js`文件，其中包含如何连接到数据库的细节。这个文件使用适当的配置设置调用 Knex 对象。

```
const config = require('./knexfile.js')
const Knex = require('knex')
module.exports = Knex(config[process.env.NODE_ENV || 'development'])
```

我这样做是因为有时我需要一些更复杂的东西。我可能正在设置多个连接，或者做其他与设置数据库相关的工作。得益于模块系统，knex 对象被当作[单例](https://en.wikipedia.org/wiki/Singleton_pattern)处理。

一旦完成，我就创建一个我称之为“模型”的东西来做数据工作。

```
// src/models/task.jsconst knex = require('./connection.js')async function getAll() {
  return knex('tasks').orderBy('name')
}module.exports = {
  getAll
}
```

现在，我可以要求该文件，并在应用程序中的任何需要的地方使用它:

```
// src/services/current.jsconst TaskModel = require('../models/task.js')
console.log(TaskModel.getAll())
```

如果我的 tasks 表中有数据，我将得到一个对象数组来表示表中的每条记录。

嘣！我们拥有潜力巨大的 RDBMS 连接。

当然，细节变得有点复杂，但是这应该让你开始并指向正确的方向。