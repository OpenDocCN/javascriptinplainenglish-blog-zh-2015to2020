# 具有即插即用功能的 JavaScript 和 Node.js 插件架构

> 原文：<https://javascript.plainenglish.io/plugin-architecture-in-javascript-and-node-js-with-plug-and-play-6ac0d410db0?source=collection_archive---------10----------------------->

![](img/6e4155f96a2efcc246c7e9c1f0744ac2.png)

[即插即用](https://github.com/adaltas/node-plug-and-play)帮助库和应用程序作者在他们的代码中引入插件架构。它通过定义明确的拦截点(也称为挂钩)简化了复杂的代码执行，并为库用户提供了扩展、修复和更改代码执行流的能力，而无需更改代码。

这个库刚刚发布，并在麻省理工学院的许可下在 NPM 上[出版。作为其他库的一部分，它已经开放了一段时间。第一个实现是为](https://www.npmjs.com/package/plug-and-play)[节点参数](https://github.com/adaltas/node-parameters/)创建的，这是一个高级 CLI 参数库。它后来被改进为 [Nikita](https://nikita.js.org) ，一个部署自动化工具，最后被隔离到即插即用包中。

插件架构实现了多种目的。当钩子被仔细选择和设置时，应用程序和库用户可以扩展和调整库来满足他们的需要。但不仅仅是用户，当代码变得复杂时，原作者也会从插件中受益。

复杂的代码可以分解成多个组件。它简化了开发、调试和测试。例如，Node.js 的部署自动化工具 [Nikita](https://nikita.js.org) 的核心过去是基于一个 600 行的模块，它之所以能工作是因为它有 300 多个测试的支持。引入新功能很复杂，如果不是不可能的话。微妙和已知的错误仍然存在。在其最新的重构中，相同的核心模块大约有 140 行，一旦去掉即插即用总结代码，就只有 100 行。即使是代码的原作者，现在也有可能清楚地理解代码。旧的令人讨厌的错误被删除(新的错误可能已经被引入:)。一堆新功能出现了。更重要的是，当新想法出现时，开发人员的体验从噩梦变成了许多乐趣。

这是一个教程，让您开始使用即插即用的主要功能。这并不复杂，我们将慢慢地学习下面的用法。

*   访问和修改挂钩参数
*   修改处理程序行为
*   在处理程序执行后插入代码
*   订购挂钩并要求依赖关系
*   通过处理程序链传递结果
*   异步钩子处理程序
*   嵌套/分层架构

# 1.没有任何插件的简单应用程序

模块是一个基本的 web 服务器代码。它由核心库组成，并且可以通过插件进行扩展。

```
const http = require('http')

module.exports = (config = {}) => {
  if(!config.mesage){
    config.message = 'hello'
  }
  const server = http.createServer((req, res) => {
    res.end(config.message)
  })
  return {
    start: () => {
      server.listen(config.port)
    },
    stop: () => {
      server.close()
    }
  }
}
```

该模块由一个初始函数组成，该函数接受一个配置对象`config`，并返回两个函数`start`和`stop`。

`sample/1/index.js`模块是一个 cli 界面，提示用户输入`start`和`stop`命令。

```
const readline = require('readline')
const app = require('./app')()

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});
rl.prompt();
rl.on('line', (line) => {
  switch (line.trim()) {
    case 'start':
      app.start()
      break;
    case 'stop':
      app.stop()
      break;
    default:
      process.stdout.write('Only `start` and `stop` are supported\n')
      break;
  }
  rl.prompt();
})
```

要执行这个示例，克隆[教程库](https://github.com/adaltas/node-plug-and-play-tutorial)。从项目根目录，运行`node tutorial/1`。在 shell 提示符下，输入`start`，然后输入`stop`。它将在 Node.js 选择的某个随机端口上启动和停止 web 服务器。您可以修改代码来打印由`server.address().port`返回的值，或者使用`netcat` Linux 命令来找出是哪个端口。

# 2.访问和修改挂钩参数

让我们用一个新的插件架构来增强我们的应用程序。我们的第一个插件将修改配置对象，将监听端口的默认值设为`3000`。在注册新插件之前，我们应该更新我们的原始库，即`app.js`文件。

在`sample/2/app.js`模块中，我们导入即插即用模块并初始化它。创建的名为`plugins`的实例被返回并公开，这样用户可以通过调用`plugins.register`函数来注册新的钩子。

然后我们定义了第一个名为`server:start`的钩子。它的作用是为用户提供拦截`start`命令的机会。`handler`函数提供了默认实现。`handler`函数的第一个参数在`args`中定义。我们通过`config`属性传递配置。

用户现在有能力修改、丰富、改变`handler`函数及其参数。在我们的例子中，我们可以访问配置来设置默认的`config.port`值。

```
const http = require('http')
// Import the Plug and Play module
const plugandplay = require('plug-and-play')

module.exports = (config = {}) => {
  // Initialize our plugin architecture
  const plugins = plugandplay()
  const server = http.createServer((req, res) => {
    res.end(config.message)
  })
  return {
    // Return and expose the plugin instance
    plugins: plugins,
    start: () => {
      // Defined the `start` hook
      plugins.call({
        name: 'server:start',
        args: {
          config: config
        },
        handler: ({config}) => {
          server.listen(config.port)
        }
      })
    },
    stop: () => {
      server.close()
    }
  }
}
```

通过调用`register`创建一个新的插件。为了有效，一个插件必须拦截至少一个钩子并插入它的逻辑。

挂钩名称与用户函数相关联。它也可以是具有与函数相关联的`handler`属性的对象。当只定义了一个参数时，它将在原始钩子处理程序之前执行。我们将在后面看到如何通过声明第二个参数来访问原始处理程序以改变其执行或在执行后注入逻辑。

当一个新的钩子被注册时，用户函数接收与在`args`中定义的相同的参数。在我们的例子中，我们可以访问`config`的财产。`sample/2/index.js`模块注册并实现我们的第一个钩子来修改配置。

```
const readline = require('readline')
const myapp = require('./app')()

myapp.plugins.register({
  hooks: {
    'server:start': ({config}) => {
      // Set the default port value
      if( !config.port ){
        config.port = 3000
      }
    }
  }
})

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});
rl.prompt();
rl.on('line', (line) => {
  switch (line.trim()) {
    case 'start':
      myapp.start()
      break;
    case 'stop':
      myapp.stop()
      break;
    default:
      process.stdout.write('Only `start` and `stop` are supported\n')
      break;
  }
  rl.prompt();
})
```

# 3.修改处理程序行为

尝试输入`start`两次将导致一个不太友好的错误，如下所示:

```
> start
> start
readline.js:1150
            throw err;
            ^

Error [ERR_SERVER_ALREADY_LISTEN]: Listen method has been called more than once without closing.
    at Server.listen (net.js:1399:11)
    at Object.start (/home/david/plug-and-play/sample/app.js:10:14)
    at Interface.<anonymous> (/home/david/plug-and-play/sample/index.js:14:11)
    at Interface.emit (events.js:315:20)
    at Interface._onLine (readline.js:337:10)
    at Interface._line (readline.js:666:8)
    at Interface._ttyWrite (readline.js:1006:14)
    at ReadStream.onkeypress (readline.js:213:10)
    at ReadStream.emit (events.js:315:20)
    at emitKeys (internal/readline/utils.js:335:14) {
  code: 'ERR_SERVER_ALREADY_LISTEN'
}
```

web 服务器已经在监听端口`3000`，新的服务器无法启动。更有趣的是，如果已经启动，通过不调用它的`server.listen`函数来避免启动 HTTP 服务器两次。为此，我们将在`sample/3/app.js`模块中公开`server`实例参数:

```
const http = require('http')
const plugandplay = require('plug-and-play')

module.exports = (config = {}) => {
  const plugins = plugandplay()
  const server = http.createServer((req, res) => {
    res.end(config.message)
  })
  return {
    plugins: plugins,
    start: () => {
      plugins.call({
        name: 'server:start',
        // Expose the server argument
        args: {
          config: config,
          server: server
        },
        // Grab the server argument
        handler: ({config, server}) => {
          server.listen(config.port)
        }
      })
    },
    stop: () => {
      server.close()
    }
  }
}
```

我们的用户钩子现在可以访问`server.listening`属性，以了解服务器是否启动并运行。根据它的值，我们将关闭原始钩子处理程序的执行。

在我们的处理程序中，我们声明了第二个参数，即之前调用的处理程序，它也是原始的处理程序。钩子可以通过提供另一个实现或`null`来选择覆盖或绕过处理程序。在我们的例子中，当服务器已经在监听时，我们返回`null`。另一种类似的方法是返回一个空函数`function(){}`。

```
const readline = require('readline')
const myapp = require('./app')()

myapp.plugins.register({
  hooks: {
    // Declare the `handler` second argument
    'server:start': ({config, server}, handler) => {
      if( !config.port ){
        config.port = 3000
      }
      // Return null when the server is already listening
      return server.listening ? null : handler
    }
  }
})

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});
rl.prompt();
rl.on('line', (line) => {
  switch (line.trim()) {
    case 'start':
      myapp.start()
      break;
    case 'stop':
      myapp.stop()
      break;
    default:
      process.stdout.write('Only `start` and `stop` are supported\n')
      break;
  }
  rl.prompt();
})
```

# 4.在处理程序执行后插入代码

一旦服务器启动，打印一条消息将是一个很好的补充。

一个新的插件将为此而创建。它向用户报告有关服务器生命周期的信息。它就在先前创建的那个之后注册。

挂钩插入`server:start`拦截点。它返回一个新的处理函数，从而替换原来的函数。它首先调用原始处理程序，打印一条消息，并返回原始处理程序返回的任何内容。在这种情况下，我们的新处理程序的行为就像旧的一样，它只是在服务器启动后打印一条消息。

```
const readline = require('readline')
const myapp = require('./app')()

myapp.plugins.register({
  hooks: {
    'server:start': ({config, server}, handler) => {
      if( !config.port ){
        config.port = 3000
      }
      return server.listening ? null : handler
    }
  }
})

myapp.plugins.register({
  hooks: {
    'server:start': {
      handler: (args, handler) => {
        // Return a new handler function
        return () => {
          // Call the original handler
          const info = handler.call(null, args)
          // Print a message
          process.stdout.write('Server is started\n')
          // Return whatever the original handler was returning
          return info
        }
      }
    }
  }
})

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});
rl.prompt();
rl.on('line', (line) => {
  switch (line.trim()) {
    case 'start':
      myapp.start()
      break;
    case 'stop':
      myapp.stop()
      break;
    default:
      process.stdout.write('Only `start` and `stop` are supported\n')
      break;
  }
  rl.prompt();
})
```

# 5.订购挂钩并要求依赖关系

运行两次`start`命令将导致错误:

```
[whoami@localhost] > start
[whoami@localhost] > Server is started
                     start
[whoami@localhost] > (node:1358) UnhandledPromiseRejectionWarning: TypeError: Cannot read property 'call' of null
                     at Object.<anonymous> (/home/david/plug-and-play/sample/4/index.js:21:34)
                     at Object.hook (/home/david/plug-and-play/lib/index.js:200:24)
                     at processTicksAndRejections (internal/process/task_queues.js:97:5)
                     (node:1358) UnhandledPromiseRejectionWarning: Unhandled promise rejection. This error originated either by throwing inside of an async function without a catch block, or by rejecting a promise which was not handled with .catch(). To terminate the node process on unhandled promise rejection, use the CLI flag `--unhandled-rejections=strict` (see https://nodejs.org/api/cli.html#cli_unhandled_rejections_mode). (rejection id: 1)
                     (node:1358) [DEP0018] DeprecationWarning: Unhandled promise rejections are deprecated. In the future, promise rejections that are not handled will terminate the Node.js process with a non-zero exit code.
```

在第二次调用中，`handler`是`null`而不是一个函数，因此抛出了上面的错误。这是预料之中的，因为我们的第一个插件返回`null`如果服务器已经启动。

一种解决方案是检查`handler`值，当它等于`null`时，打印一条类似“服务器已经在运行”的消息。为了说明钩子排序，我们选择了另一种方法。我们将确保第二个钩子总是在第一个钩子之前执行。

让我们给我们的插件取个名字。第一个名为`plugin:enhancer`，第二个名为`plugin:reporter`。`server:start`钩子不再是一个处理函数。相反，它是一个包含处理程序和指向第一个插件`plugin:enhancer`的`before`属性的对象。这样，`plugin:reporter`的`server:start`钩子将总是在`plugin:enhancer`钩子之前执行，并且`handler`参数将永远不会是`null`。

注意，`before`和`after`属性为多个插件定义了钩子之间的执行顺序。他们不要求插件存在。默认情况下，依赖项是可选的。要定义一个必需的依赖项，您必须在 plugin `required`属性中以字符串或字符串数组的形式列出它的名称，以表示插件名称。

```
const readline = require('readline')
const myapp = require('./app')()

myapp.plugins.register({  name: 'plugin:enhancer',  hooks: {    'server:start': ({config, server}, handler) => {      if( !config.port ){        config.port = 3000      }      return server.listening ? null : handler    }  }})
myapp.plugins.register({  name: 'plugin:reporter',  required: 'plugin:enhancer',  hooks: {    'server:start': {      before: 'plugin:enhancer',      handler: (args, handler) => {        return () => {          const info = handler.call(null, args)          process.stdout.write('Server is started\n')          return info        }      }    }  }})
const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});
rl.prompt();
rl.on('line', (line) => {
  switch (line.trim()) {
    case 'start':
      myapp.start()
      break;
    case 'stop':
      myapp.stop()
      break;
    default:
      process.stdout.write('Only `start` and `stop` are supported\n')
      break;
  }
  rl.prompt();
})
```

# 6.通过处理程序链传递结果

当插件注册新的钩子时，以原始处理程序开始的几个处理程序被一个接一个地调用。序列顺序基于插件注册以及钩子`before`和`after`属性。

当实现钩子处理程序时，库作者得到执行链返回的结果。`plugins.call`函数返回最后执行的处理程序的值。库作者可以在函数中自由使用和返回值。在`sample/6/app.js`模块中，处理程序返回端口号。该端口用于丰富 info 对象，该对象在`start`函数结束时返回给最终用户。

```
const http = require('http')
const plugandplay = require('plug-and-play')

module.exports = (config = {}) => {
  const plugins = plugandplay()
  const server = http.createServer((req, res) => {
    res.end(config.message)
  })
  return {
    plugins: plugins,
    start: async () => {
      const info = {
        time: new Date()
      }
      // Get the result returned by the last called handler
      info.port = await plugins.call({
        name: 'server:start',
        args: {
          config: config,
          server: server
        },
        // Handler returning the port number
        handler: ({config, server}) => {
          server.listen(config.port)
          return server.address().port
        }
      })
      return info
    },
    stop: () => {
      server.close()
    }
  }
}
```

在`sample/6/index.js`模块中，`info`对象用于打印端口号。

```
const readline = require('readline')
const myapp = require('./app')()

myapp.plugins.register({
  name: 'plugin:enhancer',
  hooks: {
    'server:start': ({config, server}, handler) => {
      if( !config.port ){
        config.port = 3000
      }
      return server.listening ? null : handler
    }
  }
})

myapp.plugins.register({
  name: 'plugin:reporter',
  required: 'plugin:enhancer',
  hooks: {
    'server:start': {
      before: 'plugin:enhancer',
      handler: (args, handler) => {
        return () => {
          const info = handler.call(null, args)
          process.stdout.write('Server is started\n')
          return info
        }
      }
    }
  }
})

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});
rl.prompt();
rl.on('line', async (line) => {
  switch (line.trim()) {
    case 'start':
      // Get the info object and print the port number
      const info = await myapp.start()
      process.stdout.write(`Port is ${info.port}\n`)
      break;
    case 'stop':
      myapp.stop()
      break;
    default:
      process.stdout.write('Only `start` and `stop` are supported\n')
      break;
  }
  rl.prompt();
})
```

注意，值总是作为承诺返回。任何其他类型的价值都包含在承诺中。

# 7.异步钩子处理程序

上面的代码有点问题。该消息通知用户服务器已经启动，但并不完全正确。启动函数是一个异步函数。在打印消息时，服务器还没有启动并准备好处理请求。

异步处理程序必须返回并解析承诺。

`sample/7/app.js`模块解决了这个问题。它的处理程序现在返回一个承诺。在返回之前，`start`函数本身也必须等待承诺解决。注意，`setTimeout`函数只出现在代码中，用来模拟缓慢的启动时间。

```
const http = require('http')
const plugandplay = require('plug-and-play')

module.exports = (config = {}) => {
  const plugins = plugandplay()
  const server = http.createServer((req, res) => {
    res.end(config.message)
  })
  return {
    plugins: plugins,
    // Return a promise
    start: async () => {
      const info = {
        time: new Date()
      }
      // Wait for the promise to resolve
      info.port = await plugins.call({
        name: 'server:start',
        args: {
          config: config,
          server: server
        },
        handler: ({config, server}) => {
          // Return a Promise
          return new Promise( (accept, reject) => {
            server.listen({port: config.port}, (err) => {
              // Simulate a perceptible delay
              setTimeout( () => {
                err ? reject(err) : accept(server.address().port)
              }, 1000)
            })
          })
        }
      })
      return info
    },
    stop: () => {
      server.close()
    }
  }
}
```

插件作者还必须尊重处理程序的异步特性，并像在`sample/7/index.js`模块中一样处理它们的返回值。

```
const readline = require('readline')
const myapp = require('./app')()

myapp.plugins.register({
  name: 'plugin:enhancer',
  hooks: {
    'server:start': ({config, server}, handler) => {
      if( !config.port ){
        config.port = 3000
      }
      return server.listening ? null : handler
    }
  }
})

myapp.plugins.register({
  name: 'plugin:reporter',
  required: 'plugin:enhancer',
  hooks: {
    'server:start': {
      before: 'plugin:enhancer',
      handler: (args, handler) => {
        // Return a promise
        return async () => {
          // Obtain a promise
          const info = await handler.call(null, args)
          process.stdout.write('Server is started\n')
          return info
        }
      }
    }
  }
})

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});
rl.prompt();
rl.on('line', async (line) => {
  switch (line.trim()) {
    case 'start':
      const info = await myapp.start()
      process.stdout.write(`Port is ${info.port}\n`)
      break;
    case 'stop':
      myapp.stop()
      break;
    default:
      process.stdout.write('Only `start` and `stop` are supported\n')
      break;
  }
  rl.prompt();
})
```

# 8.嵌套/分层架构

插件实例可以嵌套，这意味着一个拦截点可以调用当前实例和父实例中注册的钩子。

[Nikita 项目](https://nikita.js.org)在其架构内部充分利用了这一特性。Nikita 是一个[基础设施代码(IaC)工具](https://www.adaltas.com/en/tag/iac/)，用于自动化部署。它由嵌套的动作组成。父动作可以定义安装组件的过程，比如安装 MariaDB。这个父动作由多个子动作组成，比如安装 YUM 或 APT 包、维护配置文件以及上传 SSL 证书。同样，每个动作都由多个子动作组成。配置操作写入一个文件，但也检查其权限和所有权。package 操作根据当前的 Linux 发行版执行 shell 命令来安装软件包。在任何级别，Nikita 都为用户提供了注册插件的机会。一旦注册，插件对每个子动作都是可用和有效的。例如，`debug`插件将日志信息打印到 stdout。注册并激活后，它将对一个动作及其子动作有效。

嵌套架构的另一个常见用途是为用户提供在实例级和全局级注册插件的可能性。在全局级别注册一个插件使得它对每个实例都可用。

`sample/8/app.js`实现了最新的模式。主即插即用模块全局导出一个新的`plugins`实例。这个全局实例被传递给每个创建的本地即插即用实例。父实例在初始化时通过`parent`属性暴露给它们的子实例。

```
const http = require('http')
const plugandplay = require('plug-and-play')

module.exports = (config = {}) => {
  const plugins = plugandplay({
    // Pass the global plugins instance
    parent: module.exports.plugins
  })
  const server = http.createServer((req, res) => {
    res.end(config.message)
  })
  return {
    plugins: plugins,
    start: async () => {
      const info = {
        time: new Date()
      }
      info.port = await plugins.call({
        name: 'server:start',
        args: {
          config: config,
          server: server
        },
        handler: ({config, server}) => {
          return new Promise( (accept, reject) => {
            server.listen({port: config.port}, (err) => {
              setTimeout( () => {
                err ? reject(err) : accept(server.address().port)
              }, 1000)
            })
          })
        }
      })
      return info
    },
    stop: () => {
      server.close()
    }
  }
}

// Export global plugins
module.exports.plugins = plugandplay()
```

`sample/8/index.js`模块说明了全球和本地注册。`plugin:enhancer`是全球注册的，而`plugin:reporter`是本地注册的。

```
const readline = require('readline')
const app = require('./app')
// Global registration
app.plugins.register({
  name: 'plugin:enhancer',
  hooks: {
    'server:start': ({config, server}, handler) => {
      if( !config.port ){
        config.port = 3000
      }
      return server.listening ? null : handler
    }
  }
})

// Local registration
const myapp = app()
myapp.plugins.register({
  name: 'plugin:reporter',
  required: 'plugin:enhancer',
  hooks: {
    'server:start': {
      before: 'plugin:enhancer',
      handler: (args, handler) => {
        return async () => {
          const info = await handler.call(null, args)
          process.stdout.write('Server is started\n')
          return info
        }
      }
    }
  }
})

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});
rl.prompt();
rl.on('line', async (line) => {
  switch (line.trim()) {
    case 'start':
      const info = await myapp.start()
      process.stdout.write(`Port is ${info.port}\n`)
      break;
    case 'stop':
      myapp.stop()
      break;
    default:
      process.stdout.write('Only `start` and `stop` are supported\n')
      break;
  }
  rl.prompt();
})
```

# 结论

我们使用即插即用和它的祖先已经有几年了，它被证明是有帮助的。它为我们的库和应用程序打开了许多潜力，包括可扩展性和可组合性。我们希望它能很好地为你服务，并欢迎新的[功能和修复](https://github.com/adaltas/node-plug-and-play/issues)。

*原载于 2020 年 8 月 28 日 https://www.adaltas.com**的* [*。*](https://www.adaltas.com/en/2020/08/28/node-js-plugin-architecture/)

*更多内容请看* [***说白了就是***](https://plainenglish.io/) *。*

*报名参加我们的* [***免费每周简讯***](http://newsletter.plainenglish.io/) *。关注我们关于* [***推特***](https://twitter.com/inPlainEngHQ) ，[***LinkedIn***](https://www.linkedin.com/company/inplainenglish/)*，*[***YouTube***](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)*，以及* [***不和***](https://discord.gg/GtDtUAvyhW)*****。*****

***有兴趣缩放你的软件启动*** *？检查出* [***电路***](https://circuit.ooo?utm=publication-post-cta) *。*