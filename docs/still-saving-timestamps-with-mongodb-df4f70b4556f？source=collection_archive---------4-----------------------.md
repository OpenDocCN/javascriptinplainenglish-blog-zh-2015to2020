# 还在用 MongoDB 保存时间戳？

> 原文：<https://javascript.plainenglish.io/still-saving-timestamps-with-mongodb-df4f70b4556f?source=collection_archive---------4----------------------->

## 解密 MongoDB 的 id

我经常看到与后端应用程序开发人员一起工作的一件事是他们对保存时间戳的痴迷。我知道保存时间戳、标记创建和更新在几种情况下是不可避免的，从调试和日志记录，到根据对象或实体第一次创建的时间对其进行排序。然而，如果您使用的是 MongoDB，您并不需要显式地节省创建时间。

![](img/c143d0c9a08bb7d97fcd94ff1d30d7cf.png)

source: giphy

# 放弃“createdAt”

大多数 JavaScript 开发人员都熟悉 MongoDB。当您与 JavaScript/NodeJS 开发人员一起工作时，它通常是 goto 数据库。众所周知，MongoDB 的`_id`属性可以用来惟一地标识我们的文档。但是,`_id`属性也可以用来获取文档的创建时间。

看起来像随机字符集合的`_id`并不完全随机。`_id`存储为 12 字节长的值，其中 4 字节代表对象创建的时间戳，5 字节为随机字符，其余 3 字节用作从随机值开始自动递增的计数器。

时间戳是这篇文章的主题。它以纪元的形式表示。有一个方便的 API 允许我们访问这个时间戳。我们一会儿就会看到这一点。

将字节分成时间戳、随机字符和自动递增计数器，这使得 MongoDB 成为寻求开发分布式系统的人们的普遍选择。这种分离消除了数据库 id 冲突的风险。当一个 id 代表多个行/文档时，会发生数据库 id 冲突。打个比方，这就像两个人有相同的社会安全号码或联系号码。

# 从 _id 获取时间

要从`_id`属性中提取时间戳，第一步是安装节点。节点安装程序可以在[这里](https://nodejs.org/en/download/)下载。

假设您已经启动并运行了 Node，您将需要下载一个名为`mongodb`的依赖项

`mongodb`包是官方的 MongoDB 驱动程序，允许您的 JavaScript 代码与 MongoDB 数据库通信。但是，出于本演练的目的，您不需要安装 MongoDB。mongodb 驱动程序足以涵盖本文的范围。该软件包可以在[这里](https://www.npmjs.com/package/mongodb)找到，并且可以使用

```
npm install mongodb
```

安装包后，创建一个 JavaScript 文件。从导入 mongodb 开始。

```
const mongodb = require('mongodb');
```

接下来，我们从导入的 mongodb 中拉出`ObjectID`。请注意 ObjectID 中的大写“O”和“ID”。

```
const myObject = mongodb.ObjectID;
```

myObject 现在可以使用 new 关键字创建对象 id 的实例。

```
const object = new myObject();
```

最后，您需要在创建的实例上调用函数`getTimestamp`。

```
const timestamp = object.getTimestamp()
```

您可以根据需要进一步格式化时间戳。例如，以小时和分钟的形式显示时间，

```
let hours = timestamp.getHours()
let minutes = timestamp.getMinutes();
console.log("Object created at - "+hours+":"+minutes);
```

![](img/3c87597121e96165990b8fe9d017d134.png)

带有对象分解的完整代码可以在下面找到。

Getting timestamp from MongoDB’s GUID

这篇文章的相关文档和基础可以在[这里](https://docs.mongodb.com/manual/reference/method/ObjectId/)获得。尽管这消除了存储创建日期和时间的需要，但您可能仍然需要存储更新时间戳。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**