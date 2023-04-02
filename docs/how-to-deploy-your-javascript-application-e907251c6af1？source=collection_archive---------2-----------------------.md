# 如何部署您的 JavaScript Web 应用程序

> 原文：<https://javascript.plainenglish.io/how-to-deploy-your-javascript-application-e907251c6af1?source=collection_archive---------2----------------------->

## 在不到 30 分钟的时间内部署您的 JavaScript web 应用程序

![](img/657497b80b4331dfc7243e2584ab7779.png)

Photo by [clark_fransa](https://unsplash.com/@clark_fransa) on [Unsplash](https://unsplash.com/photos/f77Bh3inUpE)

您是否刚刚完成了您的第一个 JavaScript 应用程序，现在想知道“如何让我的应用程序上线？”在本教程中，我们将通过使用 [Git](https://git-scm.com/) 、[节点](https://nodejs.org/en/)、[节点包管理器](https://npmjs.com/) (NPM)和 [Express](https://expressjs.com/) ，以最快最简单的方式将您的 JavaScript 应用程序部署到 [Heroku](https://www.heroku.com/) 上。

Heroku 是一个基于云的平台，为 web 开发人员管理和部署他们的项目提供了一个免费的计划。为了利用他们的服务，我们必须[注册](https://signup.heroku.com/)。

## 0.先决条件

在我们开始之前，请通过在您的终端中运行以下命令来验证是否安装了 Git、Node 和 NPM:

* `$`表示在您的终端中运行命令。

```
$ node --version
```

如果 Node 没有安装，请在此下载它[。](https://nodejs.org/en/download/)

```
$ npm --version
```

NPM 通常与节点一起安装。但是，如果您的系统上没有安装 NPM，请尝试重新安装更新版本的 Node。

```
$ git --version
```

如果没有安装 Git，请在这里下载它[。安装 Git 后，让我们为应用程序初始化一个本地存储库。](https://git-scm.com/downloads)

```
$ git init
```

## 1.安装 Heroku CLI

一旦我们注册了 Heroku 并安装了所有的先决条件，安装 Heroku CLI 并通过您的终端登录。

```
$ heroku login
```

## 2.在应用程序目录中初始化 package.json

如果您的根应用程序目录中已经有一个 *package.json* 文件，您可以跳到第 3 步。

否则，在您的根应用程序目录中，运行下面的命令来初始化一个 *package.json* 文件。这个文件作为一个标识符(就像您的应用程序的 ID 卡)，NPM 需要它来知道为您的应用程序安装哪些依赖项。

```
$ npm init
```

运行该命令后，将提示您一系列问题，这些问题将使用您的答案中的元数据自动填充您的 *package.json* 文件。您现在可以按下`enter`跳过这些问题，因为您稍后可以编辑该文件。

## 3.安装 Express

接下来，我们将使用名为 [Express](https://expressjs.com/) 的 Node.js web 应用程序框架来设置我们的服务器。要安装 Express，我们需要运行以下命令:

```
$ npm install --save express
```

使用 NPM，这将有助于我们将快速模块所需的文件下载并安装到应用程序目录中。`--save`标志还将在我们的 *package.json* 文件中将 Express 列为依赖项。

## 4.**设置快速服务器**

创建一个 *server.js* 文件。当我们在 Heroku 上部署应用程序时，这将作为我们的初始化脚本。

```
$ touch server.js
```

接下来打开 *server.js* 文件并保存以下代码:

这到底是什么意思？让我们再深入一点。

```
const express = require('express');
const app = express();
const path = require('path');
```

为了让我们的 *server.js* 脚本使用 Express，我们需要通过初始化`const express = require('express')`来请求 Express 模块。

下面的代码行`const app = express()`用一个新的 Express 对象初始化 *app* 变量。这将允许我们使用 Express 模块提供的方法。

我们将使用的另一个模块是 Node 提供的 [Path](https://nodejs.org/api/path.html) 模块，我们可以通过`const path = require('path')`访问它。这将为我们提供一组在处理路径目录时可以使用的方法。在我们的例子中，我们将主要使用`path.join(...)`。

```
app.use(express.static(path.join(__dirname)));
```

每当请求(GET、POST、PATCH、DELETE)被发送到触发回调函数的*路径*时，就会调用`app.use(path, callback)`。简单地说，想象一个事件监听器被放置在带有回调函数的指定路径上。然而；如果一个*路径*是**而不是**指定的，则*回调*将在每个请求上触发，而不考虑路径。由于我们目前没有设置路径，我们的回调函数`express.static(...)`将在我们的站点收到每个请求时触发。

`express.static(...)`是一个中间件功能，它允许我们公开一个目录或文件供公众使用。在我们的例子中，我们已经将`path.join(__dirname)`作为中间件函数的参数传入，其中`__dirname`表示应用程序的根目录。这意味着当客户端的浏览器执行请求时，我们的根应用程序目录中的每个目录和文件都可以被客户端访问。

不要惊慌！请记住，每次客户访问我们的网站时，他们的浏览器都会执行一个 GET 请求。反过来，我们的服务器应该发回一个响应以及组成基本 web 应用程序的 *html* 、 *css* 和 *js* 文件。如果我们想只允许某个目录被公开访问，我们可以传入`path.join(__dirname, '*[name of the directory]*')`来代替。随着我们的 web 应用程序在方式上变得越来越复杂，我们可能希望隐藏某些可能包含 API 密钥、秘密等的文件。通过限制哪些文件可以公开访问。

```
app.get('/', function (req, res) {
  res.sendFile(path.join(__dirname, 'index.html'));
});
```

这里是我们设定路线的地方。在`app.get(path, callback)`函数中，当对指定路径发出 GET 请求时，回调被触发。在我们的例子中，我们目前有一条到`'/’`的路由，在这条路由中，我们将*index.html*发送回客户端。只要我们的`app.use(express.static(...))`允许我们的 *css* 和 *js* 文件被公开使用，客户端浏览器也将能够接收这些文件供使用。目前，这足以让我们的网站开始运行。

```
app.listen(process.env.PORT || 3000);
```

这一行告诉我们的 web 应用程序监听哪个端口。当我们部署我们的 web 应用程序时，Heroku 将为服务器设置一个默认的端口值，该值将被标记为`process.env.PORT`。如果没有提供端口，我们的 web 应用程序将默认使用端口 3000。

为了快速测试这一点，我们可以在浏览器中运行`$ node server.js`并转到`[http://localhost:3000](http://localhost:3000)`。一旦一切正常，在你的终端上按`Ctrl + C`关闭本地服务器。

## **4。配置 package.json**

一旦我们配置好了我们的 *server.js* ，我们就必须为 Heroku 设置指令，以便在部署时运行我们的服务器。在您的 *package.json* 文件中的*脚本*下添加`"start": "node server.js"`。

Heroku 将自动寻找 *start* 脚本并在部署时执行它。

## 5.使用 Git 提交所有更改

现在我们已经配置了所有需要的文件，运行下面的命令来提交我们的更改。首先，我们需要进行变革。

```
$ git add .
```

这个命令会将我们所有的文件添加到临时区域。接下来，我们需要提交更改。

```
$ git commit -m '[type some detailed message here]' 
```

提交您的更改后，我们终于可以部署我们的应用程序了！

## 6.将我们的应用程序部署到 Heroku

运行以下命令，并将[]替换为应用程序的名称。

```
$ heroku create [name of your app]
```

这将初始化 Heroku 网站上的一个应用程序，并将 Heroku 设置为您的*中的一个远程存储库。git* 文件。完成后，让我们将本地存储库推送到远程 Heroku 存储库。

```
$ git push heroku master
```

一旦我们的 web 应用程序被成功推送到远程存储库，Heroku 将自动运行我们之前在 *package.json* 文件中定义的`npm start`脚本。接下来，这将运行`node server.js`并启动我们的服务器！如果一切顺利，我们将返回到我们的 web 应用程序的 URL！

祝你好运，编码快乐！

## 资源

*   [使用 Node.js 开始使用 Heroku](https://devcenter.heroku.com/articles/getting-started-with-nodejs?singlepage=true#provision-a-database)
*   [Node.js 路径文档](https://nodejs.org/docs/latest-v13.x/api/path.html)
*   [关于节点包管理器](https://docs.npmjs.com/about-npm/)
*   [关于 Git](https://git-scm.com/about)
*   [使用 ExpressJS 中间件](https://expressjs.com/en/guide/using-middleware.html)
*   [ExpressJS -服务静态文件](https://www.tutorialspoint.com/expressjs/expressjs_static_files.htm)