# 用 Node.js 和 MongoDB 创建 URL shortener

> 原文：<https://javascript.plainenglish.io/creating-a-url-shortener-with-node-js-and-mongodb-db4bc6488445?source=collection_archive---------2----------------------->

## 下面是如何使用 Node，MongoDB 和 Express 创建一个定制的 URL shortener。

# 介绍

URL shortener 是一个简单的网络工具，它将一个很长的 URL 简化为更小的、可管理的链接，使在线流量管理更加容易。最常用的网址缩写是:

*   bitly.com
*   tinyurl.com

在这个故事中，我们将在 Node.js 和 Express 的帮助下，使用 MongoDB 作为我们的数据库，创建我们自己的 URL shortener。

前往[https://shortify-link.herokuapp.com/](https://shortify-link.herokuapp.com/)查看现场版本。

![](img/b5be589cf313a7fa1f9d717258ae3d35.png)

# 要求

*   诺杰斯和 NPM
*   MongoDB
*   Postman 或类似的 API 测试工具
*   代码编辑器，如 Visual Studio 代码或 Atom

# NodeJS 设置

创建您的项目目录并初始化 npm 包。我们称之为简化。

```
$ npm init
```

接下来，我们将安装这个项目所需的所有包。

```
$ npm install express mongoose fs
```

另外，如果您还没有安装 nodemon，请在您的系统上安装它。

```
$ npm install -g nodemon
```

我们将使用 Express.js 和 Mongoose 连接到我们的 MongoDB 数据库。Fs 将用于呈现我们的 HTML 页面。

您的 index.js 文件应该如下所示。

在 package.json 文件中包含一个启动脚本。

```
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "nodemon index.js"
},
```

# 前端设置

我们将创建两个网页，一个用于缩短网址，另一个用于分析链接。

在项目目录中创建视图文件夹。在这里，我们将在我们的视图文件夹中创建我们的网页，home.html 和 count.html。

# 创建后端 API

我们已经创建了我们的前端，现在我们需要我们的后端来服务它和相关的 API。

让我们首先创建 GET 请求来服务我们的 web 页面。

```
// GET request to serve home.html
app.get('/', (req, res) => {
  res.writeHead(200, {
    'Content-Type': 'text/html'
  });
  fs.readFile('./views/home.html', null, function (error, data) {
    if (error) {
      res.writeHead(404);
      res.write('Route not found!');
    } else {
      res.write(data);
    }
    res.end();
  });
})// GET request to redirect the shortened URL to the original URL
app.get('/:route', async (req, res) => {
  const route = req.params.route;
  const instance = await Url.findOne({id: route});
  if(instance) {
    instance.visitors = instance.visitors + 1;
    await instance.save();
    res.redirect(`//${instance.url}`)
  } else {
    res.send("404")
  }
})// GET request to serve count.html and show analytics
app.get('/analytics/:route', async (req, res) => {
  res.writeHead(200, {
    'Content-Type': 'text/html'
  });
  fs.readFile('./views/count.html', null, function (error, data) {
    if (error) {
      res.writeHead(404);
      res.write('Route not found!');
    } else {
      res.write(data);
    }
    res.end();
  });
})
```

现在启动本地服务器并测试主屏幕。

```
$ npm start
```

我们的网址缩写仍然没有工作。为了使这个功能，我们需要连接到我们的后端。
现在，我们将创建 API 来连接我们的数据库和应用程序。
首先，我们将在 models 文件夹中创建我们的 URL 模式。

在 index.js 文件中导入模型。

```
require('./models/Url');
const Url = mongoose.model('Url');
```

现在，我们将为创建 API 来为网页提供内容。

首先，我们将创建一个 API 来缩短 URL。

```
// POST request for shortening the URL
app.post('/', async (req, res) => {
  const url = req.body.url;
  const instance = new Url({
    url: url,
    visitors: 0
  });
  short = JSON.stringify(instance._id)
  const id = short.slice(short.length-7, short.length-1)
  instance.id = id;
  await instance.save()
  res.send({
    message: `${id} was created`,
    url: `${id}`,
  });
})
```

当我们单击 shorten 按钮并返回缩短的 URL 时，就会调用这个 API。

接下来，我们将创建分析 API 来服务大量的访问者。

```
// POST request to send the number of visitors to count.html
app.post('/analytics', async (req, res) => {
  const route = req.body.route;
  const instance = await Url.findOne({id: route});
  res.send({
    visitors: instance.visitors,
    message: "Number of visitors sent!"
  })
})
```

现在，您的 index.js 文件应该看起来像这样。

这样，我们的网址缩写就准备好了。我们现在要做的就是主持它。

# 功能(可选)

如果你想了解这个网址缩写的功能，那么你可以阅读这一部分，否则你可以直接跳到主机。

我们的 URL 缩短器将给定的 URL 映射到更小的 API URLs。我们将把原始的 URL 存储在我们的 MongoDB 数据库中。

我们在 index.js 中创建的 GET 请求“/:route”将我们重定向到原始 URL。通过这种方式，我们将长 URL 映射到更小的 URL。

GET 请求“/analytics/:route”返回显示该 URL“/:route”的访问者数量的页面。

# 在 Heroku 上举办

为了这个项目，我们将在 Heroku 上托管我们的 URL shortener。

如果您还没有帐户，请在 Heroku 上创建一个帐户。

在 Heroku 上创建一个项目。

在你的终端登录你的 Heroku 账户。

```
$ heroku login
```

在项目目录中初始化 git 存储库。

```
$ git init
```

添加远程 git 仓库。

```
$ heroku git:remote -a project-name
```

创建一个 Procfile 并将其写入文件中。Heroku 部署您的应用程序需要 Procfile。

添加并提交所有文件。

```
$ git add .
$ git commit -m "heroku deploy"
```

将您的更改推送到 Heroku 远程 git 存储库。

```
$ git push heroku master
```

等待构建完成，然后检查你的网址缩写！

# 包扎

您的自定义网址缩写程序现已准备就绪。你可以通过打开并使用你的 Heroku 应用来测试它。这是一个非常短的项目，可以在许多地方进行改进。没有适当的 URL 验证来检查输入的 URL 是否有效。通过引入一个简单的正则表达式来验证 URL，可以很容易地解决这个问题。可以对 Web 应用程序 UI 进行改进。

祝贺您，您已经成功创建了自己的网址缩写程序。✌

*在*[*【https://github.com/maw1a/Shortify/tree/master】*](https://github.com/maw1a/Shortify/tree/master)签出该项目的 github 库

*如果你想使用我的这个工具，请在*[*https://shortify-link.herokuapp.com/*](https://shortify-link.herokuapp.com/)查看现场版

*如果对故事有任何问题或建议。请留言，我会处理的。*