# 你需要知道的 3 个 Express.js 特性

> 原文：<https://javascript.plainenglish.io/3-express-js-features-you-need-to-know-8f78b0035f33?source=collection_archive---------2----------------------->

## 即使是复杂的 web 应用程序也可以用 Express.js 很好地实现——尤其是有了这些有用的特性。

![](img/a215447f351736f231eab14fed245650.png)

Express.js — the most famous framework for Node.js

任何使用过 Node.js 的人都应该知道 Express.js，几乎所有主要的 Node.js apps 都依赖于这个框架，就像我对自己的项目一样。是的，Express 肯定不是无与伦比的，但它已经成为动态 web 应用程序的标准，使用 Express 实现起来要容易得多。

因此，即使 Express 已经广为人知，这里还是有一些你应该知道的重要特性。

# 1.)创建自己的 Express 中间件

中间件还没告诉你什么？没问题，你会喜欢的。

中间件是在任何路由请求之后执行的功能。在中间件中，我们可以像往常一样访问请求和响应，并使用它们来输出内容、检查 URL 等。
与普通 app.get 的主要区别在于中间件具有 **next()** 功能。
如果这个被执行，任务被转发到 **app.get** 函数，该函数在中间件被执行后实际负责路由。

让我们看一个简单的例子:

```
app.use((req, res, next) => {
  console.log(‘Middleware triggered’, req.url)
  next()
})app.get(‘/’, (req, res) => {
  res.send(‘landing page’)
})app.get(‘/start’, (req, res) => {
  res.send(‘start page’)
})
```

当你运行这个应用程序时，浏览器会做该做的事情。两条独立的路线，每条路线都输出文本。
但是在后端，每次你调用两个路由中的一个，就会通过中间件执行一个 **console.log** ，其中请求的路由通过 **req** 对象输出，中间件可以像访问两个 **app.get** 函数一样访问这个对象。
对 **next()** 的调用使得看起来好像什么都没发生。运行中间件后，app.get 函数再次接管。

在我们的第一个例子中，我们现在有了中间件，它在对服务器的每个请求上执行。我们还可以专门为路由创建中间件——通过简单地定义路由，这对于基本路由“/”不起作用。此外， **req.url** 默认设置为“/”，即使我们要访问的路径是“/start”:

```
app.use(‘/start’, (req, res, next) => {
  console.log(‘You called /start’, req.url)
  next()
})app.get(‘/’, (req, res) => {
  res.send(‘landing page’)
})app.get(‘/start’, (req, res) => {
  res.send(‘start page’)
})
```

通过有意使用 **next()** 我们可以更深入地介入应用程序的流程——不仅仅是通过 **req** 对象:

```
app.use((req, res, next) => {
  console.log(‘Middleware triggered’) if (req.url === ‘/’) {
    res.send(‘Landing page. Greetings from  middleware’)
  } else {
    next()
  }
})app.get(‘/’, (req, res) => {
  res.send(‘landing page’)
})app.get(‘/start’, (req, res) => {
  res.send(‘start page’)
})
```

