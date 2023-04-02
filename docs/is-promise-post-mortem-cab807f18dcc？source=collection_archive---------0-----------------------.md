# 死后承诺

> 原文：<https://javascript.plainenglish.io/is-promise-post-mortem-cab807f18dcc?source=collection_archive---------0----------------------->

上周六，我决定试着补上一些对我的开源项目的贡献。我决定合并的第一个拉请求之一是[向 is-promise](https://github.com/then/is-promise/pull/10) 添加一个类型声明文件。

合并后，我决定这也是一个很好的时间来更新模块[支持 ES 模块样式导入](https://github.com/then/is-promise/commit/feb90a40501c8ef69b0c65bdf1eb703182214407)。具体来说，我希望能够`import isPromise from 'is-promise';`不需要启用合成默认导入。在这之后，我运行了测试，并发布了一个新版本。

我曾经打算将我的更多项目设置为通过 CI 自动发布，而不是从我的本地机器手动发布，但是因为 is-promise 是一个非常小的库，所以我认为这样做可能不值得。这绝对是个错误。然而，即使我通过 CI is-promise 设置了发布，也可能没有进行足够彻底的测试。

# 错误

事实证明，我在发布这个新的更新时犯了很多的错误:

*   因为存储库有一个`.npmignore`文件，我假设它在 package.json 中没有一个`"files"`数组，所以我没有更新那个数组来包含新的`index.mjs`文件。
*   当将`"exports"`字段添加到`package.json`中，作为一种告诉 node.js 新版本 ES 模块版本的方式时，我假设路径的工作方式与`"main"`相同，但是它们**需要**前缀`./`。
*   我不明白`"exports"`字段到`package.json`不仅限制了**你可以导入什么**，还限制了**你如何导入它。因此，即使我的更改允许您导入`index.js`文件，如果您正在执行`require('is-promise/index')`或`require('is-promise/index.js')`操作，它也会破坏您的代码。**
*   我也没有考虑到`package.json`中的`"exports"`字段会阻止您导入`require('is-promise/package.json')`。

从长远来看，`"exports"`施加的限制可能是好的，但是它们确实需要测试，并且他们做了一个突破性的改变。

# 时间表

*   2020–04–25t 15:03:25Z—我发表了 is-promise 的[破碎版。主要问题是 package.json](https://github.com/then/is-promise/releases/tag/2.2.0) 中的`["exports"](https://github.com/then/is-promise/blob/8e51d62bf158eb0685cd6109f0137472e8c3cb91/package.json#L7-L10)` [字段](https://github.com/then/is-promise/blob/8e51d62bf158eb0685cd6109f0137472e8c3cb91/package.json#L7-L10)
*   2020–04–25t 17:16:00 z—Ryan Zimmerman[提交了一个拉动请求](https://github.com/then/is-promise/pull/15)，为大多数人解决了问题。
*   2020–04–25t 17:48:00 z—我收到一条脸书消息，要求我查看 GitHub。这是我第一次意识到有些不对劲。
*   2020–04–25t 17:54:00 z—[我合并并释放 Ryan 的拉请求](https://github.com/then/is-promise/releases/tag/2.2.1)。对于大多数人来说，这修复了库，但许多人仍然有缓存版本，一些人通过奇怪的路径导入，现在`package.json`中的`"exports"`阻止了这些路径。
*   2020–04–25t 17:57:00 z—通读了各种问题评论后，我将它们全部关闭，并创建了一个[新标签](https://github.com/then/is-promise/issues/20)，以便我们可以进行更有成效的讨论。
*   2020–04–25t 18:06:00 z—[乔丹·哈班德向我解释了](https://github.com/then/is-promise/issues/20#issuecomment-619418975)为什么`"exports"`仍然是一个突破性的改变。
*   2020–04–25t 18:08:08Z—我将`package.json`中的`"exports"`全部移除，并且[发布固定版本](https://github.com/then/is-promise/releases/tag/2.2.2)
*   2020–04–25t 19:20:00 z—我取消了已损坏版本的发布，试图迫使任何拥有锁定文件的人从这些版本进行更新。

总的来说，图书馆被破坏了大约 3 个小时。

# 成因

各种因素使我犯了我犯过的错误:

*   从我的本地机器上发布总是让人想跳过创建拉请求的重要步骤，让 CI 测试我的更改。
*   我们的测试只覆盖了实际代码，他们没有检查发布给 npm 的内容。
*   我们没有在 node.js 的最新版本上进行测试
*   在这次事件中，我不容易找到。尽管 GitHub 库上有多个贡献者，但他们没有部署新版本的权限。

# 为防止未来问题而采取的措施

*   将节点 12 和 14 添加到 CI 测试套件(【https://github.com/then/is-promise/pull/21】T2)
*   添加了使用`npm pack`测试实际发布的 API 的测试(【https://github.com/then/is-promise/pull/25】T4)
*   移除了`.npmignore`以减少混淆的机会([https://github.com/then/is-promise/pull/28](https://github.com/then/is-promise/pull/28))
*   使用[滚动版本](https://rollingversions.com/)([https://github.com/then/is-promise/pull/25](https://github.com/then/is-promise/pull/25))从 CI 自动发布

# 滚动版本&承诺 3.0.0

这件事之后，我决定尽可能地把一切都设置得更健壮。我还决定，最安全的做法是将所有这些更改作为一个新的主要版本发布，以避免破坏人们的东西，即使他们使用未记录的方法来导入。

在过去的几个月里，我一直在构建滚动版本，这是一个通过持续集成来帮助您安全发布包的工具，同时还有很棒的变更日志。我现在已经将这一点添加到 is-promise 中，这让我对未来的版本更有信心。

从持续集成中释放出来是你可以做的最大的事情之一，来帮助防止类似的事件发生。写变更日志也是帮助你回顾自己的变更并思考其影响的一种非常有效的方式。只有当您将更改日志作为拉请求的一部分而不是在事后编写时，这才真正有效。你可以在变更日志中看到细节的增加，因为我是事后才写的(< 3.0.0) to writing them as part of the pull request (≥ 3.0.0): [https://github.com/then/is-promise/releases](https://github.com/then/is-promise/releases)

如果您想了解更多关于滚动版本的信息，或者想了解我对软件开发和开源的最新想法，您可以在这里订阅我的邮件列表:

![](img/ac2a368df91a0cbd37a18872d81373ed.png)