# 如何用 Jest 测试 Redux API 调用

> 原文：<https://javascript.plainenglish.io/how-to-test-your-redux-api-calls-with-jest-64ebdd9c0034?source=collection_archive---------6----------------------->

![](img/6ad6e4e8c811351ce4ea7312e394c146.png)

在本教程中，我们将讨论如何测试 API 如何在我们的应用程序中调用 Jest 并使用 Axios 作为 HTTP 客户端。Redux 允许您处理和管理多条路线上的复杂数据，分派各种类型的动作，但有时处理异步数据可能会很棘手，因为响应会随时间变化。

在下面的示例中，您将看到关于如何使用以下工具处理异步调用的提示和教程:

Jest:React(或其他)应用的主要测试框架；
**Axios** :用于 HTTP 客户端通信；
**Moxios** :模仿我们的 Axios API 调用。

在下一个示例中，我将提供一个使用 Axios 的 get 请求的简单示例:

开始对 get 用户进行单元测试的第一步将是使用 **Moxios** 进行模拟。这个文件必须有 fixture 扩展名(例如:getUsers.fixtures.js)。模拟请求的主要目的显然是测试两种可能的场景:成功和失败。别忘了定义方法和状态！

现在我们已经准备好开始编写我们的测试场景，我们将看看成功和失败的场景是否通过。目标是测试三件事:正确的响应数据、成功的获得和失败。让我们为测试设置初始配置。

在我们的测试配置中，我们开始将 Axios 配置定义为 undefined，因为我们将使用带有默认配置的 Axios。最重要的部分出现在第 5 行和第 9 行。要开始运行您的 Moxios 实例，您需要在每个测试中安装并在结束时卸载它，这样您的测试将能够使用 mockStubs 访问您的 fixtures 文件。另一件重要的事情是探查用户将被调用的方法类型。

在上面的例子中，您可以检查是否涵盖了三种可能的场景:响应数据、成功和失败。不要忘记将您的测试定义为异步的，并在调用模拟的 fixture 之后进行断言。什么是断言？

> Expect.assertions(number)验证在测试过程中调用了一定数量的断言。这在测试异步代码时非常有用，可以确保回调中的断言确实被调用了。

Moxios 将帮助您加快测试速度，并保证所有的覆盖率都得到满足，因为它是一个基于承诺的解决方案。

## **用简单英语写的 JavaScript 笔记**

我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名在[**【submissions@javascriptinplainenglish.com】**](mailto:submissions@javascriptinplainenglish.com)给我们发电子邮件，我们会把你添加为作者。

我们还推出了三种新的出版物！请关注我们的新出版物: [**AI in Plain English**](https://medium.com/ai-in-plain-english) ，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**