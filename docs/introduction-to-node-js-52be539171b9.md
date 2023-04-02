# Node.js 简介

> 原文：<https://javascript.plainenglish.io/introduction-to-node-js-52be539171b9?source=collection_archive---------7----------------------->

![](img/bf6a4aefe5bf6696a87e527ded2415e8.png)

Photo by [Petar Tonchev](https://unsplash.com/@ptonchev?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是世界上最流行的编程语言之一。为了有效地使用它，我们必须了解它的基本知识。

在本文中，我们将了解 Node.js 的基础知识，包括创建和安装节点模块。

# 在浏览器外运行 JavaScript

Node.js 是一个运行时环境，允许我们在浏览器之外运行 JavaScript 应用程序。

它可以用来以不同的方式进行通信，例如读取和文件。

这些都是浏览器 JavaScript 做不到的。

同样，Node.js 没有 DOM 操作 API、画布操作库和浏览器之外的东西。

# 节点命令

安装 Node.js 运行时后，我们可以用`node`命令运行节点程序。

例如，我们可以写:

`hello.js`

```
const message = "Hello";
console.log(message);
```

然后，当我们运行`node hello.js`时，我们让`'hello'`登录到控制台。

Node 还提供了一个提示，我们可以在其中输入 JavaScript 代码并查看它们的输入。

要退出提示，我们可以运行`process.exit(0)`。

`process`是一个像`console`一样的全局绑定。

`exit`方法结束进程，并给它一个退出状态代码。

0 表示程序成功完成。

任何其他代码都意味着遇到了错误。

`process.argv`从命令行参数中获取字符串数组。

其他全局变量如`Array`、`Math`和`JSON`也是可用的。

然而，特定于浏览器的变量，如`document`和`alert`不是。

# 模块

CommonJS 是一个由节点应用程序使用的模块系统。

它基于`require`功能。

该系统内置于 Node 中，用于加载内置模块、下载软件包和使用我们自己的模块。

以`/`、`./`或`../`开头的路径名相对于其当前路径进行解析。

如果被解析为`node_modules`文件夹中的模块，还有什么。

例如，我们可以通过编写以下代码来创建一个模块:

`module.js`

```
module.exports = {
  foo(){
    console.log('foo')
  }, bar(){
    console.log('bar')
  }
}
```

那么在`index.js`中，我们可以写:

```
const { foo, bar } = require('./module');foo();
bar();
```

我们通过使用`require`函数导入了模块的成员。

然后我们调用里面的函数。

# NPM

NPM 是 JavaScript 模块的在线仓库。

许多模块是专门为 Node 编写的。

当我们在计算机上安装 Node 时，我们得到了`npm`命令。

我们可以用它来和知识库互动。

它的主要用途是下载软件包。

要用它安装一个包，我们可以运行`npm install`后跟包名。

例如，我们可以通过编写以下代码来安装 [chalk](https://www.npmjs.com/package/chalk) 包:

```
npm install chalk
```

如果一个`package.json`文件还没有被创建，它会在我们安装包的时候被创建。

如果包裹没有列在`package.json`里，那么 NPM 会把它添加到`package.json`里。

# 版本

`package.json`列出程序自己的版本和依赖的版本。

版本处理包独立发展的事实。

NPM 包使用语义版本化以及关于哪些版本与版本号兼容的信息。

语义版本由 3 个数字组成，用句点分隔。

例如，`1.0.0`遵循语义版本化。

如果添加了功能，中间的数字会增加。

这样，现有代码无法使用新版本，可能会破坏兼容性。

版本号前面的脱字符号(^)是`package.json`中的依赖项。

例如`^1.0.0`表示允许大于等于`1.0.0`小于`2.0.0`的任何版本。

`npm`命令用于发布新的包或包的新版本。

如果我们在一个有`package.json`文件的目录中运行`npm publish`，那么它会将 JSON 文件中列出的`name`和`version`发布到注册表中。

`npm`只是从 NPM 发布和安装软件包的一种方式。

另一个可选的包管理器是`yarn`，它也做同样的事情，但是使用不同的接口和策略。

![](img/5028bbf8cdfb5ee423cd1afe4067259d.png)

Photo by [Victor Grabarczyk](https://unsplash.com/@qrupt?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

Node.js 是一个在浏览器外运行 JavaScript 程序的运行时环境。

NPM 是一个巨大的 JavaScript 应用包仓库，它有一个用来安装包的程序。

模块大多是 CommonJS 格式的。

节点包遵循语义版本化。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**