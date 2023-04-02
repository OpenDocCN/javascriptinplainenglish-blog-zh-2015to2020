# Nest.js & RabbitMQ 中的死字——这可能吗？

> 原文：<https://javascript.plainenglish.io/dead-lettering-in-nest-js-rabbitmq-is-it-even-possible-a6aa5732ef1d?source=collection_archive---------2----------------------->

上周，我发现自己在努力寻找一些关于与 Nest.js & RabbitMQ 的死信交换的文档。有些 GitHub 问题推荐使用外部包，但是你实际上可以用普通的 Nest.js 来实现！

我们需要一些关于 RabbitMQ 和 Nest.js 的基础知识。

> 完整的源代码可在[这里](https://github.com/NiklasPor/nestjs-rmq-dead-lettering)获得。如果你想直接跳到前面。

![](img/89e90bba62d244ee987d7195ece31df1.png)

Photo by [Joanna Kosinska](https://unsplash.com/@joannakosinska?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 你说的这个死字是什么？

RabbitMQ 死信消息，以下[点中至少有一点适用于该消息](https://www.rabbitmq.com/dlx.html):

*   消息被拒绝或未用`requeue: false`确认。
*   消息的 TTL(生存时间)已过期。
*   该消息已被丢弃，因为它超过了队列的最大长度。

如果指定了死信交换，则死信消息将被转发到死信交换。在我们的例子中，我们只是将它们转发到另一个 RabbitMQ 队列，并在那里进行处理。这种模式非常适合实现容错工作者！

# 我们将建造什么？

很高兴你问了！我们将构建两个模拟汉堡制作的微服务。可悲的是，我们的厨师放弃了三分之一的汉堡肉饼。我们将引入一些错误处理来解决这个问题。

![](img/4fec789c57087f4ce1b887af492114e9.png)

Sequence Diagram of our Burger Ordering

正如我们所见，制作一个汉堡有 4 个组成部分:

1.  汉堡队列:接收关于汉堡制作的所有信息，并与顾客沟通。
2.  Burger Worker:获取来自`Burger Queue`的消息并处理它们。
3.  恢复队列:接收所有死信。
4.  恢复工作者:从`Recovery Queue`获取消息，并将它们重定向到`Burger Queue`。还将重试次数增加 1。如果重试次数超过 4，则发出失败。

# 设置 RabbitMQ

设置 RabbitMQ 最简单的方法是使用 [Docker](https://docs.docker.com/get-docker/) :

```
docker run -d -p 15672:15672 -p 5672:5672 rabbitmq:3-management
```

执行这个命令后，您的 Docker 守护进程中将运行一个 RabbitMQ。在`amqp://guest:guest@localhost:5672`有，在`[http://localhost:15672](http://localhost:15672.)` [可以进入管理界面。](http://localhost:15672.)

> 注意:密码和用户名都是`*guest*`。您可以使用它们来访问 RabbitMQ 本身和管理界面。

# 设置 Nest.js

首先，我们将通过 npm 在全球范围内安装 Nest CLI:

```
npm i -g @nestjs/cli
```

然后我们必须创建一个名为`burger-worker`的新项目:

```
nest new burger-worker
```

现在向项目添加第二个名为`recovery-worker`的应用程序:

```
cd burger-worker
nest g app recovery-worker
```

接下来，将名为`shared`的库添加到项目中。我们将使用它来共享连接和消息信息:

```
nest generate lib shared
```

当询问前缀时，只需保留默认的`@app`并按回车键。

最后，我们必须添加一些用于连接 RabbitMQ 和创建微服务的包:

```
npm i @nestjs/microservices amqplib amqp-connection-manager
```

# 创建 RabbitMQ 和死字连接参数

为了给两个项目提供选项，我们将在共享库中创建一个`queue-options.ts`文件。这包括死信交换的设置:

libs/shared/src/queue-options.ts

# 创建消息

我们还将添加一个`messages.ts`文件到我们的共享库中。这将包括所有消息。每条消息由两个主要部分组成:

1.  模式:模式是一个`string`或`symbol`，用于标识消息的类型。
2.  有效负载/数据:有效负载是消息中提供的实际数据。在我们的例子中，主要是与汉堡相关的东西和重试次数。

libs/shared/src/messages.ts

请记住导出“index.ts”文件中的消息和队列选项:

libs/shared/src/index.ts

# 将汉堡工人与兔子连接起来

首先:由于我们不会实现任何复杂的业务逻辑，我们可以安全地移除`app.service.ts`及其所有引用。

## 主页面

我们需要替换`apps/burger-worker/src/main.ts`的内容。这创建了一个完整的应用程序，但我们只需要一个微服务，它将使用`index.ts`。

我们可以用这个简单的设置代码完全替换`main.ts`:

apps/burger-worker/src/main.ts

## 应用程序模块

此外，我们需要更新汉堡工人的`app.module.ts`。我们想要向`burger-queue`发送消息。Nest.js 的`ClientModule`会给我们提供这个功能:

apps/burger-worker/src/app.module.ts

## 首次启动

如果您已经完成了上述所有步骤，并且还清理了您的`app.controller.ts`，那么您现在就可以启动`burger-worker`！

```
nest start -w burger-worker
```

您的 CLI 应该看起来像这样:

![](img/f139e282a879495e96b6bb6d517e1d18.png)

这也将在我们的 RabbitMQ 实例中创建`burger-queue`。您可以通过查看队列选项卡来检查是否一切正常:

[http://localhost:15672/#/queues](http://localhost:15672/#/queues)。

![](img/755488dc10492dd6b89fb1e4b629a87f.png)

> 登录管理界面的密码和用户名均为`*guest*`。以防你忘了👌

# 实现汉堡工人

最后，我们可以着手实际的实现。我们将使用`@EventPattern`装饰器从我们的`Burger Queue`接收事件(消息)。`@Payload`和`@Ctx`装饰器可以用来接收消息上下文。我们需要消息上下文来确认或拒绝消息。

为了处理 make burger 消息，我们现在可以使用我们定义的模式和`@EventPattern`装饰器:

apps/burger-worker/src/app.controller.ts

如果汉堡可以毫无问题地准备好，我们发出成功**和**确认我们处理了消息。RabbitMQ 将知道它可以从队列中删除消息。

## 添加对失败和成功的处理

我们将添加一些简单的日志，记录我们在制作汉堡时是否成功。另外，将这些处理程序添加到您的`app.controller.ts`中。

apps/burger-worker/src/app.controller.ts

> 你总是可以在这里比较你的库和完成的库。或者看一下`*Burger Worker*`的成品 [app.controller.ts](https://github.com/NiklasPor/nestjs-rmq-dead-lettering/blob/master/apps/burger-worker/src/app.controller.ts) 。

## 试试吧！

启动 burger worker 并通过 RabbitMQ 接口提交一些消息。要向队列发送消息，只需单击它并提供消息正文:

![](img/fd499a2599c650f823ad42f4f4d69059.png)

[http://localhost:15672/#/queues/%2F/burger](http://localhost:15672/#/queues/%2F/burger)

在发布消息 **2** 次后，CLI 应该显示第一次成功，然后显示一次失败(因为厨师掉了第三个汉堡肉饼)。失败后，这条信息被废弃了。

![](img/812981811f9140d9cfdb76c43a3f99e7.png)

output after publishing the message **2** times

> 但是等等！我们没有实现恢复工人，它处理我们的死信消息！

# 将恢复工作程序连接到 RabbitMQ

这和汉堡工人的情况基本相同。除了我们必须将`recovery-queue`而不是`burger-queue`添加到`main.ts`中。

apps/recovery-worker/src/main.ts

`app.module.ts`完全相同，因为我们仍然只向`Burger Queue`和**发送消息，而不是向`Recovery Queue`发送**:

apps/recovery-worker/src/app.module.ts

# 实施恢复逻辑

恢复逻辑非常简单。我们正在增加消息的重试计数有效负载，并再次将其发送到`Burger Queue`。如果超过了重试阈值，我们会发送一条失败消息。

apps/recovery-worker/src/app.controller.ts

# 运行您的可扩展和容错应用🎉

打开两个终端标签，运行`nest start -w burger-worker`和`nest start -w recovery-worker`。现在发布和以前一样的消息，并对其进行处理！

![](img/f3b7ef9e51026b35297b027b129303d7.png)

Output for: {patties: 2, customer: “John”}

如果我们现在点了 3 个以上的汉堡(记住我们的厨师每 3 个就失败一个)，这个订单总是会失败。但是首先，我们将执行最多 4 次重试。如我们`Recovery Worker`中所述:

![](img/f8bf069bda9449a82d829ee5373ccb13.png)

Output for: {patties: 4, customer: “Someone Hungry”}

如果没有`Burger Worker`可用，消息将在 4 秒后超时。然后消息被我们的`Recovery Worker`处理。重试 4 次后，工人将发出失败消息:

![](img/66d2c647fad2a74f32e08b5f5289b906.png)

Output for no burger worker & {patties: 2, customer: “Emilia”}

> 这个汉堡实验到此结束。如果你想进一步试验，你可以试着启动每个 worker 的多个实例。看一看，看看是否一切顺利！

恭喜你！你现在是少数几个知道如何在 Nest.js 中使用死字体的人之一👌非常感谢您花时间阅读这篇文章。如果你有任何公开的问题，请在 Twitter [@niklaspor](https://twitter.com/niklaspor) 上给我打电话，或者发表评论。

> 完成项目的源代码可在[这里](https://github.com/NiklasPor/nestjs-rmq-dead-lettering)获得。

祝您愉快！

尼克拉斯