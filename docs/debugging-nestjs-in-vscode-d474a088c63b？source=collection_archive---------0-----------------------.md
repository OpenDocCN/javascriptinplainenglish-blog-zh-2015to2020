# 在 VSCode 中调试 NestJS

> 原文：<https://javascript.plainenglish.io/debugging-nestjs-in-vscode-d474a088c63b?source=collection_archive---------0----------------------->

## 如何使用 VSCode 调试 NestJS 应用程序和 Jest 测试的分步指南

![](img/d463ba4786c1068b6540f38078ab15a9.png)

Photo by [Olav Ahrens Røtne](https://unsplash.com/@olav_ahrens?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

本文将介绍用 VSCode 调试 NestJS 应用程序的三种不同技术。

要快速了解 NestJS，[请查看本文](https://codeburst.io/why-you-should-use-nestjs-for-your-next-project-6a0f6c993be)。

Visual Studio Code，或称 VSCode，是一个轻量级的源代码编辑器，支持跨平台，拥有令人印象深刻的各种功能。这些特性中包括用于调试应用程序的优秀工具，包括对 Node.js 和 TypeScript 的内置支持。

虽然 [NestJS 文档](https://docs.nestjs.com/)很全面，但是它目前还没有深入讨论的一件事是调试。

幸运的是，VSCode 使得调试 NestJS 应用程序代码和使用 [Jest 测试框架](https://jestjs.io/docs/en/getting-started)编写的测试变得非常简单。所需要的只是一点点调整。

记住这一点，让我们开始吧。

# 方法 1:启动配置

第一种方法依赖于工作空间根目录下的`.vscode/` 目录中包含的`launch.json`文件。

这个 JSON 文件提供了 VSCode 在确定如何正确调试应用程序时使用的配置信息。

查看[这些文档](https://code.visualstudio.com/docs/editor/debugging#_launchjson-attributes)了解更多关于配置`launch.json`文件的信息。

JSON 文件将如下所示:

Launch configuration for debugging NestJS

上面的文件中发生了很多事情，所以让我们逐个属性地分解它:

*   **类型**–指示要使用的调试器的类型。在我们的例子中，它是`node`，但是如果你有 Go 的调试扩展，你可以把它设置为`go`，等等。
*   **请求**–配置的请求类型。这可以是`launch`或`attach`(启动新流程或附加到现有流程)
*   **name**–您希望与您的调试配置相关联的名称
*   **args–**传递给正在调试的程序的参数。在这种情况下，我们提供了到我们的`main.ts`文件的路径，这是应用程序的入口点
*   **runtime args–**传递给运行时可执行文件的可选参数。因为我们直接向 Node.js 提供了一个 TypeScript 文件，所以我们使用`[ts-node](https://www.npmjs.com/package/ts-node)`来运行该文件，而不需要手动使用 TypeScript 编译器
*   **源地图–**如果找到，使用源地图。这很重要，因为它允许我们单步执行原始的类型脚本代码，而不是编译的 JavaScript
*   **env file-**一个`.env`文件的可选路径。如果您的应用程序依赖于存储在`.env`文件中的环境变量，这是必要的
*   用于查找依赖项和其他文件的当前工作目录
*   **控制台—**显示调试输出时使用哪个控制台。在这种情况下，我们表示希望在 VSCode 中使用集成终端
*   **协议—**使用哪个 Node.js 调试运行时协议。`inspector`协议是最新的

要使用这种`launch.json`配置，首先在应用程序代码中的某个地方设置一个断点。

然后，通过单击图标或使用键盘快捷键(`ctrl+shift+D` / `cmd+shift+D`)导航到 VSCode 活动栏中的 debug 窗格。

最后，从下拉菜单中选择`Debug NestJS Framework`配置，并通过点击开始图标或使用键盘快捷键(F5)运行调试器。

此时，代码应该在断点处暂停，允许您进行调试。

# 方法 2: Nodemon

调试 NestJS 的第二种方法是将`[nodemo](https://www.npmjs.com/package/nodemon)n`与 VSCode 的[自动附加特性](https://code.visualstudio.com/docs/nodejs/nodejs-debugging#_auto-attach-feature)结合使用。

作为快速复习，`nodemon`是一个 NPM 包，在开发 Node.js 应用程序时很有帮助。当检测到对源代码文件的更改时，它会自动重新启动应用程序。

首先，我们需要通过运行`npm i --save-dev nodemon`来确保我们已经将`nodemon`安装为开发依赖项。

有了这个，我们还需要在我们的`package.json`中有一个 npm 脚本来运行`nodemon`。

具体来说，我们加上`"start:debug": "nodemon --config nodemon-debug.json"`。

我们还需要上面脚本中引用的`nodemon-debug.json`文件，如下所示:

Nodemon debugging configuration

上面的配置文件告诉`nodemon`做以下事情:

*   观察`src/`目录中的代码是否有变化
*   寻找扩展名为`.ts`的文件
*   忽略`src/`目录中任何以`.spec.ts`结尾的文件(我们的测试文件)
*   执行命令。在上面，我们使用 Node.js 运行时以及`[ts-node](https://www.npmjs.com/package/ts-node)`在调试模式下运行我们的应用程序

最后一步是启用 VSCode 的自动附加功能。此功能会自动将 VSCode 调试器连接到集成终端中已启动的任何 Node.js 进程。

使用[命令面板](https://code.visualstudio.com/docs/getstarted/userinterface#_command-palette) ( `ctrl+shift+P` / `cmd+shift+P`)并选择`Toggle Auto Attach`可以打开该功能。

您现在可以设置一个断点，在集成终端中运行`npm run start:debug`，并调试您的应用程序。

# 方法 3:启动 Jest 的配置

最后一种方法将包括使用`launch.json`中的新配置在您的 NestJS 应用程序中调试 Jest 测试。

我们需要添加到`launch.json`文件中用于调试 Jest 的配置如下所示:

Launch configuration for debugging Jest tests

这个配置类似于我们用于调试 NestJS 应用程序代码的第一个配置，尽管这次它将`node_modules/.bin/`中 Jest 可执行文件的路径传递给 Node.js 运行时。

我们还指示 Jest 串行运行测试(而不是缺省的并发),给定数组`runtimeArgs`中的`--runInBand`标志。当调试给定的`--coverage`设置为 false 时，我们选择不收集测试覆盖率。

有了这个配置集，我们可以在测试文件中的任何地方设置断点，并运行 VSCode 调试器(假设您已经从活动栏的调试窗格的下拉列表中选择了新的`Debug Jest Tests`配置)。

# 结束语

虽然 quick `console.log`语句通常对快速调试任务很有用，但在调试大型、更复杂的问题时，它的伸缩性并不好。这种情况下，健壮的调试设置就派上了用场。

考虑到框架对类型脚本和依赖注入的使用，调试 NestJS 可能是一个挑战。对我们来说幸运的是，VSCode 的调试工具使这种体验相对轻松。

希望这篇文章能提供信息，并简化下一个 NestJS 项目的调试。

一如既往，我很乐意听到任何关于 NestJS/VSCode/debugging 的想法和反馈。下次见，感谢阅读。

# 参考

*   [本文源代码](https://github.com/nmchenry01/nestjs-blogs/tree/debugging-nestjs)
*   [NestJS 简介](https://codeburst.io/why-you-should-use-nestjs-for-your-next-project-6a0f6c993be)
*   [NestJS 文档](https://docs.nestjs.com/)
*   [VSCode 文档](https://code.visualstudio.com/docs)
*   [Jest 文档](https://jestjs.io/docs/en/getting-started)