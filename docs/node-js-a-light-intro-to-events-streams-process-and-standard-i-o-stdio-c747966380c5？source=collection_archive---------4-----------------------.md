# Node.js |对事件、流、流程和标准 I/O (STDIO)的简单介绍

> 原文：<https://javascript.plainenglish.io/node-js-a-light-intro-to-events-streams-process-and-standard-i-o-stdio-c747966380c5?source=collection_archive---------4----------------------->

![](img/0c72b2e4fa34abae7e6e9cacac23405b.png)

Photo by [AltumCode](https://unsplash.com/@altumcode?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

本文是对一些 Node.js 基础知识的简单介绍。打开一个类似 Unix 的操作系统，使用命令“node yourFile.js”运行代码，感觉很受鼓舞。

我们将介绍:
-流程对象
-事件
-流和标准输入输出

# 过程对象

什么是过程？进程是处理器正在处理的程序的实例。一个过程有许多属性。Node.js 引入了 process 对象来揭示流程属性。

```
//Here we count 'keys' of the process object.
"use strict";  
console.log(Object.keys(process).length); //68
```

如您所见，process 对象的这个实例有 68 个属性。

# **输出:**

```
68
```

如下所示，我们可以检查进程 ID 和进程内存使用情况。

```
"use strict";
console.log(process.pid);  
console.table(process.memoryUsage());
```

# 输出:

```
10379
┌───────────┬──────────┐
│  (index)  │  Values  │
├───────────┼──────────┤
│    rss    │ 31129600 │
│ heapTotal │ 6537216  │
│ heapUsed  │ 3988936  │
│ external  │   8272   │
└───────────┴──────────┘
```

# 事件

Node.js 流程对象也是一个 EventEmitter 实例。这意味着它可以使用“on”方法侦听事件，并且可以使用“emit”方法发出事件。

 [## Node.js v15.3.0 文档

### 编辑描述

nodejs.org](https://nodejs.org/dist/latest-v14.x/docs/api/events.html#events_class_eventemitter) 

```
"use strict";
// Here we create an event with 'hello' namespace

process.on('hello', () => console.log('hello'))process.emit('hello')
```

# 输出:

```
hello
```

当一个进程退出时，它发出一个“退出”事件。并将退出代码传递给事件侦听器的回调函数。

回调函数定义中的任何同步代码都会被执行。异步代码不会。

```
"use strict";
// The process object is listening for an "exit" event

process.on('exit', (code) => {
  setTimeout(() => console.log('This wont be logged'))
  console.log('This will be logged')
  console.log(code); // exit code 
});
```

**退出代码 0 表示该过程成功完成。**

# 输出:

```
 This will be logged
0 
```

让我们看看当我们抛出一个错误时会发生什么。

```
 "use strict";
process.on('exit', (code) => {
  setTimeout(() => console.log('This wont be logged'))
  console.log('This will be logged')
  console.log(code); // exit code    
});throw new Error();
```

**退出代码 1 表示进程因出错而退出。**

# 输出:

```
 This will be logged
1
/home/ec2-user/environment/process.js:9
throw new Error();
^Error
    at Object.<anonymous> (/home/ec2-user/environment/process.js:9:7)
```

我们还可以使用“events”模块和“event emitter”类创建一些普通的事件发射器实例。许多节点对象(包括流、HTTP 请求、响应)都继承自 EventEmitter。

现在让我们看一个简单的事件实例。

```
 "use strict";
const EventEmitter = require('events')//Initialize an event to myEventconst myEvent = new EventEmitter()// setup a listener for an eventmyEvent.on('hello world', () => console.log('hello world'))// emit eventsmyEvent.emit('hello world');
myEvent.emit('hello world');
myEvent.emit('hello world'); 
```

# 输出:

```
 hello world
hello world
hello world 
```

# 流和标准 IO(标准 IO)

[](https://en.wikipedia.org/wiki/Standard_streams) [## 标准流

### 在计算机程序设计中，标准流是计算机之间互连的输入和输出通信通道。

en.wikipedia.org](https://en.wikipedia.org/wiki/Standard_streams) 

> 在计算机程序设计中，当计算机程序开始执行时，标准流是计算机程序与其环境之间互连的输入和输出通信通道。三个输入/输出(I/O)连接称为标准输入(stdin)、标准输出(stdout)和标准错误(stderr)。

从上面的引用中，我们收集到该流程有三个流，一个流入 stdin，两个流出 stdout 和 stderr。

这里我们使用管道方法，它类似于我们在 shell 中使用的` | `操作符。我们使用管道将 process.stdin 的输出发送到 process.stdout。

*默认情况下，stdin 将通过键盘从命令行获取数据。stdout 也将输出到命令行。继续输入一些数据:*

```
"use strict";
process.stdin.pipe(process.stdout)
```

类型:你好[return]你是谁[return]

# 输出:

```
hello
hello
Who are you
Who are you
```

现在让我们用管道将数据从 stderr 中导出。

```
"use strict";
process.stdin.pipe(process.stderr)
```

类型:错误消息[return]错误消息* [return]

# 输出:

```
error messafge
error messafge
error message*
error message*
```

当从 Readable stream 类继承的流接收数据时，它会发出一个事件“data”。

当一个流显示数据时，它通常默认显示为一个“缓冲区”实例。Node.js 通过缓冲区来处理二进制数据。

这意味着我们必须调用 Buffer.prototype.toString()将数据转换成可工作的字符串。

```
"use strict";
// The stdin stream is listening for 'data' using the on methodprocess.stdin.on('data', (data) => {
  console.log("hello from the process Object")
  console.log(data);
  console.log(`Thanks for the data: ${data.toString().toUpperCase()}`);
})
```

第一行是我打的。所有其他输出行都是从数据事件创建的，该事件是在我通过键盘将数据写入 stdin 流时触发的。

# 输出:

```
Hello, take this data // typed
hello from the process Object
<Buffer 48 65 6c 6c 6f 2c 20 74 61 6b 65 20 74 68 69 73 20 64 61 74 61 0a>
Thanks for the data: HELLO, TAKE THIS DATA
```

还可以使用 Streams 模块创建流。

 [## Node.js v15.3.0 文档

### 源代码:lib/stream.js 流是一个抽象接口，用于处理 Node.js 中的流数据

nodejs.org](https://nodejs.org/dist/latest-v14.x/docs/api/stream.html) 

感谢阅读。下次见！

*更多内容尽在*[plain English . io](http://plainenglish.io/)