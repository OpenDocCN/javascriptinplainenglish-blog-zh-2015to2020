# 将 Immer.js 与 use-global-hook 集成

> 原文：<https://javascript.plainenglish.io/integrating-immer-js-with-use-global-hook-4a1373d777a5?source=collection_archive---------5----------------------->

## 2020 年的 Pleasant React 状态管理，结合了我最喜欢的两个库。

![](img/35de144438bcbb7f6c60b34c97d0a8d9.png)

**Immer.js** 是 2019 年“**年度突破**”[React 开源奖](https://osawards.com/react/)和“**最具影响力贡献**”[JavaScript 开源奖](https://osawards.com/javascript/)的获得者。将它与 **use-global-hook** 结合起来，我们就有了我尝试过的最愉快的管理复杂状态的方法。

假设你有一个复杂的状态，比如:

**在 Immer** 之前，如果你想改变:`cities[0].weatherStatus.current.weather[0].description` 你需要这样的东西:

如果我们能做到以下几点，生活会变得更容易吗？

是的，我们可以做到这一点，安装非常快。**动手吧！**

## Immer 是如何工作的

![](img/b2532e9c88842ef8bafd7a4a3829b356.png)

*使用 Immer 就像有了一个私人助理；他拿了一封信(当前状态)并给了你一份副本(草稿),让你在上面草草修改。一旦你完成了，助手会拿走你的草稿，为你产生真正的不可变的，最终的信(下一个状态)。*

您可以通过运行以下命令将其添加到您的项目中:

```
npm install immer
```

## use-global-hook 是如何工作的

use-global-hook 是一个轻量级的状态管理库，它不需要 redux 样板文件，但保留了**通量模式**。这意味着你仍然在创造行动来改变你的状态。下面是一个简单的例子:

您可以通过运行以下命令将其添加到您的项目中:

```
npm install use-global-hook
```

# 集成 Immer 和 use-global-hook

从版本`0.2.1`开始， **use-global-hook** 带有开箱即用的 **Immer** 集成。我们只需要将 **Immer** lib 作为 use-global-hook 选项中的依赖项发送。

现在，让我们回到第一个天气例子。要创建一个改变天气描述的动作，我们只需:

如果你的动作返回一个函数， **use-global-hook** 会把它作为一个 Immer 函数来执行。例如，如果您想要一个将数字添加到当前人口的操作，您可以写为:

如果您想查看完整的示例，包括异步操作，您可以查看以下 CodeSandbox:

[https://codesandbox.io/s/immer-integration-e1hpj](https://codesandbox.io/s/immer-integration-e1hpj)

如需更多使用-global-hook，您还可以查看 npm 页面:

[](https://www.npmjs.com/package/use-global-hook) [## 使用全局钩子

### 使用不到 1kb 的钩子轻松管理 react 的状态。目录或添加尽可能多的计数器，它…

www.npmjs.com](https://www.npmjs.com/package/use-global-hook) 

## 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**