如果你现在在浏览器中进入**"/**，app.get 的代码永远不会在后端执行，因为正如你所看到的，我们在调用**"/**时专门在中间件中捕捉。
因为我们从不执行 **next()** ，所以我们已经可以在中间件中执行一个响应了。
**注意:**在中间件响应后执行 **next()** 会导致错误。

**结论:**中间件非常强大，可以深入我们的 app，简化流程，拦截错误甚至保护某些路线。

# 2.)使用 app.use 提供静态资产

你有没有尝试过通过你的 Node.js 应用让静态内容比如图像甚至 CSS 文件变得可用？
当然你可以为每个 CSS 文件创建一个单独的路径，但是你也可以使用 Express 来使整个文件夹及其内容静态可用。

下面是文件夹结构:对于静态内容，我创建了文件夹“ **static** ”。
├──app . js
├──package-lock . JSON
├──package . JSON
└──静态
└── styles.css

下面是 **app.js** :

```
const express = require(‘express’)
const app = express()app.use(‘/static’, express.static(‘./static’))app.listen(8080)
```

通过 **app.use** 我们定义了通过哪条路线可以得到某些东西。其次，我们传递了 **express.static** 函数，这是使静态内容可用的实际特性。
因此，我们将从路径 [*下的**静态**文件夹中获取 CSS 文件 http://localhost:8080/static/styles . CSS/*](http://localhost:8080/static/styles.css/)

# 3.)将路线文件分割成单个文件

在我早期的一个项目中，我不熟悉这种技术，这只是导致我最终得到了一个大约 300 行的 app.js 文件。
300 行？怎么做呢？通过在一个文件中使用所有的路由函数。当然，到处都有 app.get()的会导致巨大的混乱。在 Express 中有一个解决方案:我们可以很容易地将我们的单独路线分割成单独的文件。这是它的工作原理。

项目结构:
├──app . js
├──package-lock . JSON
├──package . JSON
└──航线
└── home.js

单个路线的所有文件进入我们的 **/routes** 文件夹，我们导入&并在主文件 **app.js** 中使用它们。

**home.js:**

```
const express = require(‘express’)
const Router = express.Router()Router.get(‘/home’, (req, res) => {
  res.send(‘from the home.js’)
})module.exports = Router
```

**app.js:**

```
const express = require(‘express’)
const app = express()const homeRoute = require(‘./routes/home’)app.use(homeRoute)app.get(‘/’, (req, res) => {
  res.send(‘from our main file’)
})app.listen(8080)
```

为了解决我们的问题，快递提供了**快递。路由器()**。
这允许我们像往常一样使用我们的路线，然后导出它们以使它们可用。
在我们的 **app.js** 中，我们可以导入我们的路线处理文件，并通过 app.use 使用它们。
结果与我们通过 **app.js** 处理所有路线是一样的。

## 迷你功能，感谢评论中的建议:)

为了避免重复，我们可以将几个应该使用相同功能的路由合并到一个数组中。因此，如果我们希望在“/”和“/start”处得到相同的输出，我们可以这样做:

```
app.get([‘/’, ‘/start’], (req, res) => {
  res.send(‘/ or /start’)
})
```

# 关于 Express 的更多信息:

[](https://medium.com/javascript-in-plain-english/express-handlebars-dd2fb85e265d) [## Express.js 和把手——3 个有用的功能

### HTML 变得更加高效和有趣

medium.com](https://medium.com/javascript-in-plain-english/express-handlebars-dd2fb85e265d) 

# 到目前为止就是这样——但是如果你想了解更多关于 Node & Express 的知识，我这里有一些提示给你。

首先感谢您阅读这篇文章。记得给我留点掌声，免费的:)

以下是我的建议，如果你想了解更多关于 Node 和 co。

*   在 [JS in plain English](https://medium.com/javascript-in-plain-english) 上，你可以找到更多与 Node.js、Express & co .相关的精彩文章。
*   查看我的个人资料，我也非常关注 Node.js 内容
*   Node.js 上 Maximilian Schwarzüller & Stephen Grider 的 Udemy 课程很不错。
*   就我个人而言，我喜欢大量阅读，因为不用看屏幕对眼睛来说更舒服。因此，我可以向您强烈推荐亚马逊上的这本
    [节点&快递圣经](https://geni.us/louisbooks)。

## [加入我的邮件，接收你感兴趣的一切](http://eepurl.com/hacY0v)

# **关于我，作者:)**

嗨！再次感谢您的阅读，我叫路易斯，是一名来自德国的 18 岁学生。我热爱 Web 开发，包括后端和前端。我最喜欢的技术是 React，Vue，React Native 和 Node.js.
请务必关注我，了解更多与这些相关的内容，并随时查看我的 IG @ Louis . jsx&@ codingcultureshop
祝您愉快！