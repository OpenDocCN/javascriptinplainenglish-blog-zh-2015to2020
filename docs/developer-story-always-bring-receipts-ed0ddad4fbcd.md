# 开发商故事:永远带上收据

> 原文：<https://javascript.plainenglish.io/developer-story-always-bring-receipts-ed0ddad4fbcd?source=collection_archive---------10----------------------->

![](img/123327af66e4298ddeb9e36cc49dd829.png)

我知道在这一点上我可能听起来有点重复，宣称软件开发过程的许多不同部分是至关重要的，包括[过程管理](https://medium.com/@keithvictordawson/developer-story-process-management-is-key-5115d2886ccc)和 [API 结构](https://medium.com/@keithvictordawson/developer-story-designing-a-proper-server-api-structure-5a7a3a7a34fe)。但是，当涉及到收集和维护应用程序中活动的收据时，软件开发过程的另一个真正重要的部分，正确地做这件事的唯一方法是通过使用健壮的日志记录系统。为了使这样的记录系统有用，它需要可靠并且在整个系统中一致地使用，以维护整个系统中发生的所有活动的完整和一致的记录。如果没有这种活动的完整记录，就很难准确地诊断软件系统不时产生的任何错误的原因。此外，在需要个人用户活动跟踪功能的系统的情况下，适当部署的日志记录系统将确保维护所有用户活动的记录，以供以后查看。如果没有一个高质量的日志记录系统，没有人能够真正知道软件系统中已经发生或正在发生的事情。

当我在以前的一家公司中独自承担设计、构建和管理整个服务器软件系统的任务，同时设计系统所需的所有不同模块时，我首先寻求的事情之一是能够满足我的即时开发需求的日志记录系统。第一个需求是跟踪进入系统的请求，以及服务器响应每个请求需要多长时间和响应大小的细节。第二个需求是跟踪关键的操作活动以及系统中发生的所有错误。因为这个应用程序是使用 NodeJS 开发的，所以我在 NPM 搜索潜在的日志解决方案。结果是，我的搜索发现我需要使用两个独立的日志解决方案来为我正在开发的应用程序创建整个日志系统。

为了满足我的第一个需求，即请求日志记录，我选择了一贯流行和老套的 [**摩根**](https://www.npmjs.com/package/morgan) ，在我写这篇文章时，它的版本是 1.9.1。根据**摩根**的自述，它应该被用作 HTTP 请求记录器中间件，这正是我在我的应用程序中使用它的方式。下面是我如何使用 **morgan** 在我的应用程序中实现主请求日志逻辑:

```
***log/request.js***function pad(num) {
  return `${num > 9 ? '' : '0'}${num}`
}module.exports = (logsDirectory) => {
  const messageFormat = `:date[iso] - ':method :url :status' :res[content-length] - :response-time ms`
  const stream = require('rotating-file-stream')((time, index) =>
  {
    return !time ? 'request.log' : `request_${time.getFullYear()}-${pad(time.getMonth() + 1)}-${pad(time.getDate())}-${index || '0'}.log`
  }, {
    interval: '1d',
    path: logsDirectory,
  }) return require('morgan')(messageFormat, { stream })
}
```

在这个代码示例中有一些事情需要注意。首先，我为记录的每个请求定义了所需的消息格式。我在这里定义的格式完全是自定义的，适合我的特定日志记录需求，但这绝不是您可以在日志消息中使用的唯一格式。有一个由 morgan 提供的预定义格式的扩展列表，以及一个可以包含在自定义日志消息格式中的单个请求详细信息的长列表。有关日志消息格式的所有细节和正确语法，请参见 mor gan**文档。我保证他们会提供您所需要的一切，以确保您可以收集关于您的应用程序所接收的所有请求的所有所需细节。**

在这个代码示例中需要注意的另一件事是，我使用另一个名为 [**的 NPM 包【旋转文件流**](https://www.npmjs.com/package/rotating-file-stream) 定义了一个要传递给**摩根**的流。由于我创建的应用程序不仅要在开发环境中运行，还要在生产环境中运行，因此我需要清除的一个重要障碍是确保我的请求日志按照合理的时间间隔进行划分。显然，如果您的系统将接收大量传入请求，如果不定期轮换，这些请求的日志文件可能会变得相当大。我不会在这里详细讨论文件流是如何工作的，但是我鼓励你通读一下 **rotating-file-stream** 的文档，看看它们提供了哪些选项来决定你的日志存储在哪里以及如何存储。关于循环文件流，我的代码示例中需要注意的唯一细节是，我选择将各个日志文件划分为 24 小时的块，使用日志文件创建日期命名，并附加一个索引，以防应用程序重新启动，从而为当天创建一个新的日志文件。此外，生成的每个日志文件的名称前都会加上`request_`，以区别于日志文件目录中的其他类型的日志文件。最后，这个代码示例中导出的函数返回一个 **morgan** logger 中间件函数，稍后当我展示如何将请求日志记录合并到应用程序的整体逻辑中时，这个函数会很有用。

为了满足我的第二个需求，也就是关键活动和错误日志，我选择了更流行的 [**winston**](https://www.npmjs.com/package/winston) ，在我写这篇文章的时候，它的版本是 3.2.1。根据 winston 的自述文件，这是一个简单通用的日志库。下面是我如何使用 **winston** 在我的应用程序中实现主要的关键活动和错误记录逻辑:

```
***log/app.js***const { createLogger, format, transports } = require('winston')
require('winston-daily-rotate-file')exports = module.exports = (logsDirectory) => {
  return createLogger({
    format: format.combine(
      format.timestamp(),
      format.align(),
      format.printf((info) => `${info.timestamp} - ${info.level.toUpperCase()} - ${info.message}`)
    ),
    level: 'info',
    transports: [
      new transports.DailyRotateFile({
        filename: 'app_%DATE%.log',
        datePattern: 'YYYY-MM-DD',
        dirname: logsDirectory
      })
    ]
  })
}
```

与请求日志记录代码示例一样，该代码示例中有一些需要注意的事项。首先，使用从 **winston** 导入的一些东西，我定义了我想要的日志消息打印格式。与**摩根**一样， **winston** 为创建定制日志消息格式提供了许多不同的选项。但与摩根的**不同的是， **winston** 并没有真正提供任何预定义的日志消息格式供您使用。我怀疑这样做的原因是因为 **winston** 的创建者希望它被用作一个通用的日志记录系统，所以他们不想让任何人知道他们如何使用日志记录功能，而是简单地提供所有必要的工具来构建任何所需的自定义日志消息。我鼓励您阅读 winston 的文档，了解如何使用这个日志系统创建您自己的定制日志消息的详细信息。**

在这个代码示例中需要注意的另一件事是我定义所有日志消息的传输或目的地的地方。我使用一个名为[**Winston-daily-rotate-file**](https://www.npmjs.com/package/winston-daily-rotate-file)的每日文件轮换 NPM 包，也是由 **winston** 的创建者提供的，将我的所有日志文件分成 24 小时的块，就像使用 **morgan** 创建的日志文件一样，所有日志文件的名称都包含文件创建的日期。此外，生成的每个日志文件的名称前都会加上`app_`，以区别于日志文件目录中的其他类型的日志文件。幸运的是，由于 **winston** 是一个如此广泛使用的日志解决方案，因此 **winston** 的创建者和其他开发者已经创建了许多不同的传输 NPM 包。我建议搜索 npmjs.com，看看你的日志信息有多种不同的存储方式。最后，该代码示例中导出的函数返回一个 logger 对象，该对象可以在任何地方用来生成日志消息，并且在我稍后展示如何将错误日志记录合并到应用程序的整体逻辑中时会很有用。

您可能已经注意到，在上面的两个代码示例中，两个导出的函数都接收了一个日志目录路径参数。这个日志目录路径用来告诉 **morgan** 和 **winston** 应该在哪里创建所有的日志文件。以下代码示例显示了日志目录路径是如何生成的:

```
***log/index.js***const fs = require('fs')
const logsDirectory = require('path').join(process.env.CWD || process.cwd(), 'logs', 'app')
fs.existsSync(logsDirectory) || fs.mkdirSync(logsDirectory, { recursive: true })exports = module.exports = {
  app: require('./app')(logsDirectory),
  req: require('./request')(logsDirectory)
}
```

在这个代码示例中需要注意的关键事情是，我从顶级应用程序目录定义了预期的日志目录路径， *logs/app* ，并检查它是否已经存在，如果不存在，则创建它。一旦日志目录被保证存在，日志目录路径就被传递给两个模块中的每一个，我在前面的开发人员故事条目中为这两个模块提供了代码示例。初始化两个日志记录系统后，最后要做的事情是在应用程序初始化逻辑中的适当位置使用它们。下面的代码示例展示了我如何在所有 NodeJS 应用程序的启动脚本中实现这一点:

```
***server.js***const logger = require('./log').apprun().catch(e => {
  logger.error(`Application has encountered an error: {e.message}`)
  return process.exit(0)
})async function run() {
  logger.info(`Initializing application`)
  ...
  const app = require('express')()
  app.use(require('./log').req)
  ...
  app.listen(1234, () => {
    logger.info('Server listening to port 1234')
  })
}
```

在这个代码示例中，我使用了我在前面的代码示例中详述的两个日志记录系统。首先，最重要的部分是我通过在启动脚本的第一行调用`require(‘./log’)`来初始化*日志*模块，以确保在初始化两个日志记录系统之前日志目录存在。在初始化*日志*模块时，我还导入了关键操作活动和错误日志模块，以便在启动脚本中直接使用。在执行所有应用程序初始化逻辑的 catch-all 函数和 execution 函数中，您可以看到我使用了`logger.error`和`logger.info`函数来记录关于正在执行的初始化过程的当前步骤的信息，以及在应用程序运行期间可能遇到的任何错误。除了使用错误记录模块之外，我还从 *log* 模块导入请求记录模块，并将导出的函数作为中间件使用在我已经启动供我的应用程序使用的 Express 应用程序中。这是唯一需要做的事情，以确保对应用程序的所有 HTTP 请求都记录到正确的位置。从这个代码示例中可以看出，请求日志记录模块是被动使用的，只需要在初始化时作为中间件导入一次，而错误日志记录模块的使用要活跃得多，需要在任何必要的地方导入和使用，以确保记录所有错误和关键的操作活动。但是一旦所有的东西都被正确地实现了，就像我在这里实现的那样，你就应该对你的应用程序正在做什么或者如果遇到错误，错误在哪里产生没有任何疑问。

至此，您对我如何记录所有 NodeJS 应用程序中的大部分活动和错误已经有了很好的了解。在我的职业生涯中，我以前在构建几个 NodeJS 应用程序时开发了协同使用两个独立日志系统的技术。随着我构建每个后续应用程序，我改进了我的日志记录功能，并且在将来，我计划进一步发展我的 NodeJS 应用程序中的日志记录功能。我还没有完全弄明白，但将在我的个人项目中使用的一个部分是跟踪整个应用程序中所有用户操作的系统。它不会记录应用程序的活动，而是维护用户采取的所有操作的运行列表，这些操作会影响他们的数据和系统中的其他数据，就像会计分类帐系统一样。这样的系统不需要传统的日志系统，就像我在这篇开发者故事中详细描述的那样。相反，我希望在 RedisDB 或 MongoDB 这样的数据库中跟踪此类活动，并利用这两个数据库系统中的一个或两个提供的功能。但是我还没有决定，所以我会在即将到来的开发者故事条目中详细说明这些决定和更多内容。随着我个人项目的进一步进展，请保持关注。