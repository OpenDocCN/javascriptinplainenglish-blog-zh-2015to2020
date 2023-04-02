# 使用 GraphQL 将 RethinkDB 的实时魔力带到前端

> 原文：<https://javascript.plainenglish.io/bring-rethinkdbs-realtime-magic-to-the-frontend-with-graphql-1995cf20144f?source=collection_archive---------4----------------------->

![](img/ac1926e81c94a57d3ea3218c0d20d917.png)

Photo by Nicolas Picard on Unsplash

在[最近的一篇帖子](https://medium.com/swlh/build-a-chat-app-with-socket-io-and-rethinkdb-df10c5c27bb1)中，我们探讨了 RethinkDB 的内置反应性如何非常适合用 Socket.io 编写聊天应用。在这篇文章中，你将学习如何使用 GraphQL 订阅，以在前端访问 RethinkDB 的反应性。

RethinkDB 是一个实时文档数据库。它易于使用且无模式，就像 MongoDB 一样。此外，您可以订阅查询并在数据更改时得到通知，这使它成为实时应用程序的最佳选择。

> 你也可以试试[运行 app](https://rethink-chat-graphql.herokuapp.com/) ，或者查看[代码库](https://github.com/mostlytyped/rethink-chat-graphql)。

# 应用程序设置

我们将构建一个 Node.js 应用程序，因此您需要安装`node`和`npm`。如果你想把你的应用程序部署到 Heroku 上，你还需要一个 Heroku 账户，并安装他们的 CLI。要在本地运行您的应用程序，您需要[安装并运行一个 RethinkDB 实例](https://rethinkdb.com/)。

我们将使用一个简单的 Node.js 服务器和一个 Vue.js 前端。由于前端需要构建，我们将使用 [Vue CLI](https://cli.vuejs.org/) 创建一个 Vue 应用程序:

```
$ vue create -d rethink-chat
$ cd rethink-chat
```

这将创建一个节点项目，创建一个 Vue.js 框架，并初始化一个 git 存储库。

# 准备一个 Heroku 应用

为了将应用程序部署到 Heroku，我们需要创建一个 Heroku 应用程序:

```
$ heroku create
```

我们还需要一个 RethinkDB 实例来存储和订阅用户之间发送的聊天消息。您可以通过 [RethinkDB Cloud 插件](https://www.rethinkdb.cloud/)来完成此操作，如下所示:

```
$ heroku addons:create rethinkdb
```

> RethinkDB Cloud 附加组件目前处于 alpha 版本。为您的 Heroku 帐户电子邮件申请邀请。

# 构建服务器

我们将在`server`目录中创建我们的服务器。首先，让我们创建目录并安装所需的依赖项:

```
$ mkdir server
$ npm install rethinkdb apollo-server-express graphql morgan lorem-ipsum
```

现在，让我们设置 Node.js 服务器。创建一个`index.js`文件，并添加下面的服务器框架。我们使用 Express.js 服务器来服务前端，使用 Apollo GraphQL 服务器来访问和订阅聊天消息。

这个框架提供了一个来自`dist`文件夹的静态前端。这是我们稍后将创建的已编译的 Vue.js 应用程序所在的位置。此外，我们的服务器需要做三件事:

1.  处理与 RethinkDB 数据库的连接
2.  设置阿波罗服务器
3.  创建一个包含类型定义和解析器的 GraphQL 模式

# 重新思考数据库连接

我们懒惰地管理我们的 RethinkDB 连接，也就是说，我们只在实际需要时才创建(重新)连接。连接参数是从环境变量中解析出来的，或者使用默认值。

在 Heroku 上，RethinkDB Cloud 插件将设置环境变量。对于本地运行的 RethinkDB 实例，缺省值应该有效。

# Apollo GraphQL 服务器安装

如前所述，前端是静态的。然而，我们确实需要访问聊天室中的数据。这将由使用最多的 GraphQL 服务器 Apollo 来处理。

这将使用模式文件中定义的类型定义和解析创建一个 Apollo 服务器(下一节)。我们还连接到 RethinkDB，并将连接传递给我们的 GraphQL 上下文，以便它可以用于任何传入的请求。

# 创建一个 GraphQL 模式

服务器的主要逻辑在于定义 GraphQL 类型并实现它们的解析器。我们需要能够执行三种不同的操作，即

*   在房间中查询聊天消息
*   向房间发送聊天信息
*   在房间中订阅新的聊天消息

首先，我们创建 GraphQL 类型。这由一个`Chat`消息类型和三个提到的动作组成，即`chats`查询、`sendChat`变异和`chatAdded`订阅。

其次，我们需要解析这些动作，即实现它们调用的代码。查询和变异相当简单，实现为一个简单的 RethinkDB 查询。然而，订阅需要异步迭代器。这基本上是一个将 RethinkDB 魔术变成 GraphQL 订阅魔术的咒语。用更通俗的术语来说，异步迭代器包装了 RethinkDB 变更提要，因此我们可以通过 GraphQL 订阅它。

设置好服务器后，让我们转到前端。

# 创建前端

我们已经创建了将用于前端的 Vue.js 应用程序框架。然而，由于我们的服务器实现了标准的 GraphQL 后端，您也可以使用 React 或任何其他支持 GraphQL 的前端框架。

我们的前端将使用两个视图，一个用于主页，一个用于聊天室，以及在两者之间导航的路由器。为此，让我们向 Vue 框架添加一个路由器，并安装所有必需的依赖项。将路由器添加到 Vue 应用程序将警告您未提交的更改(无论如何继续)，并询问您是否想要历史模式(否)。

```
$ vue add router
$ npm install apollo-client apollo-link-http apollo-link-ws apollo-cache-inmemory vue-apollo
$ npm install sass sass-loader --save-dev
```

我们的 Vue 应用程序位于`src`文件夹中，结构如下:入口点在`main.js`中，从`graphql.js`获取 GraphQL 客户端配置。我们的主文件还挂载了显示由`router/index.js`中的路由器选择的视图的`App.vue`。我们的应用程序包含两个视图，`views/Home.vue`和`views/ChatRoom.vue`。

```
src
├── main.js
├── graphql.js
├── App.vue
├── router
│   └── index.js
└── views
    ├── Home.vue
    └── ChatRoom.vue
```

# 主应用程序和路由器

第一步，让我们修改在骨架 Vue 应用中初始化的主应用、主视图和路由器文件。在`main.js`中，我们导入了我们将进一步定义的 Apollo GraphQL 客户端，并将其添加到我们的 Vue 应用程序中。此外，我们还将为用户创建一个随机的聊天用户名。

我们的`App.vue`甚至比骨架更简单，它只是显示了路由器视图，并有一些样式。

在我们的`router/index.js`中，我们基本上用“房间”路线代替了“关于”路线。

在主视图中，我们删除了`HelloWorld`组件，并添加了一个允许我们加入房间的表单。

既然我们已经用我们需要的部分填充了框架，让我们来处理前端、GraphQL 客户端和聊天室视图的真正内容。

# GraphQL 客户端

当我们的前端加载时，我们需要启动 GraphQL 客户端。在我们的例子中，我们使用最常用的 GraphQL 客户端 Apollo，它与`vue-apollo`包有很好的 Vue.js 集成。

因为我们将使用 GraphQL 订阅，所以我们的 Apollo 设置比通常要复杂一些。这是因为普通的 GraphQL 应该通过 HTTP 执行，但是订阅更新将通过 WebSocket 推送。

# 聊天室视图

前端的最后一块将是`ChatRoom`视图。在这里，我们实际上开始使用我们刚刚初始化的 GraphQL 客户端。这个视图基本上显示了一个列表，其中包含了`chats`变量中的所有条目，并提供了一个向后端发送聊天消息的表单。

`sendMessage`方法被绑定到`sendChat` GraphQL 突变。至于`chats`变量，绑定就有点复杂了。我们将它绑定到 GraphQL `chats`查询，此外，我们使用`chatAdded`订阅来保持变量最新。

现在我们有了一个工作的服务器和前端。我们需要做的最后一件事是，当我们运行应用程序时，确保 RethinkDB 数据库中确实存在`chats`表。

# 数据库迁移

没有`chats`表格，应用程序无法运行。因此，我们需要一个添加表的数据库迁移。

这个迁移检查`chats`表是否存在，如果它不存在，就创建它。

# 一个简单的聊天机器人

正如我们所看到的，RethinkDBs 的一个重要特性是其固有的反应能力，允许我们订阅查询。当创建一个简单的聊天机器人时，这个特性也很方便。机器人只需要订阅`chats`表中的变化，并在适当的时候做出反应。

每当用`@lorem`提示时，我们的 Lorem bot 将回复随机的 Lorem Ipsum 部分。机器人订阅了`chats`表，并扫描消息的开头。如果是以`@lorem`开头，会回复同室留言。

# 将应用程序部署到 Heroku

为了将我们的工作应用程序和 bot 部署到 Heroku，我们需要创建一个`Procfile`。这个文件基本上告诉 Heroku 运行什么进程。

```
// Procfilerelease: node server/migrate.js
web: node server/index.js
lorem-bot: node server/lorem-bot.js
```

Heroku 将`release`和`web`进程分别识别为发布时运行的命令和主 web 应用程序。`lorem-bot`进程只是一个工作进程，可以有任何名称。

使用以下命令将应用程序部署到 Heroku

```
$ git add .
$ git commit -m 'Working rethink-chat app'
$ git push heroku master
```

> 您需要在 Heroku 应用程序中手动启用`lorem-bot`流程。您可以在“资源”选项卡上执行此操作。

# 结论

在不到 15 分钟的时间里，我们设法用一个简单的机器人创建并部署了一个聊天应用程序。这显示了 RethinkDB 的强大和易用性。订阅查询的能力使得构建一个反应式应用程序变得很容易，并且可以很容易地与 GraphQL 集成。此外，Heroku 使部署变得轻而易举，有了 RethinkDB Cloud 插件，您将永远不必亲自管理数据库服务器的繁琐工作。

*原载于 2020 年 9 月 3 日*[*https://www . rethinkdb . cloud*](https://www.rethinkdb.cloud/2020/09/03/rethinkdb-chat-graphql.html)*。*

## 简单英语的 JavaScript

你知道我们有三份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)找到一切的链接！