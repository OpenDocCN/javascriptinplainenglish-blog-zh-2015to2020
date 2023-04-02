# 面向数据工程师的 JavaScript

> 原文：<https://javascript.plainenglish.io/javascript-for-data-engineers-ccce214e9aff?source=collection_archive---------1----------------------->

![](img/e866e1a73c5335e49394afb581ab064c.png)

Photo by [Fotis Fotopoulos](https://unsplash.com/@ffstop?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/programming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## 数据工程

## 面向数据工程师的基于 JavaScript 的开源 SQL、编排和 ETL 工具简介

最新的 StackOverflow 开发者调查认为 JavaScript 是最受欢迎的技术，紧随其后的 SQL 是第三大受欢迎的技术。前者被认为是一种客户端脚本/前端语言，直到几年前基于 JavaScript 的服务器得到广泛关注。从那时起，在软件开发的保护伞下，几乎所有主要工作领域都启动了 JavaScript 项目。数据工程就是这样一个领域，JavaScript 在这个领域的使用比以往任何时候都多。

已经有很多用 JavaScript 写的可视化库比如 D3.js，C3.js，Charts.js 等等。[已经有很多关于它们的文章](https://linktr.ee/kovid)，但是没有关于处理数据库、数据清理、ETL、数据管道编排等核心数据工程工具的。让我们来看看一些最流行和最有用的活动 JavaScript 项目，数据工程师可以在他们的*当前工作*中学习和使用。

## [**Knex.js**](https://knexjs.org)

它是 PostgreSQL、MySQL、MariaDB、MSSQL、Oracle 和 Amazon Redshift 的查询生成器。到目前为止，它已经有超过 300 个贡献者和大约 150 个版本，是目前最流行的查询构建器。*有趣的事实*—[SLO Nik](https://github.com/gajus/slonik)的作者写了这篇关于为什么使用 [Knex.js](https://github.com/knex/knex) 不利于动态查询构建的文章，受到了很多批评——他在下面的文章中提出了一些普遍的优点，但不足以让我相信 Knex.js 是不好的！

[](https://medium.com/@gajus/stop-using-knex-js-and-earn-30-bf410349856c) [## 停止使用 Knex.js(赚取 30 美元)

### 使用 SQL 查询生成器是一种反模式

medium.com](https://medium.com/@gajus/stop-using-knex-js-and-earn-30-bf410349856c) 

## [**阿拉 SQL**](https://github.com/agershun/alasql)

基于 JavaScript 的浏览器数据库，适用于移动应用程序、浏览器和 node.js 应用程序。它真的很擅长处理 CSVs & Excel 文件。这个项目在 GitHub 上有 5.2K 的星级，在 npm 上每月有 51K 的下载量。它在一周前的最后一次代码推送中保持得相当好。

## 库普斯

Koop 项目有两个值得关注的项目——第一个项目毫不奇怪地被称为 Koop——它是一个用于地理空间数据的 ETL 工具。该项目得到了很好的维护，并由 ESRI 赞助，这是我们在谈论位置智能时要谈论的公司。

称它为完整的 ETL 工具是错误的。首先，它只适用于地理空间数据，因为它支持的转换与地理空间数据相关。例如，将地理空间数据动态转换为 GeoJSON 和矢量切片。

## [诺弗洛](https://noflojs.org/documentation/)

基于组件的编程环境，遵循基于流程的编程原则(程序的逻辑在图中定义)。许多人会将此与气流或类似的管弦乐联系起来。虽然在某种意义上，NoFlo 可以被编程为有点像管弦乐队一样使用，但它的范围比流行的管弦乐队稍大。可以使用 noflo-nodejs 在 Node.js 上执行 noflo 代码。

## [恩普加](https://github.com/taskrabbit/empujar)

这是 TaskRabbit 对开源数据工程社区的贡献。Empujar 是一个 ETL 工具，可以用来做大量的数据移动，包括创建和存储备份。目前，Empujar 已经支持 MySQL、亚马逊红移、Elasticsearch 和 S3。可以很容易地创建自定义连接器来包含其他数据库或数据源。虽然这个工具已经存在一段时间了，但值得一提的是，由于构建失败，最新的 PR 没有合并。

## **荣誉提名**

*   这是一个简单的任务运行器，帮助你自动完成一些繁重的工作，比如确保代码被格式化、链接、精简等等。
*   [**BookshelfJS**](https://bookshelfjs.org)—基于 Knex.js 构建的 ORM，支持事务，支持关系加载，支持 1:1，1:n 和 n:n 关系。
*   [**objection js**](https://github.com/Vincit/objection.js)—基于 Knex.js 的 Node.js，全面支持 MySQL、MariaDB 和 PostgreSQL。
*   [**SLO Nik**](https://github.com/gajus/slonik)—PostgreSQL 的基于 Node.js 的 SQL 客户端，促进编写原始 SQL，不鼓励临时动态生成 SQL。

GitHub 上还有很多其他项目，但我没有选择其中的很多，因为它们中的大多数都不是最新的，并且很久没有代码签入了。

总之，我们可以说，有许多基于 JavaScript 的、维护良好的开源存储库来帮助处理日常的*数据工程*工作——生成 SQL、与数据库交互、将数据从一个地方移动到另一个地方、集成数据并将其可视化。随着 JavaScript 成为未来最有前途的语言之一，学习 JavaScript 是值得的，因为它会被更广泛地使用。

## 简单英语的 JavaScript

你知道我们有三份出版物和一个 YouTube 频道吗？在[T3【plain English . io找到一切的链接！](https://plainenglish.io/)