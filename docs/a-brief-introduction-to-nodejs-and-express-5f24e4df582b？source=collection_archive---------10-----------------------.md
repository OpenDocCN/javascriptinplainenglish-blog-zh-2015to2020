# NodeJS 和 Express 简介

> 原文：<https://javascript.plainenglish.io/a-brief-introduction-to-nodejs-and-express-5f24e4df582b?source=collection_archive---------10----------------------->

![](img/71968cd8b5d5674bc178c421ac358115.png)

## 什么是 NodeJS？

简而言之，NodeJS 是浏览器之外的 JavaScript。它是你后端的 JavaScript 运行时环境，也是一个运行其他程序的程序。

## NodeJS 的四个关键特性

1.因为 NodeJS 本质上只是服务器上的 JavaScript，所以它使得在客户端和服务器之间共享代码变得很容易，并且它是 JavaScript 开发人员容易学习的后端环境。

2.它还是单线程的，消除了 Java 等语言中处理多线程的复杂性。

3.这是*异步*(两个或更多的对象或事件**不是**同时存在或发生)，这很重要，因为节点进程将在同一个 CPU 上运行。

4.它包含一个 npm 存储库，这意味着它有一个充满有用的库和框架(如 Express)的节点模块集合。

## 什么是快递？

Express 就像 React 一样适用于您的后端。它是一个位于 NodeJS web 服务器 http 之上的框架。Express 增加了额外的功能，比如路由、*中间件*和一个干净的 API。

*中间件:*中间件是可以获取 req 和 res(请求和响应)对象，管理它们，并且当被请求时，可以触发一个动作的功能。换句话说，中间件功能可以执行以下任务:

1.执行代码
2。对 req 和 res 进行更改(尽管这不是必须的)
3。结束请求& res 循环
4。调用堆栈中的下一个中间件函数

您可以将 express 中间件堆栈视为一组函数。

Express 只需一行代码就可以将 HTML 文件或图像发送到浏览器。没有它，我们不得不使用 HTTP 模块，这需要更多的代码来完成这个简单的任务。

下面是一段代码，展示了使用 Express 编写的服务器

```
const express = require('express'); // we use the word require to import the express package

const server = express(); // creates the server

// handles the get requests to the root of the api, the / route
server.get('/', (req, res) => {
  res.send('Hola from Express');
});

// Listens for connections on port 6000
server.listen(6000, () =>
  console.log('Server is running on http://localhost:6000')
);
```