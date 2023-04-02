# 开发者故事:错误处理

> 原文：<https://javascript.plainenglish.io/developer-story-error-handling-fcaa357578cc?source=collection_archive---------7----------------------->

![](img/ff487b41fd5b1f2a5b66d49fbabb9474.png)

在以前的开发人员故事中，我讨论了在我开发的每个服务器应用程序中包含健壮的日志记录系统的重要性，无论是在我的专业工作中还是在我的个人项目中。

正如我在那篇文章中提到的，日志记录系统不仅允许应用程序的开发人员知道该应用程序在任何给定时间的当前状态，而且当遇到错误时，它还将提供一系列收据，这将允许开发人员追溯到错误的源头。

没有日志记录系统，就不能以有组织的方式跟踪任何地方的错误，并且没有良好实现的错误处理机制，当实际错误发生时，日志记录系统就没有什么可跟踪的。在这个开发人员故事条目中，我将讨论我个人的错误处理策略，我将它与一个健壮的日志记录系统相结合，以确保我在遇到错误时不会有追踪错误来源的麻烦。

在介绍我开发的 NodeJS 服务器应用程序的不同元素时，我偶尔会提到我的应用程序都包含一个实用模块，该模块由许多不同的有用子模块组成，用于各种用途。其中一个实用程序子模块是我的应用程序中的大部分错误处理逻辑所在的地方。该*错误*子模块的内容如下所示:

```
***util/error.js***class ServiceError extends Error {
  constructor(message, originFunctionName, httpResponseCode = 400, addSuffix = true) {
    super(message)
    Error.captureStackTrace(this, this.constructor) this.name = this.constructor.name
    this.origin = originFunctionName
    this.code = httpResponseCode const functionName = (originFunctionName.slice(-2) === '()' || !addSuffix) ? originFunctionName : `${originFunctionName}()`
    this.display = `${functionName}: ${message}`
  }
}class WrapperError extends ServiceError {
  constructor(error, originFunctionName, addSuffix) {
    super(error.message, originFunctionName, error.code, addSuffix)
    Error.captureStackTrace(this, this.constructor) this.name = this.constructor.name
    this.data = { error }
  }
}const errors = {
  // Database-related errors
  DB_CONNECTION_FAILED: { code: 400, message: 'Client database connection failed' },
  DB_CONNECTION_NOT_ESTABLISHED: { code: 400, message: 'Database connection has not been established' },
  DB_SESSION_ENDED: { code: 400, message: 'Database session has already ended' },
  DB_SESSION_NOT_SPECIFIED: { code: 400, message: 'Database session has not been specified' },
}exports = module.exports = {
  createError: (e, origin, addSuffix) => {
    let error
    if (typeof e === 'string') {
      error = errors[e] ? new ServiceError(errors[e].message, origin, errors[e].code, addSuffix) : new ServiceError(e, origin, 400, addSuffix)
    } else {
      error = e.origin === origin ? e : new WrapperError(e, origin, addSuffix)
    }
    return error
  },
}
```

虽然这个*错误*子模块相当小，但是它做了很多工作。首先，在实现任何其他逻辑之前，创建两个新的错误类。通过扩展内置的 NodeJS `Error`类来创建`ServiceError`类，而通过扩展这个`ServiceError`类来创建`WrapperError`类。`ServiceError`类的目的是创建一个更健壮的错误对象，该对象包含所有关于错误原因、错误最初发生的位置以及该错误会导致什么结果的相关信息。`WrapperError`类，顾名思义，只是作为另一个先前创建的错误类对象的包装器。一般来说，这些由`WrapperError`类包装的错误类对象不是来自我的代码，而是来自应用程序使用的第三方包。

