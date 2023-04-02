# 如何用 Node.js 像老板一样登录

> 原文：<https://javascript.plainenglish.io/log-like-a-boss-nodejs-a10f57a8e3fc?source=collection_archive---------7----------------------->

![](img/b2238ec7310a413d2dc926a6f86b39b0.png)

Credits [https://www.baileyjavinscarter.com/wp-content/uploads/2019/08/logging-accident-1030x429.jpg](https://www.baileyjavinscarter.com/wp-content/uploads/2019/08/logging-accident-1030x429.jpg)

过去几周对我来说很艰难。挑战性和长达数周的工作时间背后的原因是推出已经开发了七年的后端的新版本。日复一日，直到今年九月，代码库随着每一个新特性和错误修复而变得越来越大。我在 2019 年 7 月加入了这个旅程，和我的同事们一起付出了这么多努力。

首先，我们将应用程序分成两个逻辑分区。一个用于客户，一个用于服务提供商。然后，我们决定开始对“服务提供商”进行彻底改革，因为业务逻辑在这方面很重要。然后我们把代码库分成微服务。压力越大，忽略的东西越多。

当他们施加更大的压力时，高层决定忽略这些事情。最终，我们有了一个包含一些功能和一堆微服务的整体后端。可以想象事情变得复杂了。但是我们有一样东西要坚持！这是我们的伐木策略。我们记录了每一步。当一个错误发生时，我们只是打开 CloudWatch 日志并询问，相信我，五分钟或更短时间后，问题就在你的眼前。

# 我们是如何做到的

我们使用 Node.js 作为后端。说到伐木，我们有很多选择。他们都很好，但其中有一个是杰作，那就是温斯顿。我喜欢 Winston，因为它很容易配置，社区也很好。它有很好的文档。你可以用传送器在任何你想去的地方记录。您也可以创建自己的自定义传输。您可以为所有日志级别单独配置 Winston，从何处以及如何进行日志记录。

但是最佳实践是将数据源从代码中分离出来。我们将 Winston 配置为很好地登录到控制台，并使用 fluentd 获取全部输出，然后填充 CloudWatch。

CloudWatch Log Insights 根本不是一项预算友好的服务，但它使您能够交互式地搜索和分析您的日志数据。您可以执行查询来帮助您更高效地响应操作问题。可以保存查询并根据您的需求创建仪表板。

# 我们需要什么？

*   nodejs^ 10xx . x
*   代码编辑器，我选择 VS 代码，
*   一个黑暗，完美的主题，我选择一个黑暗，为什么不呢？
*   流体 d

我将 Fluentd 作为一项要求添加，但我不会介绍如何安装或运行它。AWS 上关于 FluentD 和登录 CloudWatch 的文档很不错。请点击[链接获取流量](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Container-Insights-setup-logs.html)。

# 我们开始吧！

首先，我们需要创建 Node.js 项目并安装软件包。

```
mkdir winston-logger && cd winston-logger 
npm init -y #this will skip prompts
npm install --save winston winston-cloudwatch winston-slack-webhook-transport 
touch logger.js
touch index.js
```

我们准备好记录了！Winston 提供了`error`、`verbose`、`silly`、`info`、`debug`等分级日志。您可以为所有日志级别单独配置 Winston，从何处以及如何进行日志记录。我只是简单地使用信息测试的目的，但我会稍后配置水平。

**logger.js**

```
const winston = require('winston');module.exports = winston;
```

**index.js**

```
const logger = require('./logger');
logger.info('Test Log', {});
```

如果您使用`node index.js`跑步，您将获得:

```
[winston] Attempt to write logs with no transports {"message":"Test Log","level":"info"}
```

因为我们还没有配置它。我添加了一个名为 logger 的变量并创建了一个实例。默认情况下，Winston 可以将数据记录到**控制台、文件、Http** 中。我现在选择**文件**,稍后将研究其他传输选项。您需要提供日志级别和文件名。详细日志记录将记录每个级别。如果您想要记录特定级别，请将级别更改为信息、错误或警告。

**logger.js**

```
const winston = require('winston');const logger = winston.createLogger({
        new winston.transports.File({ level: 'verbose', filename: 'log.out' }),
    ],
});module.exports = logger;
```

让我们运行我们的应用程序。如您所见，在我们的项目目录中有一个名为 **log.out** 的文件。我们做到了。

**注销**

```
{"message": "Test Log","level": "info"}
```

让我们通过提供格式来提高可读性。我还为不同的日志级别添加了传输策略。正如你所看到的，从现在开始，我们将把所有的事情记录到控制台中，但是只把错误记录到文件中。

**logger.js**

```
const winston = require('winston');const logger = winston.createLogger({
    format: winston.format.printf((info) => JSON.stringify(info, null, 2)),
    transports: [
        new winston.transports.File({ level: 'error', filename: 'log.out' }),
        new winston.transports.Console({ level: 'verbose' }),
    ],
});module.exports = logger;
```

让我们运行它，就像我说的对吗？现在，我将添加一些默认值并组合格式。

**logger.js**

```
const winston = require('winston');const logger = winston.createLogger({
    format: winston.format.combine(
        winston.format.timestamp({
            format: 'YYYY-MM-DD HH:mm:ss',
        }),
        winston.format.printf((info) => {
            let logData = { ...info }; if (info.error instanceof Error) {
                logData.error = {
                    message: info.error.message,
                    stack: info.error.stack,
                };
            } return JSON.stringify(logData, null, 2);
        }),
    ),
    transports: [
        new winston.transports.File({ level: 'error', filename: 'log.out' }),
        new winston.transports.Console({ level: 'verbose' }),
    ],
});module.exports = logger;
```

**index.js**

```
const logger = require('./logger');
logger.info('Test Log info', { error: new Error('test') });
```

让我们来分解一下，我使用`winston.format.combine`将这些格式组合在一起。这个函数以格式选项作为参数，你可以使用默认的函数如 **timestamp()、printf()、simple()、colorize()** 。我们可以将可选对象传递给日志，但是传递错误将是空的。因为 Error 对象没有它的可枚举属性，所以它打印一个空对象。

因此，我检查了参数类型，并用错误堆栈更改了它。当您运行代码时，输出如下所示。

```
{
  "error": {
    "message": "test",
    "stack": "Error: test\\n    at Object.<anonymous> (C:\\\\Users\\\\Eren\\\\Desktop\\\\winston-logger\\\\index.js:114:39)\\n    at Module._compile (internal/modules/cjs/loader.js:1137:30)\\n   
 at Object.Module._extensions..js (internal/modules/cjs/loader.js:1157:10)\\n    at Module.load (internal/modules/cjs/loader.js:985:32)\\n    at Function.Module._load (internal/modules/cjs/loader.js:878:14)\\n    at Function.executeUserEntryPoint [as runMain] (internal/modules/run_main.js:71:12)\\n    at internal/main/run_main_module.js:17:47"
  },
  "level": "info",
  "message": "Test Log info",
  "timestamp": "2020-11-10 23:03:29"
}
```

**奖金**

也是 **logger.js** 的最终版本。不要忘记提供 Slack 和 CloudWatch 的密钥。

**logger.js**

```
const winston = require('winston');
const SlackHook = require('winston-slack-webhook-transport');
const WinstonCloudWatch = require('winston-cloudwatch');const logger = winston.createLogger({
    format: winston.format.combine(
        winston.format.timestamp({
            format: 'YYYY-MM-DD HH:mm:ss',
        }),
        winston.format.printf((info) => {
            let logData = { ...info }; if (info.error instanceof Error) {
                logData.error = {
                    message: info.error.message,
                    stack: info.error.stack,
                };
            } return JSON.stringify(logData, null, 2);
        }),
    ),
    transports: [
        new winston.transports.File({ level: 'error', filename: 'log.out' }),
        new winston.transports.Console({ level: 'verbose' }),
    ],
});logger.add(
    new SlackHook({
        webhookUrl: 'SLACK.WEBHOOK_URL',
        channel: '#SLACK.CHANNEL',
        username: 'SLACK.USERNAME',
        iconEmoji: ':warning:',
        formatter: (info) => ({
            text: `_${info.timestamp}_\\n*${info.level.toUpperCase()}:*\\n>${info.message}`,
            attachments: [
                {
                    text: `\\`\\`\\`${JSON.stringify(logData, null, 2)}\\`\\`\\``,
                },
            ],
        }),
        level: 'error',
    }),
);logger.add(
    new WinstonCloudWatch({
        logGroupName: 'CLOUD_WATCH_LOG_GROUP',
        logStreamName: 'CLOUD_WATCH_STREAM_NAME',
        level: 'verbose',
        messageFormatter: (info) => JSON.stringify(logData, null, 2),
    }),
);module.exports = logger;
```

所以我们到了最后，你现在可以像老板一样登录了。有能力记录总是好的，当谈到狩猎错误。希望这个配置对你有帮助。如果你有任何问题或建议，请在评论中告诉我。更多类似这样的内容可以关注我。

**敬请期待！**