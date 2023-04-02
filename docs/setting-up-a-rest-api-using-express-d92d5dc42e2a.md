# 使用 Express 设置 REST API

> 原文：<https://javascript.plainenglish.io/setting-up-a-rest-api-using-express-d92d5dc42e2a?source=collection_archive---------10----------------------->

## 一个可以用作模板的基本 REST API

![](img/fd31683eb1cf9e9c7cbe41e779845230.png)

Photo by [Brian McGowan](https://unsplash.com/@sushioutlaw?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/express?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在本文中，我想向您展示一种快速而可靠的方法来用 [ExpressJS](http://expressjs.com/) 设置一个 [REST-API](https://en.wikipedia.org/wiki/Representational_state_transfer) 。这不包括任何种类的认证或授权。我们将只建立一个单一的路线，并定义一些标准，这将有助于我们扩展这个 API，如果我们选择这样做。

# 先决条件

在我们开始编码之前，我们需要在我们的系统上安装 NodeJS 和 NPM 的版本。只需点击链接[的官方网站](https://nodejs.org/en/)，下载 NodeJS 的 LTS(长期支持)版本。这将自动安装 [NPM](https://www.npmjs.com/) 作为它的软件包管理器。

接下来，我们通过创建一个名为 *express_api_template* 的文件夹来生成我们的项目，然后使用 *npm* 来初始化 NodeJS 项目。

```
$ mkdir express_api_template
$ cd express_api_template/
$ npm init
```

*npm，init* 将带您了解建立新项目的流程。除了*入口点*我一般都用默认设置。我喜欢调用我的主 javascript 文件 *server.js* 而不是默认的 *index.js* 然后填写作者。完成后，我们需要通过将 ExpressJS 添加到我们的*包中来安装它。为此，我们将使用以下命令。*

```
$ npm install express
```

下载完成后，我们应该有一个 *node_modules* 文件夹和两个文件 *package.json* 和 *package-lock.json* 。

# 基础知识

首先，我们需要创建两个新文件夹和两个新文件，以及一些新的依赖项。

```
$ mkdir src
$ mkdir src/config
$ touch src/server.js src/config/config.env
$ npm install colors dotenv
$ npm install nodemon --save-dev
```

普通安装和-save-dev 安装的区别在于普通安装安装生产所需的依赖项。— save-dev 只安装开发所需的依赖项。但是我们实际上在这里安装了什么呢？

*   [***颜色***](https://www.npmjs.com/package/colors)***:***这个包用来使控制台输出丰富多彩。
*   [***dotenv***](https://www.npmjs.com/package/dotenv)***:***这个包从。env 文件到 process.env.{variable_name}
*   [***nodemon***](https://nodemon.io/)***:***这是在开发中用于每次保存更改时重新加载服务器。

安装所有这些不会使应用程序运行。为此，我们还需要做两件事:

1.  配置我们的 *package.json* 来启动 *server.js*
2.  在 *server.js* 中实现基本 Express 服务器

让我们从这样配置 *package.json* 开始:

package.json

我们定义了两个命令用于 **npm** 。第一个是生产用的。它将 NODE_ENV 变量设置为*生产*，然后使用*节点命令*启动服务器。第二个用于开发，将 NODE_ENV 设置为 *development* ，然后使用 *nodemon* 而不是 *node* ，这样我们就可以在开发过程中利用保存时重新加载功能。

**注意:**如果你使用 Windows 作为操作系统，你需要安装 [cross-env](https://www.npmjs.com/package/cross-env) 作为开发依赖来设置 NODE_ENV。

```
$ npm install cross-env --save-dev
```

然后像这样编辑两个脚本:

```
"scripts": {
  "start": "cross-env NODE_ENV=production node src/server.js",
  "dev": "cross-env NODE_ENV=development nodemon src/server.js"
},
```

为了让所有这些工作，我们需要先完成第二步。我们必须创建一个 express 应用程序，然后通过使用我们在 *config.env* 中定义的端口来启动它。将端口添加到文件中，如下所示:

```
PORT=5000
```

现在我们可以开始在 *server.js* 上写一些代码了。

server.js

这非常简单，我们将路径设置为我们的 *config.env* ，然后初始化 express 应用程序。之后，我们开始监听我们刚刚在 *config.env* 中设置的端口。如果我们运行以下命令:

```
$ npm run dev
```

您应该会看到以下输出:

```
[nodemon] 2.0.4
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,json
[nodemon] starting `node src/server.js`
Server up and running in development mode on port 5000
```

现在我们有了一个非常基本的设置。现在让我们在下一节中将它作为 REST-API 的模板。

# 调试和安全性

我们的目标是添加更多的特性，比如将更多的信息记录到控制台，用于调试目的和保护 API。

为了实现上述目标，我们需要添加更多的依赖项。

```
$ npm install helmet cors
$ npm install morgan  --save-dev
```

这三个新的依赖项是做什么的？

*   [***头盔***](https://helmetjs.github.io/)***:***这个包是一个中间件，通过添加多个 HTTP 头来帮助保护你的 API。
*   [***CORS***](https://www.npmjs.com/package/cors)***:***这个中间件帮助我们实现 [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) 。
*   [***摩根***](https://www.npmjs.com/package/morgan)***:***这是一个简单的 HTTP 请求记录器，它将传入的请求输出到节点控制台。

安装完所有这些中间件后，我们需要继续将它们添加到我们的 express 应用程序中。

updated server.js

最值得注意的是对当前 NODE_ENV 的新检查。我们这样做是因为如果我们处于开发模式，我们只需要摩根。如果您以后想要为开发中的数据库添加类似数据播种脚本的东西，那么您可以在那里完成。

在新的检查之后，我们将中间件连接到我们的 express 应用程序。对于 *cors* ，我们配置一个原点。这意味着只允许来自这个来源的请求与我们的 API 通信。例如您构建的前端应用程序。我们只需要将地址添加到我们的 *config.env* 文件中，如下所示:

```
CORS_ORIGIN=http://localhost:8000
```

根据您的 web 应用程序开发设置，端口可能会有所不同。如果是这样的话，那就去改变它。

# 端点和定制中间件

现在我们已经完成了保护 API 的工作，我们将实现两个基本的中间件和一个示例路由。为了保持项目的整洁和可维护性，我们将添加三个文件夹和四个新文件。

```
$ mkdir src/routes src/middleware src/controllers
$ touch src/middleware/notFound.js src/middleware/errorHandler.js src/routes/post.js src/controllers/postsController.js
```

## 中间件

我们首先在 *notFound.js* 中创建第一个中间件函数，它通过抛出 *404 Not Found 错误*来处理没有命中 API 端点的请求。

notFound.js

它只是一个接收请求、响应和 next 的函数。我们创建一个错误，将 HTTP 状态代码设置为 404，并将错误传递给 next。

光靠这个中间件根本帮不了我们。我们需要一些东西来处理传入的错误，比如我们刚刚创建的 *Not Found Error* 。为此，我们实现了下一个中间件功能，称为 *errorHandler* 。

errorHandler.js

如果有人点击了没有端点的路由，我们的 API 将返回一个包含错误消息的 JSON 对象，如果我们正在开发中运行，它也将返回堆栈。

最后一步是将中间件添加到我们的 *server.js* 中。

```
// After the other require statements:
const notFound = require('./middleware/notFound');
const errorHandler = require('./middleware/errorHandler');
// Custom middleware here
app.use(notFound);
app.use(errorHandler);
```

## 路由器

我们离终点越来越近了。只剩下两步了。我们现在重点关注其中一个:增加一条路线。为此，我们需要问自己想要添加什么路线。在本文中，我想添加两个不同的获取路径，一个获取所有文章，另一个根据文章 ID 获取文章。让我们从实现文件 *post.js* 中的路由开始。

post.js

*快速路由器*让我们基于 HTTP 动词定义路由，如 GET、POST 等。我们只需要将我们稍后将实现的控制器方法添加到 HTTP 动词中，然后*路由器*就会施展他的魔法。在 *server.js* 中，我们需要像这样添加路由器:

```
// Between helmet and custom middleware:
// All routes here
app.use('/api/posts', require('./routes/post'));
```

这将抛出一个错误，因为我们还没有实现*控制器函数*。

## 控制器

现在我们处于 REST-API 模板的最后一步。控制器起作用。我们需要创建其中的两个， *getPosts* 和 *getPostById* 。让我们通过在 *postsController.js* 中实现这些方法来开始工作。

postsController.js

在文件的顶部，我们有一些静态数据。之后，我们导出两个函数。第一个函数 getPosts 返回静态数据的完整列表。第二个方法 getPostById，如果 Id 匹配，则从数组中返回一个对象；如果没有 post 匹配请求中提供的 id，则返回一个错误。

我们需要做的最后一件事是通过添加另一个中间件来为我们的应用程序启用 JSON。

```
// Right below helmet:
app.use(express.json());
```

添加这个之后，我们的 *server.js* 应该是这样的:

Final version of server.js

# 结论

你现在可以输入**http://localhost:5000/api/posts**或者**http://localhost:5000/API/posts/2**来访问 API(当它正在运行的时候)。我希望你喜欢这个设置模板 express API 的快速指南。您可以在此基础上添加数据库、身份验证和授权、更多端点等等。让我知道你对它的想法，如果你在这个模板上建立一些东西。整个项目可以在我的 [GitHub](https://github.com/JakobKIT/express_api_template) 上找到。