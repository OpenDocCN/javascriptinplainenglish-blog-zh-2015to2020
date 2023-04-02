# 将德诺杀死节点。未来几天的 JS？

> 原文：<https://javascript.plainenglish.io/will-deno-kill-node-js-in-the-coming-days-df6f6ebf494?source=collection_archive---------6----------------------->

## Deno 为开发者带来的关键东西

![](img/06a0c4a01022ae3382f5566c22b7c54e.png)

Photo by [Jimmy Chang](https://unsplash.com/@photohunter?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/dagger?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Node.js，十多年来开发人员非常熟悉的东西。在未来的日子里，会不会有一个叫 Deno 的东西取代这个超级流行的运行时？

**注:Node.js 和 Deno 都是同一个父亲瑞安·达尔的孩子。**

他在这里与 Nodejs [谈论他的错误](https://www.youtube.com/watch?v=M3BM9TB-8yA&vl=en)并且在这里更深入地研究了 deno

对于开发者来说，Deno 是一个非常新的东西。作为介绍

> Deno 是一个简单、现代和安全的 JavaScript 和 TypeScript 运行时，它使用 V8 并内置于 Rust 中。

![](img/4afa21d0271f2d79ba3be77ef8790abb.png)

Deno logo looks like this.

从头开始构建

*   **V8 as node.js**
*   **Rust** 而不是 C++中的 node
*   **Tokio(事件循环)**代替 libuv
*   **打字稿**代替 vanila JS

我在这里注意到了这个新运行时的**安装**和一些最有趣的**方面**。

# 装置

**使用自制软件(macOS):**

```
brew install deno
```

**使用 Cargo (Windows、macOS、Linux):**

```
cargo install deno
```

# 玻恩打字稿

Deno 默认支持 typescript。不需要复杂的设置或配置。

> 安息吧 tsconfig.json

内置对类型脚本的支持，即你可以直接运行类型脚本文件而无需编译成 javascript。

Deno 使得使用 TypeScript 变得容易，不需要任何配置文件。尽管如此，用普通的 JavaScript 编写程序并毫无困难地用 Deno 执行它们是可能的。

# 默认安全

> 默认安全。除非明确启用，否则没有文件、网络或环境访问权限。

德诺是天生安全的。您必须明确指定安全性，如网络、环境或任何类似的东西。

与节点不同，deno 在主机的沙盒中运行，即

*   没有文件系统访问权限
*   没有网络接入
*   无法访问其他脚本
*   没有环境变量访问

因此，从运行时到网络、文件系统等都无法访问。当您的代码试图访问这些资源时，系统会提示您允许该操作。

让我们试一个例子

FirstDeno.ts

现在运行文件的方式:

```
deno run FirstDeno.ts
```

现在，安全屏障开始发挥作用

```
**error**: Uncaught PermissionDenied: network access to "0.0.0.0:8000", run again with the --allow-net flag
at ***unwrapResponse*** ($deno$/ops/dispatch_json.ts:43:11)
at ***Object.sendSync*** ($deno$/ops/dispatch_json.ts:72:10)
at ***Object.listen*** ($deno$/ops/net.ts:51:10)
at ***listen*** ($deno$/net.ts:152:22)
at ***serve*** (https://deno.land/std@0.50.0/http/server.ts:261:20)
at file:///Users/tunvirrahman/Downloads/FirstDeno.ts:2:11
```

因为 Deno 在默认情况下是安全的，您必须显式地允许网络

```
deno run — allow-net FirstDeno.ts
```

以下是可提供的安全选项

```
--allow-all # Allow all permissions--allow-env # Allow environment access--allow-hrtime # Allow high resolution time measurement--allow-net=<allow-net> # Allow network access--allow-plugin # Allow loading plugins--allow-read=<allow-read> # Allow file system read access--allow-run # Allow running subprocesses--allow-write=<allow-write> # Allow file system write access
```

# RIP 包管理器

Deno 不需要任何包管理器或者 **package.json** 。相反，它使用了如下现代 ES 语法

> 没有**更没有** `**package.json**` **和** `**node_modules**`

```
import { serve } from “https://deno.land/std@0.50.0/http/server.ts";
```

当我们启动应用程序时，Deno 下载所有导入的模块并缓存它们。一旦它们被缓存，Deno 将不会再次下载它们，直到我们用`--reload`标志明确要求它。

Deno 不支持 npm 包。这令人担忧，对吗？

相反，它有一个类似于 Go 的模块管理系统，通过 URL 导入模块

# 承诺以承诺为基础

它支持顶级等待。一切异步都是基于 Deno 的承诺。

> 拜拜回电地狱

```
const res = await fetch("https://someUrl")
```

这并不要求包含方法是异步的。

# Deno 标准模块

这些模块没有外部依赖性，它们由 Deno 核心团队审查。目的是有一套标准的高质量代码，所有 Deno 项目都可以大胆地使用。

要浏览模块的文档:

*   去[https://deno.land/std/](https://deno.land/std/)。
*   导航到任何感兴趣的模块。
*   点击“查看文档”。

# 浏览器兼容性

Deno 的目标是兼容浏览器。从技术上讲，当使用 ES 模块时，我们不必使用任何像 webpack 这样的构建工具来使我们的应用程序准备好在浏览器中使用。

然而，像 Babel 这样的工具会将代码转换到 JavaScript 的 ES5 版本，因此，代码甚至可以在不支持该语言所有最新特性的旧浏览器中运行。但这也是以在最终文件中包含大量不必要的代码和输出文件膨胀为代价的。

由我们来决定我们的主要目标是什么，并做出相应的选择。

# 结论

> Node.js 的伙计们。不要惊慌。Node 哪儿也不去！

如果您正在从头开始编写软件，Deno 可能值得研究。目前它是为爱好者准备的。

# 感谢阅读。干杯🥂

## **简明英语团队的笔记**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页 [**plainenglish.io**](https://plainenglish.io/) 找到所有这些内容——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**