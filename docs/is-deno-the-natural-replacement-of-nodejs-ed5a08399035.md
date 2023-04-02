# Deno 是 Node 的自然替代品吗？JS？

> 原文：<https://javascript.plainenglish.io/is-deno-the-natural-replacement-of-nodejs-ed5a08399035?source=collection_archive---------2----------------------->

## JavaScript 和 TypeScript 的安全运行时

![](img/3365011db510fbbaf44d13e5728a38ce.png)

Image designed by Kevin Qian based on Ryan Dahl’s sketch.

## 一、Deno 是什么？

Deno 是一个简单的现代安全的 JavaScript 和 TypeScript 运行时，它使用 V8 并内置于 Rust 中

Deno 由 Ryan Dahl 在 2018 年创建(他是 NodeJs 在 2009 年的创建者)，最近发布了 1.0 版本，作为 NodeJs 的演进。Deno 是用 [V8](https://v8.dev/) 、 [Rust](https://www.rust-lang.org/) 和 [Tokio](https://docs.rs/tokio/0.2.21/tokio/) 打造的，侧重于安全性或易用性等方面。

## 它是 NodeJS 的好替代品吗？

简而言之，虽然 Deno 是 JavaScript 的一个很好的运行时环境，但是 NodeJs 是一个很大的、成熟的技术，将会存在几十年。尽管如此，Deno 很快会成为 NodeJs 主要问题的一个很好的替代方案:

*   必须支持大量遗留 API。
*   缺乏安全感。默认情况下，在 NodeJs 中，您可以访问文件系统或环境。
*   一个设计很差的模块系统，有集中的注册表。

## Deno 的顶级特性

## 安全性

Deno 默认情况下在沙箱中执行代码，因此在运行时默认情况下没有对文件系统或环境的访问权限，除非您显式启用它。

相反，您可以使用命令行参数或以编程方式启用或禁用不同的安全功能。

例如，如果您要求脚本从/assets 文件夹中读取，您可以执行以下命令:

```
//--allow-read
deno --allow-read=/assets yourScript.js
```

这将允许您的代码读取文件夹。

现在，如果您的脚本需要使用网络，例如，在 8090 端口发出获取请求，您可以使用以下标志:

```
//--allow-netdeno yourScript.js --allow-net=:8090
```

可用的标志有:

```
--allow-all (similar behavior to nodeJS)--allow-hrtime
--allow-read=<whitelist>
--allow-write=<whitelist>
--allow-plugin
--allow-env
--allow-net=<whitelist>
--allow-run
```

## 支持现成的类型脚本

Deno 是用 TypeScript 编写的，你可以在你的代码中直接使用 TypeScript，而不需要任何 transpiling 步骤。另一方面，如果您愿意，也可以选择用普通 javascript 编写。

## ES 模块

Deno 支持使用本地或远程 URL 的 ES 模块，使得依赖关系管理非常简单。Deno 中的包分发不需要像 NPM 那样的集中注册中心。

如果需要，Npm 仍然可以用作简单的本地文件 URL。

NodeJs 的其他差异包括:

*   不支持 require()。相反，它使用 es 模块标准语法(import from …)。
*   Deno 下载所有的模块并缓存它们。一旦它们被缓存，Deno 将不会再次下载它们，直到您用 reload 标志明确请求它们。
*   Deno 认为远程代码是不可变的。如果你想修改它，你必须使用<reload flag="">。</reload>
*   没有 node_module 文件夹。

## 浏览器兼容性

由于 Deno 对 ES 模块的本地支持，您不必使用其他构建工具，如 [WebPack](https://webpack.js.org/) 或 [Parcel](https://parceljs.org/) 来使我们的应用程序准备好在浏览器中使用。

## 标准模块

Deno 提供了一个没有外部依赖的模块列表，Deno 核心团队审查这些模块。这些模块托管在 deno.land/std[的](https://deno.land/std)，并像所有其他 es 模块一样分发。

## 其他人

Deno 在其 API 和库 Node.js 中使用最新的 ECMAScript 特性，相反，它使用一个旧的基于回调的库，并且不打算更新它。

如果您想尝试 Deno，安装过程很简单:

使用 Shell (Linux):

```
$curl -fsSL [https://deno.land/x/install/install.sh](https://deno.land/x/install/install.sh) | sh
```

使用 PowerShell(Windows):

```
iwr [https://deno.land/x/install/install.ps1](https://deno.land/x/install/install.ps1) -useb -outf install.ps1; .\install.ps1 v0.38.0
```

使用包管理器

带巧克力的(Windows):

```
choco install deno
```

检查其安装是否正确:

```
//In ubuntu 20.04:/home/xxx/.deno/bin/deno --help//In Windows 10:deno --help
```

使用 Deno 运行脚本:

创建 hello.js 文件(如果是 TypeScript，则创建 hello.ts 文件):

```
console.log("Hello world!");
```

运行脚本:

```
//In ubuntu 20.04:
/home/xxx/.deno/bin/deno run hello.js
//Hello world!//In Windows 10:
deno run hello.js
//Hello world!
```

Deno 提供了一个安装选项来快速安装和共享可执行代码。此命令生成一个可执行的 shell 脚本，该脚本使用指定的 CLI 标志和主模块调用 deno。它位于安装根目录的 bin 目录中:

```
deno install hello.js
```

✅ *成功安装你好*

*/home/xxx/。deno/bin/hello*

进入 deno 执行环境(类似 nodeJs):

```
./deno
>console.log("Hello world!");
Hello world!
```

## 结论

学习像 Deno 这样的新技术需要很大的努力。我的建议是，如果您现在从服务器端 JS 开始，并且您还不知道 Node，并且从未使用 TypeScript 编写过，请从 Node 开始。

尽管 Deno 对于我们的程序来说是一个优秀的运行时环境，但它仍处于早期阶段。Node.js 仍然是一个巨大的、得到很好支持的、成熟的网站，但是在未来，Deno 可能是一个很好的选择。

非常感谢你阅读这篇文章！希望你觉得有用。

## 参考资料:

[https://deno.land/](https://deno.land/)

下面是 Deno 演示的视频:

[https://youtu.be/M3BM9TB-8yA](https://youtu.be/M3BM9TB-8yA)

## **简明英语团队的笔记**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**我们还推出了一个 YouTube，希望你能通过 [**订阅我们的简明英语频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) 来支持我们**

**一如既往,“简明英语”希望帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****