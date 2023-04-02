# Node.js 最佳实践—监控和测试

> 原文：<https://javascript.plainenglish.io/node-js-best-practices-monitoring-and-testing-8197b12c6679?source=collection_archive---------6----------------------->

![](img/2f2b961ae2cc76ad62177c53e4cb672f.png)

Photo by [Louis Hansel @shotsoflouis](https://unsplash.com/@louishansel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# 检查过期的包装

我们可以使用`npm outdated`和`npm-check-updates`工具检查过期的包。

它们检测已安装的过期软件包。

为了实现自动化，我们可以将它注入到 CI 管道中，以便在构建时进行检查。

这样，我们将尽快更新软件包。

# 使用类似生产的环境进行端到端测试

我们应该在类似生产的环境中测试我们的应用程序，以模拟真实的用户交互。

这是端到端测试的全部要点。

我们在一个干净的、类似生产的环境中运行测试，这样我们就知道如果应用程序投入生产会发生什么。

此外，我们应该在每次测试后将数据库重置为原始种子数据，这样所有测试都可以独立运行。

当我们使用 docker-compose 来加速我们的环境时，我们可以使用它来重置我们的数据库。

# 使用静态分析工具定期重构

我们可以使用静态分析工具来检测棉绒没有检测到的东西。

像重复和代码复杂性这样的问题只能由静态分析工具来挑选。

因此，我们必须使用它们来帮助我们检查这些问题，并逐一解决它们。

进行检查的一些工具包括[声纳箱](https://www.sonarqube.org/)和[代码环境](https://codeclimate.com/)。

# 谨慎选择您的竞争情报平台

我们需要一些 CI 平台来运行我们所有的测试和构建我们的应用程序，这样我们就可以轻松地部署它们。

它们应该让我们的生活更轻松。

有很多像詹金斯和切尔莱西这样的选择。

我们必须选择一个具有我们需要的功能和良好的速度。

# 隔离测试中间件

中间件应该单独测试，这样我们可以更早地发现任何错误。

因为它们在许多地方被使用，所以它们的任何错误都会影响许多端点。

我们可以存根并监视请求、响应和`next`函数来进行测试。

# 监视

监控我们的应用绝对是应该一直做的事情。

如果应用程序出现问题，我们必须马上知道。

否则，我们会有很多愤怒和沮丧的用户。

他们有许多图表向我们展示我们的应用程序的进展。

# 使用智能日志记录增加透明度

没有日志记录，我们的应用程序就是一个黑匣子。

如果没有日志，我们就不知道我们的应用在做什么。

因此，我们需要找到一个日志解决方案，以便我们可以轻松地搜索和提取日志活动。

我们需要一个像 Winston 这样有信誉的日志库来做我们的日志。

我们需要放在一个我们可以方便地可视化和搜索数据的位置。

错误跟踪工具也应该给我们有用的度量，比如我们可以检查的错误率。

![](img/2bc109e570bfb2793ac8720a316b1840.png)

Photo by [Alice Pasqual](https://unsplash.com/@stri_khedonia?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该检查过时的包并自动化我们的构建。

此外，监控和智能日志记录是必不可少的。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**