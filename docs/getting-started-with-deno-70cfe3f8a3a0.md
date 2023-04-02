# Deno 入门

> 原文：<https://javascript.plainenglish.io/getting-started-with-deno-70cfe3f8a3a0?source=collection_archive---------7----------------------->

![](img/2475da820e0d66605582f1b6a0e62eff.png)

TLDR: 未来几十年，Deno 可能会取代 Node。所以十年后再回来。不然，我们还是第一吧！

# 什么是德诺？

Deno 是一个带有安全默认值的 JavaScript(和 TypeScript)运行时。和 Node.js 一样，也是基于 V8 构建的。

# 为什么是德诺？

*   Deno 在默认情况下是安全的。与节点不同，您必须指定系统访问，如文件、网络、环境访问等。
*   它支持现成的 TypeScript。因此，您不需要单独安装 TypeScript 编译器。
*   还有很多。最后但同样重要的是，它是由十年前创建 Node.js 的同一个人 Ryan Dahl 创建的。因此，如果他决定创造一个新的，这将意味着……好吧，你自己弄清楚了。如果你有 3000 万的空闲时间，看看他的[演讲](https://www.youtube.com/watch?v=M3BM9TB-8yA)关于他对 Node.js 后悔的 10 件事

# 装置

既然你信服了，那我们就打开一个终端，安装那个东西吧。您可以通过多种方式安装，请查阅此官方[文件](https://deno.land/manual/getting_started/installation)。

如果你在 macOS(或 Linux)上，运行这个`**curl-fsSL**[**https://deno.land/x/install/install.sh**](https://deno.land/x/install/install.sh)**| sh**`和运行这个`**iwr**[**https://deno.land/x/install/install.ps1**](https://deno.land/x/install/install.ps1)**-useb | iex**`如果你在 Windows 上。

# 配置 VS 代码

我喜欢 VS 代码。所以如果你在同一个团队，安装这个[插件](https://marketplace.visualstudio.com/items?itemName=axetroy.vscode-deno)来获得 Deno 完整的智能感知支持和更多。

> 请在安装后重新启动或重新加载 IDE。

# 你的第一个程序

让我们创建您著名的 HelloWorld 应用程序。你可能已经猜到了，创建` **index.ts** `并编写`**console . log(' hello Deno ')；**

要运行它，请在您的终端上运行` **Deno run index.ts** `命令

> 编译文件:///Users/ABC/side-projects/deno-app/index . ts
> 
> 你好德诺

好吧，我们再跑一次

> 你好德诺

如果你注意到在第二次输出中，没有`**编译…** `行，是吗？嗯，是因为 Deno 检测到你运行的是同一个源代码，而且已经编译过一次了。Deno 刚刚执行了缓存中的二进制文件。好吧，如果你怀疑，编辑文件并再次运行。

# 沙盒安全性

据说 Deno 默认是安全的。我们来测试一下。打开一个新的终端，运行这个命令`**deno run**[**https://deno.land/std/examples/curl.ts**](https://deno.land/std/examples/curl.ts)**[**https://example.com**](https://example.com)**

> **祝贺你，你得到了你的第一个错误。一分耕耘一分收获。**
> 
> **错误:未捕获的权限被拒绝:对“https://example.com/”的网络访问，请使用— allow-net 标志再次运行**

**因此，在默认情况下，Deno 无法访问系统资源，在这种情况下，访问网络。要解决它，你用` **— allow-net** ` flag 为`**Deno run—allow-net**[**https://deno.land/std/examples/curl.ts**](https://deno.land/std/examples/curl.ts)**[**https://example.com**](https://example.com)`赋予 Deno 特权****

# ****创建您的第一个服务器应用程序****

****我们将创建一个聊天应用程序。****

****创建` **chat.ts** `并粘贴以下代码。****

****Your first module in Deno — chat.ts****

****首先，我们从 Deno 提供的标准库导入模块( **WebSocket** 、 **isWebSocketCloseEvent** 和 **v4** )。****

> ****您可以从任何第三方导入任何模块，只需指定这样的 URL(不再需要 package.json)。建议指定您正在导入的模块的版本，在本例中为` **0.51.0** `。这种进口模式的灵感来自我们的 gopher，Golang。****

****`**广播**功能用于向注册用户“广播”消息。****

****`**聊天**功能****

*   ****首先，它向用户注册存储器`**映射图**`,并通知其他用户新用户加入了聊天****
*   ****接下来，它创建一个循环事件来等待任何 WebSocket 事件，如果是合法事件，就广播消息。****
*   ****如果用户关闭 WebSocket 客户端，从内存中删除该用户，并通知团队。****

****接下来，创建` **server.ts** `并粘贴以下代码****

****Main program — server.ts****

****同样，我们使用 Deno 提供的标准模块。****

*   ****它监听端口 3000，并返回`**index.html**'请求`**获取**'****
*   ****然后，它将路径请求` **/ws** 的连接升级为套接字连接，并调用我们从之前编写的` **chat.ts** 导出的` **chat** 函数。****

****这是我们简单的前端代码“T0”index.html。****

****our humble simple frontend****

****要运行服务器，请打开一个新的终端并键入以下命令`**deno run—allow-net server . ts******

******注**:第一次运行时，从网上下载模块并缓存。****

> ****编译文件:///Users/asial/side-projects/deno-chat-app/server . ts****
> 
> ****下载[https://deno.land/std@v0.51.0/http/server.ts](https://deno.land/std@v0.51.0/http/server.ts)****
> 
> ****下载[https://deno.land/std@v0.51.0/ws/mod.ts](https://deno.land/std@v0.51.0/ws/mod.ts)****
> 
> ****…****

****服务器运行后，打开新的浏览器并导航到` **http://0.0.0.0:3000******

****什么？？？什么都不显示！我做错了什么？****

****很高兴你问了。****

****如果您从终端检查日志，它应该会报错有“**读访问权限**”的内容。当我们从程序中读取`**index.html**文件时，我们还需要在运行时指定` **—允许读取**标志。****

****所以让我们再运行一次`**deno run—allow-net—allow-read server . ts******

****现在，它应该工作正常。在此逗留期间，多打开几个浏览器，和自己聊天。****

****我希望你们喜欢它！这里是 Github [回购](https://github.com/yong-asial/deno-chat-app)。****

## ****简明英语笔记****

****你知道我们推出了一个 YouTube 频道吗？我们制作的每个视频都旨在教给你一些新的东西。点击此处 查看我们，并确保订阅该频道😎****