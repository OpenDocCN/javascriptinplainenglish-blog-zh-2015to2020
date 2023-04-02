# Node.js 最佳实践—现代特性

> 原文：<https://javascript.plainenglish.io/node-js-best-practices-modern-features-3165f75595fb?source=collection_archive---------5----------------------->

![](img/a7562a35e73b842eef5d889a8a847a15.png)

Photo by [Kalen Emsley](https://unsplash.com/@kalenemsley?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# 使用 ES2015+

从 v4 开始，Node 就支持 E2015 和更高版本的功能。

这意味着我们可以在非常早期的版本中使用这些特性。

现在没有理由不使用它们。

我们不需要 Babel 来使用最新的功能。

阶段 4 的大部分功能都是可用的。

如果我们不确定我们的 Node 版本是否支持我们想要的功能，我们可以检查 [node.green](http://node.green/) 来确定。

# 使用承诺

承诺让我们以干净的方式编写异步代码。

它们让我们的生活变得容易多了。

而不是写:

```
fs.readFile('./foo.json', 'utf-8', (err, data) => {
  if (err) {
    return console.log(err)
  } try {
    JSON.parse(data)
  } catch (ex) {
    return console.log(ex)
  }
  console.log(data.name)
})
```

我们写道:

```
import * as Promise from "bluebird";
const fs = Promise.promisifyAll(require("fs"));fs.readFileAsync('./foo.json')
.then(JSON.parse)
.then((data) => {
  console.log(data.name)
})
.catch((e) => {
  console.error('error reading file', e)
})
```

这要干净得多，因为多重承诺的嵌套更少了。

我们使用 Bluebird 来承诺整个 fs 模块，所以我们可以使用所提供方法的承诺版本。

# 使用 JavaScript 标准样式

为了让每个人的生活更轻松，我们可以对 JavaScript 代码使用标准样式。

这样的话，我们就不用为格式问题争吵了。

[JavaScript 标准样式](https://github.com/feross/standard)是一个有用的样式指南，它用自己的规则涵盖了大多数 JavaScript 语法。

现在我们不必对`.eslintrc`、`.jshintrc`和其他林挺配置文件做出决定。

我们就用他们提供的。

# 使用 Docker

Docker 让一切变得简单。

它让我们可以独立运行我们的应用程序。

它们很轻。

部署不需要手动步骤。

部署是不可变的。

我们可以轻松地用它在本地镜像生产环境。

# 监控我们的应用

如果用户使用我们的应用程序，我们必须确保它们处于运行状态。

我们知道的唯一方法就是监控他们。

我们可以用像普罗米修斯这样的工具来完成。

它会提醒我们任何问题。

此外，它还会显示我们的应用程序的 CPU 和内存使用情况。

也可以进行分布式跟踪和错误搜索。

性能监控也是内置的。

我们还可以用它来检查我们使用的 NPM 包中的安全漏洞。

# 将消息传递用于后台流程

如果我们有后台进程，我们应该在它们之间发送消息，以便在一端宕机时可以保留。

为此，我们可以使用一些消息队列解决方案。例子包括:

*   [RabbitMQ](https://www.rabbitmq.com/)
*   [卡夫卡](https://kafka.apache.org/)
*   [NSQ](http://nsq.io/)
*   [AWS SQS](https://aws.amazon.com/sqs/)

# 使用最新的 LTS 节点版本

LTS 节点版本比非 LTS 版本受支持的时间更长。

支持包括安全补丁和其他错误修复。

因此，我们会想要使用它们。

为了方便切换，我们可以使用`nvm`。

一旦我们安装了 ti，我们就可以运行:

```
nvm install 12.16.1
nvm use 12.16.1
```

分别安装和使用 12.16.1 版本。

我们还可以在 docker 文件中指定节点版本。

![](img/dc19718be5d3137991772e7bb4909f21.png)

Photo by [Adam Perry](https://unsplash.com/@transplanttexan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以通过一些现代技术让我们的生活变得更轻松，例如使用 ES2015+和将我们的应用程序分类。

## **简明英语 JavaScript**

你知道我们有三份出版物和一个 YouTube 频道吗？在 [**找到一切的链接 plainenglish.io**](https://plainenglish.io/) ！