阅读完 NodeJS 错误[文档](https://nodejs.org/api/errors.html)后，您会知道标准的 NodeJS `Error`类只提供了`code`、`message`和`stack`属性来指定特定错误情况的细节。`ServiceError`类通过添加`display`、`name`和`origin`属性并重新利用`code`属性来提供关于特定错误的更清晰的画面，并使记录人类友好的错误消息更容易。`name`属性包含实际错误类型的名称(`ServiceError`)，而`origin`属性包含最初发生错误的函数的完全限定的唯一名称。我在所有的服务器应用程序中使用严格的函数命名约定来唯一地识别任何可能发生错误的地方。`display`属性包含一个友好的字符串，可以被应用程序日志记录系统使用。最后，`code`属性被重新调整用途，以便它包含 HTTP 响应代码，该代码应该作为当前错误的结果返回给客户机。`WrapperError`类通过添加`data`属性建立在`ServiceError`类的基础上，这是存储被包装的错误对象的地方。

在定义了这两个自定义错误类之后，在*错误*子模块中定义的下一件事是一组错误对象，包含一个 HTTP 响应代码和一条错误消息。这些错误对象用于在应用程序中发生错误的任何时候构建新的`ServiceError`对象，并且该错误的原因被确定为对应于这些错误对象中的一个。如果指定的错误字符串匹配这些错误对象之一的名称(`DB_CONNECTION_FAILED`、`DB_SESSION_ENDED`等)，则确定错误对应于这些错误对象之一。).并非应用程序中的所有错误都对应于这些预定义的错误对象之一，这就是`WrapperError`类存在的原因。但是所有预期的应用程序错误都应该在*错误*子模块的这一部分中定义。在这个代码示例中，我只包含了几个与数据库相关的错误，但是这个列表应该更长，并且涵盖了您预期的应用程序在运行过程中会遇到的所有错误。尽管您可以尽可能疯狂地定义这个错误列表，但我个人会尽量减少预定义错误的数量，并将导致特定错误的情况组织到相关的组中。这不仅会导致更少的代码，而且还会防止重复错误的产生。我在以前的工作中见过类似的错误处理方案，有时会产生重叠的错误，而这些错误本应被归为一个错误。在我看来，这种情况最常见的原因是因为以前创建了太多的错误，以至于开发人员在查看一长串预定义错误时，甚至没有意识到那些适用的错误的存在。因此，我总是试图考虑错误是否需要新的 HTTP 响应代码或消息，或者两者都需要。如果是的话，我会为它创建一个新的错误。否则，我尽量在多种情况下使用现有的错误。

在*错误*子模块中，逻辑的最后部分是决定是创建一个全新的`ServiceError`对象还是用`WrapperError`对象包装一个现有的错误。每当使用这个*错误*子模块创建错误时，必须指定至少两件事，错误本身和错误被创建的当前位置，否则称为原点。如果错误是字符串，则在预定义的错误列表中检查是否匹配。如果找到一个匹配，那么在那个错误对象中定义的细节被用来创建一个新的`ServiceError`。否则，将创建一个默认的错误对象，并将错误字符串作为错误消息。相反，如果错误不是字符串，则假定它是一个对象，并根据指定的原点检查该对象中指定的原点。如果错误的来源与指定的来源匹配，则错误是在我自己的应用程序代码中创建的，并且错误按原样使用。否则，错误被认为是来自应用程序使用的第三方包，并用一个`WrapperError`对象包装。

对错误构造逻辑的工作原理有了更好的理解后，了解如何在整个应用程序中实现所有的错误处理逻辑以利用这一功能就变得非常重要。在任何可能引发错误的地方，都应该使用下面的函数结构:

```
**controllers/authentication.js**const logger = require('../log').app
const util = require('../util')module.exports.login = async ({ body: { email, password } }) => {
  const FUNCTION_NAME = 'controllers.authentication.login'
  try {
    ...
  } catch(e) {
    const error = util.error.createError(e, FUNCTION_NAME)
    logger.error(error.display)
    throw error
  }
}
```

