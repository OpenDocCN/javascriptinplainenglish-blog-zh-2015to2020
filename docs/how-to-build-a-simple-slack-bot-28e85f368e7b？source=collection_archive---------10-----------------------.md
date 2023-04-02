# 如何构建一个简单的 Slack bot

> 原文：<https://javascript.plainenglish.io/how-to-build-a-simple-slack-bot-28e85f368e7b?source=collection_archive---------10----------------------->

![](img/ef22fa193f334d0a94254432400d8654.png)

Photo by [Lianhao Qu](https://unsplash.com/@lianhao?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/surveillance?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Slack 是一个非常简单的交流工具。每个人都触手可及。你只需敲几下键盘就能吸引任何人的注意力。每当你无聊到不想自己搜索答案的时候，用一个问题分散他们的注意力；-)

在您关闭您所属的大多数频道的通知之前，并不需要您被迫加入许多工作区。然而，有些人有很高的信噪比，你不会介意被告知他们的信息。

幸运的是，这个难题可以通过一个简单的机器人轻松解决。所以让我们来学习如何创建这样一个 Slack bot。

*如果你不在乎一步一步的指导，你也可以直接* [*查看最终代码*](https://github.com/mostlytyped/slack-stalker-bot) *。*

# 构建 Slack bot

我们将在 Node.js 中构建我们的 bot，所以您需要安装`node`和`npm`。如果你想把你的应用程序部署到 Heroku，你还需要一个 Heroku 账户，并安装他们的 CLI。要在本地运行您的应用程序，您还需要[安装并运行一个 RethinkDB 实例](https://rethinkdb.com/)。

要创建应用程序，请在终端中运行以下命令。

```
$ mkdir stalker-bot && cd stalker-bot
$ npm init -y
$ npm install [@slack/events-api](http://twitter.com/slack/events-api) [@slack/web-api](http://twitter.com/slack/web-api) rethinkdb
```

这将初始化 Node.js 应用程序并安装所有必需的依赖项。

# 监听空闲事件

我们将创建一个 Node.js 服务器来监听 Slack 事件。创建一个`index.js`文件，并添加以下服务器代码。

我们首先配置 slack 库，即事件监听器服务器和 web 客户端。然后我们收听`message`事件。直接信息被解释为命令，通道中的信息被监听，以防我们需要通知跟踪者。

# Bot 命令

我们可以直接和机器人聊天来发布命令。跟踪者机器人知道三个命令:

*   `subscribe`对某个渠道的用户
*   `unsubscribe`来自某个渠道的用户
*   `list`所有当前订阅

为了保存所有订阅，我们将使用我最近最喜欢的文档数据库， [RethinkDB](https://rethinkdb.com/) 。它类似于 MongoDB，但另外还内置了反应能力，并且仍然是开源的。我们需要两个表，一个保存所有用户，一个保存他们的订阅。我们稍后将处理管理数据库连接和运行迁移。

创建一个`handler.js`文件，从下面的代码开始。我们首先配置 Slack web 客户机，以便能够响应事件，并在处理实际命令之前添加一些数据库样板文件。

当处理命令时，我们基本上在消息中搜索三个命令中的一个。我们还使用正则表达式来从(取消)订阅命令中提取用户和频道。

## 为用户订阅

为了订阅频道中的用户，我们首先需要从订阅命令中解析所述用户和频道。解析后的用户和通道保存在订阅对象中，该对象可以有侦听器。监听器，即命令发布者，保存在用户表中。

创建订阅时，机器人还需要加入相应的通道，以便能够侦听来自所需用户的消息。

## 取消用户的订阅

要取消订阅通道中的用户，我们还需要首先解析命令，然后恢复订阅命令中完成的操作。我们从订阅中移除侦听器，即命令发布者，或者如果没有侦听器，则删除订阅。

当一个频道不再有订阅时，我们也让机器人离开它。这将减少机器人必须做出反应的信息。

## 列表订阅

列出订阅是一个方便的命令，可以查看我们当前跟踪的用户。

现在我们已经实现了所有的命令，让我们做实际的跟踪。

# 做真正的跟踪

当我们在一个频道中订阅一个用户时，这个机器人就加入了这个频道。它处理每条消息，如果对消息作者感兴趣，就做出相应的反应。如果所述作者有一个收听者，则机器人向收听者发送直接消息。

> 注意:为了让我们的机器人服务于它的目的，我们显然不能禁用直接消息的通知。

# 数据库管理

到目前为止，我们只是方便地获得了一个数据库连接，并假设所需的表已经存在。现在，是时候管理实际的 RethinkDB 连接并处理所需的迁移了。

# 重新思考数据库连接

我们懒惰地管理我们的 RethinkDB 连接，也就是说，我们只在实际需要时才创建(重新)连接。连接参数是从环境变量中解析出来的，或者使用默认值。

在 Heroku 上，RethinkDB Cloud 插件将设置环境变量。对于本地运行的 RethinkDB 实例，缺省值应该有效。

# 移民

没有`users`和`subscriptions`表格，应用程序无法运行。因此，我们需要一个添加这些表的数据库迁移。

这种迁移会检查所需的表是否存在，如果缺少，就会创建它们。它还创建了必要的二级索引，一个用于按频道查找订阅，另一个用于按听众查找订阅。

# 创建 Heroku 应用程序

> 此步骤是可选的。您也可以在本地运行该应用程序，并使用[或](https://ngrok.com/)接收时差事件。

为了将应用程序部署到 Heroku，我们需要创建一个 Heroku 应用程序:

```
$ git init
$ heroku create
Creating app... done, ⬢ fast-inlet-79371
[https://fast-inlet-79371.herokuapp.com/](https://fast-inlet-79371.herokuapp.com/) | [https://git.heroku.com/fast-inlet-79371.git](https://git.heroku.com/fast-inlet-79371.git)
```

创建一个 Heroku 应用程序会给你一个随机名字的 URL。请注意这个 URL，因为它是稍后 Slack 回调 URL 所需要的。

我们还需要一个 RethinkDB 实例来存储和订阅用户之间发送的聊天消息。您可以通过 [RethinkDB Cloud 插件](https://www.rethinkdb.cloud/)进行如下操作:

```
$ heroku addons:create rethinkdb
```

> RethinkDB Cloud 加载项当前处于 alpha 模式。[为您的 Heroku 账户发邮件邀请](https://www.rethinkdb.cloud/)。

# 将应用程序部署到 Heroku

为了将我们的松弛机器人部署到 Heroku，我们需要创建一个`Procfile`。这个文件基本上告诉 Heroku 运行什么进程。

```
// Procfilerelease: node migrate.js
web: node index.js
```

Heroku 认为`release`和`web`进程分别是发布时运行的命令和主 web 应用程序。

使用将应用程序部署到 Heroku

```
$ echo node_modules > .gitignore
$ git add .
$ git commit -m 'A stalker bot'
$ git push heroku master
```

该应用程序还不会工作，因为它缺少两个环境变量，即`SLACK_SIGNING_SECRET`和`SLACK_TOKEN`。我们将在创建实际的 Slack 应用程序时得到它们。

# 创建时差应用程序

要创建一个 Slack 应用程序，请前往[api.slack.com/apps](https://api.slack.com/apps)(如果您尚未登录，请登录，然后返回此网址)。单击“创建应用程序”并填写名称和工作区以关联应用程序。

## 许可

首先，我们需要声明应用程序所需的所有权限。这可以在“验证和权限”选项卡中完成。向下滚动到“作用域”卡，并添加以下“机器人令牌作用域”:

*   频道:历史
*   频道:加入
*   聊天:写作
*   im:历史

`channels:history`和`im:history`权限允许机器人读取其所属频道的信息以及直接发送信息。`channels:join`许可允许机器人加入新的频道。最后，`chat:write`权限允许机器人写直接的信息(例如，给你)。

## 设置环境变量

我们的机器人需要两把备用钥匙。用于验证我们从 Slack 获得的消息事件的签名秘密，以及用于验证我们作为机器人的行为的令牌。签名秘密可以在“基本信息”选项卡的“应用程序凭据”卡中找到。OAuth 令牌显示在“OAuth 和权限”选项卡中。使用将两个密钥都添加到您的 Heroku 应用程序中

```
$ heroku config:set SLACK_SIGNING_SECRET=...
$ heroku config:set SLACK_TOKEN=xoxb-...
```

这将自动重启 Heroku 应用程序，并允许我们接下来添加的事件订阅来验证您正确运行的端点。

## 事件订阅

我们的应用程序只有在我们能够对松弛的工作场所发生的事件做出反应时才有效。转到“事件订阅”选项卡并启用事件。对于请求 URL，放入您从 Heroku 获得的 app URL，并添加`events`路线，例如`https://fast-inlet-79371.herokuapp.com/events`。然后订阅以下 bot 事件:

*   消息.渠道
*   message.im

您将看到这两个事件需要我们在上一步中添加的`channels:history`和`im:history`权限。保存更改以使其生效。

## 安装应用程序

现在我们已经准备好在我们的工作区中安装应用程序了。转到“基本信息”选项卡，然后单击“将应用程序安装到工作区”。这将把你置于应用程序用户的角色，并要求你授予它应用程序所需的权限。

# 测试一下

转到您的工作区，将跟踪机器人添加到您的应用程序中。测试一下，在一个充满噪音的繁忙频道订阅你最喜欢的人。每次被跟踪的人写信给你，你都会收到一条直接消息通知你。

*原载于 2020 年 11 月 16 日*[*https://www . rethinkdb . cloud*](https://www.rethinkdb.cloud/2020/11/16/slack-bot.html)*。*