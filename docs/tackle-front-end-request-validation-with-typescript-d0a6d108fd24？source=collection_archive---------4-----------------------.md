# 用 TypeScript 处理前端请求验证

> 原文：<https://javascript.plainenglish.io/tackle-front-end-request-validation-with-typescript-d0a6d108fd24?source=collection_archive---------4----------------------->

## *您确定您的 JSON 响应具有您期望的格式吗？如果没有呢？*

![](img/fd19ed9a2c618afa6d856b1e8cabda10.png)

Photo by [Raúl Nájera](https://unsplash.com/photos/MggK54YixfU?utm_source=unsplash&utm_medium=referral&utm_content=creditShareLink) on [Unsplash](https://unsplash.com/s/photos/danger-sign)

你好，前端开发人员，我希望我激起了你的兴趣。回答我自己的问题:*“不，我甚至可以肯定，在某个时刻，我的端点会以一种我不期望的格式来回答”*。

# 在前端验证请求响应的动机

不管您使用的是哪种技术/库/框架，一旦您开始使用一个前端和后端分离的 web 应用程序，就会有一个风险，即在您编写请求之后，响应的规范会发生变化。

在我参与的每个项目的某个时候，我们有一个端点在没有完全通知前端团队的情况下获得更新。

出现的问题:

*   信息在一段时间内显示不佳或为空
*   一些特定的(和嵌套的)屏幕崩溃时被打开
*   不满意的用户
*   这也意味着沟通和测试过程有弱点

像[端到端测试](https://medium.com/@davert/javascript-the-future-of-end-to-end-testing-bfc00e23110b)或者更好的[契约测试](https://medium.com/@liran.tal/a-comprehensive-guide-to-contract-testing-apis-in-a-service-oriented-architecture-5695ccf9ac5a)这样的实践，允许你预先测试你的整个系统和它的请求，以确保一切按预期工作。

[](https://medium.com/@davert/javascript-the-future-of-end-to-end-testing-bfc00e23110b) [## JavaScript:端到端测试的未来

### 浏览器测试自动化的现状

medium.com](https://medium.com/@davert/javascript-the-future-of-end-to-end-testing-bfc00e23110b) [](https://medium.com/@liran.tal/a-comprehensive-guide-to-contract-testing-apis-in-a-service-oriented-architecture-5695ccf9ac5a) [## 面向服务的架构中契约测试 API 的综合指南

### 您很可能经历过部署到生产环境中却发现某个 API 服务…

medium.com](https://medium.com/@liran.tal/a-comprehensive-guide-to-contract-testing-apis-in-a-service-oriented-architecture-5695ccf9ac5a) 

但是，根据您正在进行的项目，这可能并不完全适合您:

*   您无法控制您调用的端点。
*   您可能没有 100%的测试或案例覆盖率。
*   你还没有为这样或那样的练习做好准备。
*   您的流程尚未完全建立。
*   *墨菲定律。*

考虑到这一点，我有两个愿望:

*   实时了解我的应用在测试和生产中是否收到了格式与其主要定义不匹配的 JSON 响应。
*   使用默认值，而不是让我的应用程序因为丢失或重命名的属性而崩溃。

# 让我们谈谈打字稿

*由于我们需要一个真实的用例，我们将以 axios 的“登录”请求为例。这里* *可以找到完整的* [*例子。*](https://github.com/morintd/example-axios-validation)

## 安装

假设您正在开发新的前端应用程序，并开始发出请求。首先用`yarn add axios`安装 axios 并设置它:

因为您正在设置登录，所以您定义了以下类型:

*   您的请求参数:

*   您的请求期望得到响应:

*   以及您的函数预期返回的数据类型:

因此，您可以轻松可靠地编写您的请求函数:

你有你需要的一切:

*   由于使用了 TypeScript，降低了打字错误和错误转换的风险
*   没有正确的参数，无法调用此函数
*   错误密码的处理错误

这就是我们需要的一切，对吗？现在，如果在某个时候，`createdAt`和`updatedAt`将从`user`中移除，而只出现在`signUp`中，会怎么样呢？

甚至可以说，你没有被告知，你用这个数据来显示注册以来的用户天数。如果您没有一个健壮的测试/质量过程，并且这进入了生产，那么您的应用程序很可能会抛出一个错误。

这是一个显而易见的功能，我们不是在谈论隐藏的或很少使用的功能。现在，我们的问题是如何验证传入的请求具有预期的格式。

## 确认

我们有一个很好的起点，打字稿！我们已经定义了预期的请求响应，所以也许我们可以用它来验证传入的请求。

我们很幸运，有很多实用程序可以与大量的验证库一起使用 TypeScript。

您可以:

*   将您的类型编译成 JSON-schema！像`[typescript-json-schema](https://github.com/YousefED/typescript-json-schema)`和`[ts-json-schema-generator](https://www.google.com/search?client=firefox-b-d&q=ts-json-schema-generator)`这样的模块可以帮你做到。
*   然后，您可以使用诸如 [Yup](https://www.npmjs.com/package/yup?activeTab=readme) (带有 [json-schema-yup-yup](https://www.npmjs.com/package/json-schema-yup-transformer) )和 [Ajv](https://github.com/ajv-validator/ajv) 之类的库来将一个对象与生成的 json-schema 进行匹配。
*   另一个选择可能是 [Joi](https://github.com/sideway/joi) with [Joiful，](https://github.com/joiful-ts/joiful)但是这是一个使用[浏览器](http://browser)很难操作的库。

Ajv 是最简单的，也是我们今天要用的。

如果您想深入研究 Ajv 和 JSON-Schema，我可以推荐您从那里开始:

[](https://medium.com/swlh/an-introduction-to-json-schema-8eaea643fcda) [## JSON 模式介绍

### 我最近开始使用 JSON Schema，并开始意识到它们可以给开发者带来的价值，以及在…

medium.com](https://medium.com/swlh/an-introduction-to-json-schema-8eaea643fcda) [](https://medium.com/nsoft/using-ajv-for-schema-validation-with-nodejs-1dfef0a372f8) [## 使用 AJV 对 NodeJS 进行模式验证

### 如果你是一个开发 API 的开发者，你需要考虑的无数事情中包括验证用户输入和…

medium.com](https://medium.com/nsoft/using-ajv-for-schema-validation-with-nodejs-1dfef0a372f8) 

我对前面提到的生成 JSON 模式的工具并不满意。我不想编写和维护自己的脚本，而是希望有一个简单明了的 CLI。

这就是为什么对于这个前端请求验证项目，我创建了我自己的。构建在`[typescript-json-schema](https://github.com/YousefED/typescript-json-schema)`之上，只有一个命令。

*   用`yarn add -D ts-generate-schema`安装。
*   用给定的扩展名命名要生成 JSON-Schema 的文件(例如. response.ts)。
*   在 package.json 中添加以下代码行:

```
"schema": "ts-generate-schema src/**/*.response.ts"
```

*   运行`yarn schema`来生成所有的 JSON-Schema。

[](https://github.com/morintd/ts-generate-schema) [## morintd/ts-生成模式

### 从您的 typescript 定义生成 json-schema 文件

github.com](https://github.com/morintd/ts-generate-schema) 

## 在实践中

一旦生成了模式，一切都可以通过 axios 配置和拦截器来设置。

[](https://medium.com/datadriveninvestor/axios-instance-interceptors-682868f0de2d) [## Axios 实例和拦截器

### Axios 是最常用的 promise base http 客户端之一，它不仅仅是 get 和 post

medium.com](https://medium.com/datadriveninvestor/axios-instance-interceptors-682868f0de2d) 

出于性能原因，Ajv 允许您首先从 JSON-Schema 编译一个 validate 函数，然后使用它。让我们告诉 TypeScript 在我们的 axios 配置中允许这个验证函数。

所以现在，我们可以编译我们的 JSON-Schema，TypeScript 将允许我们在发出请求时添加这个函数。

现在，我们可以添加拦截器来最终验证我们的请求:

在这里，错误只是被发送到一个 console.error，但是应该通过一个监控工具来处理，比如 [sentry.io](https://sentry.io/) 。

## 误差容限

我的一个愿望实现了。太好了！但是另一个呢？

在我看来，如果响应中缺少给定的属性，发送一个假值是合理的:

*   显示一个空的用户名，而不是我的整个应用程序关闭。
*   在我们的用例中，显示用户从 0 天开始注册，而不是一个大红色的“RangeError: Invalid date”。
*   唯一的假设是，我会知道它何时发生，这就是我们刚刚设置的！

我的解决方案被称为“深度合并”。这是许多可能的策略之一。如果我收到一个格式错误的响应，我至少会尝试将其与一个有效的(但为空的)对象深度合并。

让我们用`yarn add deepmerge`安装一个深度合并模块:

[](https://github.com/TehShrike/deepmerge) [## teh shrike/深合并

### 深度合并两个或多个对象的可枚举属性。

github.com](https://github.com/TehShrike/deepmerge) 

该过程将与验证过程相同。让我们在 axios 配置中允许另一个名为 nullRawData 的键(不言自明吧？):

在原始响应的定义文件中，创建空对象。

然后我们可以导入并在我们的请求中使用它。

然后，我们可以通过深度合并来完成之前的验证策略。

现在，如果由于未知的原因我的 API 发生了变化，那么我的应用程序宕机或任何变化被忽略的风险就会大大降低。

当前的策略甚至可以通过检查属性类型是否无效并将其与默认的有效类型合并来改进。

示例将保存在[这里](https://github.com/morintd/example-axios-validation)。

*感谢阅读*。

[](https://medium.com/@morin.td/about-me-teddy-morin-9fb1d65fe24e) [## 关于我——泰迪·莫林

### 嗨，我是泰迪，一个反应的爱人🚀。

medium.com](https://medium.com/@morin.td/about-me-teddy-morin-9fb1d65fe24e) 

延伸阅读:

(中)JavaScript:端到端测试的未来:[**https://Medium . com/@ davert/JavaScript-The-Future-of-End-to-End-Testing-bfc 00 e 23110 b**](https://medium.com/@davert/javascript-the-future-of-end-to-end-testing-bfc00e23110b)

(中)面向服务架构中合约测试 API 综合指南:[**https://Medium . com/@ liran . tal/A-Comprehensive-Guide-to-Contract-Testing-APIs-in-A-Service-Oriented-Architecture-5695 CCF 9 AC 5a**](https://medium.com/@liran.tal/a-comprehensive-guide-to-contract-testing-apis-in-a-service-oriented-architecture-5695ccf9ac5a)

(GitHub)前端请求验证示例:[**https://github.com/morintd/example-axios-validation**](https://github.com/morintd/example-axios-validation)

(中)JSON 模式介绍:[**https://Medium . com/swlh/An-introduction-to-JSON-Schema-8 AEA 643 fcda**](https://medium.com/swlh/an-introduction-to-json-schema-8eaea643fcda)

(GitHub)ts-generate-schema:[https://github.com/morintd/ts-generate-schemaT21](https://github.com/morintd/ts-generate-schema)

(中)Axios 实例&拦截器:[**https://Medium . com/datadriveninvestor/Axios-Instance-Interceptors-682868 f0de 2d**](https://medium.com/datadriveninvestor/axios-instance-interceptors-682868f0de2d)

(GitHub)deep merge:[**https://github.com/TehShrike/deepmerge**](https://github.com/TehShrike/deepmerge)