在这个代码示例中有很多事情需要注意，我在这篇开发者故事的开头提到过。第一个是函数名的定义，用于在 *try-catch* 语句的 *catch* 部分创建一个错误。我通常根据函数在顶级模块中的位置来命名所有函数，从根级别一直到文件夹结构树，直到函数本身的名称。我总是非常严格地确保给每个函数一个唯一的名称，以便包含在错误中，这样就不会怀疑错误是否来自这个函数。在函数名定义之后，必须用一个 *try-catch* 语句包装函数中的所有代码，以确保没有错误意外地逸出该函数。在 *try-catch* 语句的 *try* 部分中，所有必要的函数逻辑都将存在。自然，会对各种条件进行逻辑检查，以确定登录尝试是否成功。在不成功的情况下，应该使用如下代码行来创建描述问题的错误:

```
throw util.error.createError('INCORRECT_PASSWORD', FUNCTION_NAME)
```

在这种情况下，在*错误*子模块中会有一个名为`INCORRECT_PASSWORD`的预定义错误，它提供了一个 HTTP 响应代码和一条消息，用于创建新的`ServiceError`对象。该错误将在 *try* 部分中被捕获，并被传递到 *try-catch* 语句的 *catch* 部分，其中任何遇到的错误都被用来构造新的`ServiceError`或`WrapperError`对象。从该代码示例中的逻辑可以更容易地理解，使用实用程序 *error* 子模块从遇到的错误构造新的错误对象有两个目的。第一个目的是使新构造的错误对象可以用于使用日志记录系统记录错误。因为最初遇到的错误没有`display`属性，所以在被日志记录系统使用之前，需要对其进行重构以具有该属性。第二个目的是让新构造的错误对象可以被抛出并冒泡到客户端接口层。一旦此错误出现在该层，它就有了额外的用途，如下面的代码示例所示:

```
**routes/authentication.js**const logger = require('../log').app
const util = require('../util')authentication.post('/login', async (req, res) => {
  const FUNCTION_NAME = 'POST /authentication/login'
  try {
    ...
  } catch(e) {
    const error = util.err.createError(e, FUNCTION_NAME)
    logger.error(error.display)
    return res.sendStatus(error.code)
  }
})
```

此代码示例中的代码与控制器代码示例非常相似。唯一的区别在于如何在 *try-catch* 语句的 *catch* 部分的末尾使用冒泡出现的错误。在控制器代码示例中，它只是被抛出以在调用堆栈中冒泡，而在这个代码示例中,`code`属性用于在 HTTP 响应中发送适当的错误状态代码。这就是为什么源自应用程序逻辑深处的错误会一直上升到客户端接口层，从而为客户端提供有意义的结果。我之前在开发者故事条目中提到过，但是我将在这里再次重申。这是绝对必要的，就像在整个应用程序中应该如何一致地使用日志记录系统一样，这里介绍的错误处理逻辑应该用在可能导致抛出错误的每个函数中，在大多数情况下，这通常是几乎所有的函数。此外，假设在*错误*子模块中创建的两个错误类对象上有额外的属性，则可以根据需要通过日志记录系统记录更多的细节。我在这里只提供了如何使用日志系统和错误处理系统的简化版本，以传达总体思想，但是您可以在自己的实现中随意使用这个错误处理解决方案提供的或多或少的错误信息。

至此，您应该很清楚我是如何在所有 NodeJS 应用程序中处理错误的，以及错误处理功能是如何与一个健壮的日志记录系统相结合来创建一个包含所有错误的系统，并使跟踪每个错误的来源变得容易。我个人处理错误的策略并不总是如此全面或谨慎，但是一旦它演变成本文中介绍的形式，我发现它确实使开发变得更加容易，因为我在我的服务器应用程序中实现了新的特性。我不再被迫到处抛出`console.log()`语句，试图追踪到底哪里出现了错误。只需打开最新的日志文件并滚动到底部，就可以找到错误的确切位置，节省了我的时间、精力和很多麻烦。我希望我的错误处理策略也能帮助您节省宝贵的开发时间和精力。请继续关注我的下一次发展讨论，因为我将继续在我的个人项目上取得进一步的进展。