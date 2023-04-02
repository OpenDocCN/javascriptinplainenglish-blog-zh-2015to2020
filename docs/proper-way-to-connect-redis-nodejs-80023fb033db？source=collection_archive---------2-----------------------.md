# 连接 Redis 的正确方式— Node.js

> 原文：<https://javascript.plainenglish.io/proper-way-to-connect-redis-nodejs-80023fb033db?source=collection_archive---------2----------------------->

![](img/6355df02e03992933f3f42d5d4fde73f.png)

你的客户是否长时间看到加载屏幕？您做了所有正确的事情，但是您仍然得到很高的响应时间甚至超时吗？您希望您的应用速度提高 10 倍、100 倍、10000 倍吗？Redis 是来解决你所有问题的。

这让我想起了 90 年代的电视广告。反正我上面问的都是真题。如果您希望您的应用程序更快，添加内存缓存解决方案可以满足您的需求。有很多选项可供选择，例如你可以使用内存中的 MongoDb，Ignite，Redis，Mem-cache 等。所有这些技术都旨在让您的应用程序运行更快。

我一直在 NodeJs 中使用 Redis & MongoDb 组合，但是这篇文章的目的不是引导您找到完美的缓存策略。那是另一个话题了。

# Redis 是什么？

Redis 是一个键值数据结构存储，用作数据库、缓存和消息代理。Redis 来自远程词典服务器。它使用 ram 来存储数据，因此它提供数据的速度快得惊人。它还支持集群，流，TTL，地理查询，发布/订阅等等。了解更多关于 Redis 的信息[点击此链接](https://redis.io/)。

好了，让我们继续，我们需要设置我们的环境，

# 我们需要什么？

*   NodeJs LTS — 10^
*   Redis 服务器
*   码头工人
*   Visual Studio 代码

# 准备开始

此时，我们需要安装依赖项。首先，我会在我的机器上安装 Redis，顺便说一下，我们有很多选择，但我们是开发者，对不对？让我们用 Docker 运行安装它

另外，我写了关于 Docker 的介绍，如果你想看的话，可以随时通过[点击这个链接](https://medium.com/faun/the-best-docker-introduction-8b518b6f0b05)来看。

如果您已经安装了 Docker，只需执行下面的命令来运行 Redis 服务器实例。

```
docker run -d  --name redis_srv -p 6379:6379 redis
```

如果你不想安装 Docker，我们可以简单地使用 Redis 实验室。打开[此链接](https://redislabs.com/try-free/)创建账户。

# 好了，我们准备好出发了！

按照以下步骤创建环境:

*   创建一个空文件夹进行工作:

```
mkdir connect-redis&& cd connect-redis
```

*   初始化 Node.js 项目时，-y 将跳过以下形式:

```
npm init -y
```

*   创建 **index.js** :

```
touch index.js
```

*   编辑 **package.json** 如图所示:

```
{
  "name": "connect-redis",
  "version": "0.0.0",
  "private": true,
  "scripts": {
    "start": "node index",
  },
  "dependencies": {},
  "devDependencies": {}
}
```

*   安装依赖项:

```
npm install --save redis lodash
```

*   打开 **index.js** ，让我们定义依赖关系和我们的函数:

```
const redis = require('redis');
const _ = require('lodash');const clients = {};
let connectionTimeout;module.exports.init = () => {};module.exports.getClients = () => {};module.exports.closeConnections = () => {};
```

好了，我们定义了函数，让我们填充它们。首先，我们需要创建一个连接实例。我们这样做是因为将来我们可能需要第二次或第三次连接到另一个 Redis 数据库。不要问为什么，一个 Redis 是不够的，不要忘记用你的替换连接字符串！

```
module.exports.init = () => {
    const cacheInstance = redis.createClient('redis://connection-string');
    clients.cacheInstance = cacheInstance;
};
```

我们用 Redis 包创建了 Redis 连接，并将该连接保存在一个对象中，以便以后导入和使用。

根据连接的状态，将触发一些事件。最好是听他们采取行动。因为这个模块将管理整个连接流。我们需要知道任何中断或连接丢失。

让我们看看这些事件是什么:

*   连接:成功连接后触发
*   End:在连接断开时触发
*   重新连接:当 Redis 试图连接时触发
*   错误:发生任何错误时触发

现在让我们添加一个事件监听器，我改变了 init()函数，如下所示，并添加了处理程序。在开发 NodeJs 应用程序时，我更喜欢 RO-RO(接收对象—返回对象)方法。因为它提高了代码的可读性。

```
function instanceEventListeners({ conn }) {
    conn.on('connect', () => {
        console.log('CacheStore - Connection status: connected');
    }); conn.on('end', () => {
        console.log('CacheStore - Connection status: disconnected');
    }); conn.on('reconnecting', () => {
        console.log('CacheStore - Connection status: reconnecting');
    }); conn.on('error', (err) => {
        console.log('CacheStore - Connection status: error ', { err });
    });
}module.exports.init = () => {
    const cacheInstance = redis.createClient('redis://connection-string');
    clients.cacheInstance = cacheInstance;
    instanceEventListeners({ conn: cacheInstance });
};
```

现在怎么办？正如我上面所说，我们将自己处理重新连接程序。按照下面的代码，我只是简单的加了一个`timeout()`。就我而言，我可以忍受很少的连接中断。我的意思是，只要它在一定的时间内重新连接，失去连接是可以的吗？

让我们添加那个`timeout()`来看看它是如何进行的，

```
function throwTimeoutError() {
    connectionTimeout = setTimeout(() => {
        throw new Error('Redis connection failed');
    }, 10000);
}function instanceEventListeners({ conn }) {
    conn.on('connect', () => {
        console.log('CacheStore - Connection status: connected');
        clearTimeout(connectionTimeout);
    }); conn.on('end', () => {
        console.log('CacheStore - Connection status: disconnected');
        throwTimeoutError();
    }); conn.on('reconnecting', () => {
        console.log('CacheStore - Connection status: reconnecting');
        clearTimeout(connectionTimeout);
    }); conn.on('error', (err) => {
        console.log('CacheStore - Connection status: error ', { err });
        throwTimeoutError();
    });
}module.exports.init = () => {
    const cacheInstance = redis.createClient('redis://connection-string');
    clients.cacheInstance = cacheInstance;
    instanceEventListeners({ conn: cacheInstance });
};
```

我们做到了！我们现在有什么？我们来分解一下，

*   我们将`connectionTimeout`定义为空，因为我们想在结束触发特定事件时开始超时，
*   当我们得到一个结束事件，超时将开始，然后 Redis 将尝试重新连接。我们会等到超时。【Redis 尝试连接时，将跟踪时间。如果连接再次建立，我们需要清除超时，因为我们不想在此之后抛出错误。顺便说一下，当`connectionTimeout`设置为空或未定义时，`clearTimeout`不会影响任何事情。

我们留下了一些好东西，让我们填满它们。我想管理关闭连接的流程，如何管理打开的流程。因为如果您在 Kubernetes 中运行应用程序，优雅地关闭应用程序总是最佳实践。

```
module.exports.closeConnections = () => _.forOwn(clients, (conn) => conn.quit());
```

*   `forOwn()`将循环对象键，而每一个连接我们只是简单地称之为`quit()`

利用关系怎么样？让我们添加它，我们就完成了！

```
module.exports.getClients = () => clients;
```

使用功能块，您可以导入和使用您创建的连接。

# 最后

我们到达了终点，为自己创建了一个漂亮的 Redis 连接处理程序。这是我们找到的最适合我们微服务的方式。

处理连接状态并了解发生了什么将会提高您的服务质量。我总是欢迎你的想法，如果你有任何问题或建议，请在评论中告诉我们。

对于完整代码签出，此[链接](https://github.com/irnmon/microservice-base/)，

对于[介绍 Docker checkout 这个链接](https://medium.com/faun/the-best-docker-introduction-8b518b6f0b05)，

对于[滚装进场检验环节](https://www.freecodecamp.org/news/elegant-patterns-in-modern-javascript-roro-be01e7669cbd/)，

[**TL；DR；**](https://github.com/irnmon/microservice-base/blob/master/lib/loaders/cacheStore.js)