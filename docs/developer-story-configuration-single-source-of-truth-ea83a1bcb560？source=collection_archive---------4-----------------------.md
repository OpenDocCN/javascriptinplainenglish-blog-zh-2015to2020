# 开发人员故事:配置真相的单一来源

> 原文：<https://javascript.plainenglish.io/developer-story-configuration-single-source-of-truth-ea83a1bcb560?source=collection_archive---------4----------------------->

![](img/ab5bce0aa628e39055f68d8bb7660f76.png)

原则上，我创建的所有服务器应用程序总是严格管理和验证所有传入的信息。显然，确保从客户端接收的任何数据都经过清理是绝对必要的，但在信任配置数据之前，我总是肯定要验证另一组数据。

之前我已经[讨论过](https://medium.com/@keithvictordawson/developer-story-process-management-is-key-5115d2886ccc)我个人对于 NodeJS 应用的过程管理策略。在那个开发者故事条目中，我用来管理应用程序进程的方法也提供了一种向应用程序交付被称为环境变量的配置数据的方式。在这个开发人员故事条目中，我将讨论我的个人策略，即验证所有的配置数据，并为应用程序逻辑的所有角落提供所有配置数据的单一真实来源。

提醒一下，通过流程管理系统交付给我的应用程序的配置数据如下所示:

```
***ecosystem.config.js***...
DB_HOSTS: 'localhost,localhost,localhost',
DB_NAME: 'db_test',
DB_PASSWORD: 'db_password',
DB_PORTS: '27017,27018,27019',
DB_USER: 'db_user',
RS_NAME: 'db_rs',
SERVER_PORT: '1234',
...
```

这是流程管理系统的配置文件，我用它来维护所有服务器应用程序的平稳运行，这些环境变量在初始化时被注入到应用程序中，并可通过所有 NodeJS 应用程序中可用的`process.env`全局变量来检索。正如我前面说过的，我对不简单地相信我在`process.env`全局变量上找到的东西非常严格。相反，我验证这个全局变量中包含的所有数据，并构建我自己的更易于编程使用的全局配置对象。

对于上面示例中指定的配置数据，验证该数据并使其全局可用的逻辑如下所示:

```
***env.js***const err = require('./util/error')
const logger = require('./log').app
const val = require('./util/validation')let env = {}module.exports.initialize = async () => {
  logger.info(`Initializing environment variables`) val.isNonEmptyString(process.env.DB_HOSTS, 'DB_HOSTS')
  const hosts = process.env.DB_HOSTS.split(',')
  val.isNonEmptyArray(hosts, 'DB_HOSTS', val.isNonEmptyString, 'host')
  env.DB_HOSTS = hosts if (hosts.length > 1) {
    val.isNonEmptyString(process.env.DB_PORTS, 'DB_PORTS')
    const ports = process.env.DB_PORTS.split(',')
    val.isNonEmptyArray(ports, 'DB_PORTS', val.isPortString, 'port')
    if (hosts.length !== ports.length) throw err.createError('ENV_HOSTS_PORTS_MISMATCH', FUNCTION_NAME)
    env.DB_PORTS = ports val.isNonEmptyString(process.env.RS_NAME, 'RS_NAME')
    env.RS_NAME = process.env.RS_NAME
  } val.isNonEmptyString(process.env.DB_NAME, 'DB_NAME')
  env.DB_NAME = process.env.DB_NAME val.isNonEmptyString(process.env.DB_PASSWORD, 'DB_PASSWORD')
  env.DB_PASSWORD = process.env.DB_PASSWORD val.isNonEmptyString(process.env.DB_USER, 'DB_USER')
  env.DB_USER = process.env.DB_USER val.isPortString(process.env.SERVER_PORT, 'SERVER_PORT')
  env.SERVER_PORT = process.env.SERVER_PORT logger.info(`Successfully initialized environment variables`)
}module.exports.retrieve = () => {
  const FUNCTION_NAME = 'env.retrieve'
  try {
    if (Object.keys(env).length === 0) throw err.createError('ENV_NOT_INITIALIZED', FUNCTION_NAME) return env
  } catch(e) {
    const error = err.createError(e, FUNCTION_NAME)
    logger.error(error.display)
    throw error
  }
}
```

这里发生了很多事情，其中一些我之前已经讨论过，一些我第一次在代码示例中展示。您可能注意到的一件事是在几个不同的地方使用了一个`logger`变量。我之前在[讨论了](https://medium.com/@keithvictordawson/developer-story-always-bring-receipts-ed0ddad4fbcd)我在所有 NodeJS 应用程序中实现日志功能的个人策略，这是使用这种功能的另一个例子。另一件事是通过使用`err`变量在整个代码示例中使用的错误处理。我在所有 NodeJS 应用程序中实现错误处理功能的个人策略将在以后的开发者故事条目中讨论。除了这两件事，这个代码示例中的大部分逻辑都是在`initialize()`函数中实现的，该函数利用`val`变量对应用程序初始化时提供的所有配置数据进行验证。

您会注意到，`val`变量是通过从应用程序另一部分的实用程序验证模块导入代码，在脚本顶部初始化的。在验证模块中有许多轻量级的函数，用于检查整个应用程序中的数据。这里主要使用它来检查大多数配置数据是否为非空字符串，但是在验证模块中还有许多其他有用的验证函数，它们在整个应用程序中有许多用途。其中一些验证逻辑是在[**validator**](https://www.npmjs.com/package/validator)NodeJS 包的帮助下实现的，如果您的应用程序中需要任何类型的字符串验证，我强烈建议您研究一下这个包。

虽然这里的大部分逻辑非常简单，只是简单的字符串验证，但我验证的主要内容之一是数据库主机的数量与数据库端口的数量相匹配。由于我主要将 MongoDB 用于我的大多数 NodeJS 应用程序，并且早期开发之后的所有情况都需要使用副本集数据库配置来实现 MongoDB 实例，所以包含了这个逻辑以确保主机和端口的两个列表匹配。如果这个检查失败了，那么整个验证过程就失败了，因为如果应用程序不能正确地连接到数据库，那么一开始启动应用程序还有什么意义呢？一旦所有的验证完成，并且在脚本顶部初始化的`env`变量的构造完成，全局对象就可以通过使用`retrieve()`函数在应用程序的任何地方进行检索。从这个函数的逻辑中您可能会注意到，试图在初始化之前检索全局配置对象将会抛出一个错误。这确保了在全局配置对象没有成功初始化的情况下，应用程序不会完成初始化。

您可能已经意识到，这个全局配置对象对于整个应用程序的设置具有巨大的价值。为了确保在应用程序中的任何地方检索它之前已经创建了它，以下代码出现在所有 my NodeJS 应用程序的启动脚本中:

```
***server.js***async function run() {
  logger.info(`Initializing application`) const env = require('./env')
  await env.initialize()
  ...
  })
}
```

全局配置对象在应用程序初始化过程的绝对开始时进行初始化，然后才会发生任何其他事情，包括连接到数据库或初始化任何其他应用程序模块。从这一点开始，当下面一行代码放在整个应用程序的任何模块的任何文件的顶部时，所有的配置数据都可以通过`env`变量获得:

```
const env = require(‘../env’).retrieve()
```

除了数据库连接过程所需的配置数据之外，应该在硬编码字符串通常出现的任何地方使用这些配置数据。将应用程序的所有硬编码字符串收集到一个地方更好的原因是，可以根据应用程序运行的环境上下文适当地指定它们。这些硬编码的字符串可以用于任何事情，从身份验证过程中使用的令牌化逻辑到应用程序与其他服务器系统的接口，这些服务器系统需要 API URL 和 ID 来与这些系统进行身份验证。

作为开发人员，您可以选择系统在多大程度上使用这种策略来将配置数据传播给应用程序的其他部分。但是，最好谨慎行事，最大限度地使用它，如果没有其他原因，利用这种策略将允许您验证所有指定的配置数据，以便您可以确保它在用于初始化您的应用程序时是正确的。

至此，您对我如何在所有 NodeJS 应用程序中验证和传播配置数据已经有了很好的了解。虽然这个策略不是我在开始实现 NodeJS 应用程序时立即开发的，但它无疑已经成为我现在在职业生涯和个人开发中创建的所有 NodeJS 应用程序的基础特性。

我真诚地希望我在这里的分享能够为您提供一个好主意，告诉您如何在所有 NodeJS 应用程序中实现这样一个系统。当我继续在我的个人项目上取得进一步进展时，请继续关注更多的开发者故事条目。