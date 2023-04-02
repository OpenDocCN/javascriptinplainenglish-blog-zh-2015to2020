# Node.js 最佳实践—安全性和结构

> 原文：<https://javascript.plainenglish.io/node-js-best-practices-security-and-structure-936b4ad32af3?source=collection_archive---------5----------------------->

![](img/41a00927f2afa29c8f49717367895680.png)

Photo by [George Bonev](https://unsplash.com/@spktwo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# 如果我们正在编写一个 Web 应用程序，请使用`Helmet`

如果我们正在写一个网络应用程序，我们应该使用头盔。

它有几项功能，包括:

*   添加 XSS 保护
*   用`X-Frame-Options`防止点击劫持
*   强制所有连接成为 HTTPS
*   设置`Context-Security-Policy`标题
*   禁用`X-Powered-By`响应头，这样攻击者就无法缩小我们用来编写应用程序的库的范围。

头盔将为所有这些选项设置合理的默认值。

我们可以通过运行以下命令来安装它:

```
npm install helmet
```

在我们的 Express 应用程序中，我们可以通过编写以下内容来使用中间件:

```
const helmet = require('helmet');
app.use(helmet());
```

# 监控我们的应用

如果我们正在运行我们的应用程序，那么我们需要监控它。

如果我们的应用程序出现故障，并且没有快速的解决方案，用户会不高兴。

因此，我们需要监控我们的应用程序并提醒每个人，以便我们可以快速恢复运行。

例如， [KeyMetrics.io](https://keymetrics.io/) 与 PM2 集成，以检查应用程序的健康状况。

还提供了一个仪表板，向我们显示它何时启动或未启动。

可以检查延迟和事件。

# 测试我们的代码

我们可以用自动化测试来测试我们的代码，这样当我们修改代码的时候就可以安心了。

它们自动快速运行，因此我们不必亲自检查应用程序的每个部分。

我们应该在修复错误时添加测试，并定期运行我们的测试。

有几种方法可以进行测试。

我们可以用摩卡、茶、Jest 或 Jasmine 来运行它们。

它们都很受欢迎，并提供相同的功能。

为了创建发出请求的测试，我们可以使用 Supertest 来发出请求并检查结果。

# 按组件构建解决方案

我们应该通过组件来构建我们的项目。

这样，我们以后就能找到他们。

如果一个项目没有结构，很容易迷失方向。

我们应该把代码分成模块。

# 将我们的组件分层，并保持 Express 在其边界内

我们应该只对应用程序的控制器部分使用 Express。

业务逻辑应该在它们自己的模块中，以便更好地组织我们的应用程序。

这样，每个模块和功能都做自己的事情。

将不同的部分混合在一起会使它们难以测试和维护。

# 将常用工具包装成 NPM 包

如果我们有在多个项目中使用的公共事物，我们应该把它们放在它们自己的包中。

日志记录、加密等常见功能。应该在他们自己的包裹里。

这样，我们只需更改一个包来更新功能。

# 将 Express“应用程序”和“服务器”分开

我们的 Express 应用程序不应该是一个大文件。

入口点应该与应用程序的其他部分分开。

一个大文件会让一切都变慢。

API 应该在`app.js`中，网络代码应该在`www`中。

如果 API 声明很大，也可以将其拆分成组件。

![](img/c7c9139c66aae9af7bd4a2bfa96fd4cc.png)

Photo by [Cristina Anne Costello](https://unsplash.com/@lightupphotos?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以更好地组织我们的应用程序。

此外，我们可以采取措施来提高安全性和监控我们的应用程序。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**