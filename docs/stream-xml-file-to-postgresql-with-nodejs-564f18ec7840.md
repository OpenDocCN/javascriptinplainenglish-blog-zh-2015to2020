# 使用 NodeJS 将 XML 文件流式传输到 PostgreSQL

> 原文：<https://javascript.plainenglish.io/stream-xml-file-to-postgresql-with-nodejs-564f18ec7840?source=collection_archive---------5----------------------->

![](img/97ca24fe3f72648cbfd750199e8dd717.png)

Photo by [Jon Flobrant](https://unsplash.com/@jonflobrant?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我职业生涯的大部分时间都是一名 Java 开发人员，但是最近我对 Node JS 的所有强大功能越来越感兴趣。正因为如此，我决定用 Node JS + Typescript 来实现我最近在工作中不得不处理的一个问题的解决方案。

问题是从远程 XML 文件中提取数据，解析信息，并将其插入 Postgres。表格的结构与此类似:

XML 文件包含欧元外汇参考汇率，这些汇率是公开的，任何人都可以从欧洲中央银行的网站上下载。XML 文件的结构如下:

可以看到，标签***“gesmes:Envelope>Cube>Cube】***包含特定一天的汇率信息。从 1999 年 1 月 4 日到今天，每天的信息都会重复这个标签。

# 流解决方案

解决这个问题的一个可靠方法是使用流。将使用以下软件包:

*   ***https*** :从远程服务器流式传输 XML 文件。
*   ***xml-stream*** :解析 xml 数据流。
*   ***pg*** :连接 Postgres 数据库。
*   ***pg-copy-streams***:将解析后的数据流式传输到 Postgres。

第一步是配置到 Postgres 的连接:

批量插入 Postgres 的一个有效方法是使用 ***COPY*** 命令。这个想法是将解析后的 XML 数据流式传输到 ***COPY*** 命令。这可以通过包***pg-copy-stream***实现，如下所示:

在最后一段代码中，常量 ***数据流*** 包含一个将使用 ***COPY*** 命令将数据加载到 Postgres 中的流。在这种情况下， ***复制*** 命令期望 CSV 格式的数据，因此，包 ***xml-stream*** 将用于将 xml 文件中的信息转换为可以插入到表中的 CSV 格式。此外，需要包 ***https*** 向远程服务器发出 HTTP 请求:

本质上，XML 流被配置为接收传入的 HTTP 响应，并基于两个事件 ***updateElement*** 和 ***end*** 来处理数据。每当一个标签***" gesmes:Envelope>Cube>Cube "***通过 XML 流时，就会发出一个***update element***事件，该事件将用于将数据转换为 CSV 格式，并将该数据发送到数据库流。 ***end*** 事件用于向数据库流指示，不再有数据需要处理。

## 完全码

## 执行时间

显然，执行时间会受到到文件服务器和数据库的连接时间的影响。但至少在我的本地机器上，我能够流式传输一个 6 MB 的文件，转换它的数据，并在 7 秒内插入近 173K 行。