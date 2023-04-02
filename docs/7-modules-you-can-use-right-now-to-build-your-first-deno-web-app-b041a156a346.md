# 7 个模块，你现在就可以用来构建你的第一个 Deno 网络应用

> 原文：<https://javascript.plainenglish.io/7-modules-you-can-use-right-now-to-build-your-first-deno-web-app-b041a156a346?source=collection_archive---------2----------------------->

![](img/007a0397c3a92c69a63b4d2d882ac910.png)

## Deno 1.0.0 终于来了。这里有一些资源可以帮助你创建你的第一个 Deno 网络应用。

# 什么是德诺？

Deno 是由 [**瑞恩达尔**](https://en.wikipedia.org/wiki/Ryan_Dahl) 创造的，那也是你可能听说过的某个东西的创造者……是的， [**Node.js**](https://nodejs.org/en/) 。

2 年前，Ryan 在 JSConf 上做了一个名为“Node.js 的 10 件让我后悔的事情”的演讲，他宣布他正在开发 Deno，这将是一个更安全的 Node 版本，没有会使你的项目膨胀的 node_modules 文件夹。

来自 [**deno.land**](http://deno.land) 网站(deno mascotte 是恐龙以来最好的域名)，Deno 是一个 *JavaScript/TypeScript 运行时，具有安全的默认值和出色的开发人员体验。它是基于 V8、Rust 和 Tokio* 打造的。

前提是伟大的，但如果你像我一样，你会想得到你的手弄脏这一点，以更好地理解它。

这里有 **7 个 Deno 模块**可以帮助你创建一个 web 应用并了解 Deno:

# 1.迪纳特拉

如果你是学 Ruby 语言的，你可能听说过 [Sinatra](http://sinatrarb.com/) ，这是一个小小的网络框架，可以在 5 分钟内以最小的努力将一个网络服务器上线。

Dinatra 是 Sinatra，但对于 Deno 来说是一个轻量级的 web 应用程序框架，你现在可以使用它在 Deno 中创建你的第一个 web 应用程序。

这里有一个例子:

您可以创建 CRUD 方法，访问查询和主体参数，并立即返回 JSON。

[](https://github.com/syumai/dinatra) [## 苏迈/迪纳特拉

### Sinatra 喜欢 deno 的轻量级 web 应用程序框架。-苏迈/迪纳拉

github.com](https://github.com/syumai/dinatra) 

# 2.德诺·波斯格雷斯

使用 Deno-Postgres，您可以连接到您的 Postgres 数据库并进行 SQL 查询。所有方法返回*承诺*，因此您将能够使用*等待*所有结果。

[](https://github.com/buildondata/deno-postgres) [## buildondata/deno-postgres

### Deno 的 PostgreSQL 驱动程序。它仍在进行中，但你可以试驾一下！deno-postgres 正在…

github.com](https://github.com/buildondata/deno-postgres) 

# **3。迪诺尼斯**

访问和阅读 Postgres 数据库很有趣，但你知道什么不有趣吗？**改变数据库结构。你没有版本控制，一个错误就能毁掉你所有的存储数据。孩子们，一定要先备份数据！**

Deno-nessie 是 Deno 的数据库迁移工具，它将帮助您创建迁移文件并更改您的表。通过这种方式，您可以将数据库更改的所有历史添加到您的版本控制中，并且您的同事将能够轻而易举地更改他们的本地数据库。

[](https://github.com/halvardssm/deno-nessie) [## halvardssm/deno-nessie

### 受 Laravel 启发的 deno 数据库迁移工具。支持 PostgreSQL 和 MySQL，很快:SQLite。参见文档…

github.com](https://github.com/halvardssm/deno-nessie) 

# 4.德诺蒙戈

你是 NoSQL 的粉丝吗？你喜欢把所有的数据放在没有固定结构的数据库里吗？

Deno_mongo 是你配合 Deno 使用 MongoDB 所需要的。您将在不到一分钟的时间内开始向您的 repos 添加非结构化数据💪

[](https://github.com/manyuanrong/deno_mongo) [## 满园荣/德诺 _ 蒙戈

### Deno 的 MongoDB 驱动程序。在 GitHub 上创建一个帐户，为 manyuanrong/deno_mongo 的发展做出贡献。

github.com](https://github.com/manyuanrong/deno_mongo) 

# 5.Deno SMTP

很多时候你需要在你的网络应用中发送电子邮件。以下是您需要发送的一些电子邮件示例:

*   新用户的确认电子邮件；
*   忘记密码的电子邮件；
*   订阅收据；
*   *当用户 7 天以上没有访问你的应用程序时，你会想念我们的*邮件吗(这是最糟糕的。求求你，不要这样)

**Deno-smtp** 是你发送那些将有助于你日常活动的重要电子邮件所需要的。还可以在邮件内容中添加 html！

[](https://github.com/manyuanrong/deno-smtp) [## 满园荣/deno-smtp

### SMTP 为 deno 实现。在 GitHub 上创建一个帐户，为 manyuanrong/deno-smtp 开发做出贡献。

github.com](https://github.com/manyuanrong/deno-smtp) 

# 6.Deno Dotenv

你将开始在你的本地 PC 上编写你闪亮的新 Deno 应用程序，上面的许多模块使用某种证书(SMTP 证书，MongoDB urls 等等)。但是您不能将这些凭据直接放在代码中，因为:

*   在生产服务器上，您将使用不同的凭证(或者至少我希望如此)
*   您不希望在某个 git 存储库上公开它们。

Deno-dotenv 允许处理 *dotenv* 文件。只需将您的凭证和 ENV 变量放入*中。env* 文件，并用这个模块访问它们:

这会打印出来

```
> deno dotenv.ts
{ GREETING: "hello world" }
```

 [## 彼得万佐恩/德诺多滕

### deno 的 Dotenv 处理。在项目的根目录中设置一个. env 文件。问候=hello world 然后导入…

github.com](https://github.com/pietvanzoen/deno-dotenv) 

# 7.德农

如果您使用 node，那么当您保存正在处理的文件时，您肯定会使用 [Nodemon](https://nodemon.io/) 来自动重新加载您的本地服务器。

德诺正是如此，但对德诺来说。

你需要先用*安装然后再安装*

```
deno install --unstable --allow-read --allow-run -f 
https://deno.land/x/denon/denon.ts
```

然后，您可以使用 denon 启动本地应用程序，后跟您的应用程序文件路径。下次你对你的应用程序进行更改时，Denon 会自动重新加载你的 Deno 服务器！

[](https://github.com/eliassjogreen/denon) [## eliassjogreen/denon

### Denon 的目标是成为 nodemon 的 deno 替代品，提供打包的功能和易于使用的体验。Denon 提供…

github.com](https://github.com/eliassjogreen/denon) 

# 但是…很多 npm 库已经和 Deno 兼容了！

是的，很多 npm 库已经和 Deno 兼容了！这是最好的特性之一:编写一次代码，然后在 Node 和 Deno 上运行。

例如，您现在就可以在 Deno 中导入和使用这些库:

[https://www.i18next.com/overview/getting-started](https://www.i18next.com/overview/getting-started)

https://github.com/adrai/enum

在接下来的几个月里，许多其他产品将与 Deno 完全兼容。

# Deno 未来会取代 Node.js 吗？

现在给出明确回应还为时过早。Deno 已经到了 1.0.0 但是还远没有完成。

在未来几年内，Node 仍然是后端 JavaScript 的首选，但是有一个更安全的替代方案，并以正确的方式解决 JavaScript 最令人尴尬的部分之一(是的，我说的是那个占用大量硬盘的 node_modules)，那就太好了。

这些模块肯定将有助于开发一些用 Deno 编写的第一批 web 应用程序，并将帮助围绕这个新的运行时的社区变得更加强大。

对 Deno 有什么疑问吗？在推特上让我知道[或者回复这篇文章！](https://twitter.com/urcoilbisurco)

## 资源

*   想了解更多关于 Deno 的知识吗？官网有一个很棒的文档:[https://deno.land/manual/introduction](https://deno.land/manual/introduction)
*   想给 Deno 找几个更牛逼的模块？https://github.com/denolib/awesome-deno 有许多其他模块可以探索和使用✨

## **简明英语团队的笔记**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)，[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！我们还推出了一个 YouTube，希望你能通过 [**订阅我们的简明英语频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) 来支持我们**

**一如既往,“简明英语”希望帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****