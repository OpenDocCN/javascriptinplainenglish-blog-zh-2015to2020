# 纱线 2 入门:模糊的进口

> 原文：<https://javascript.plainenglish.io/getting-started-with-yarn-2-ambiguous-imports-a207773b269c?source=collection_archive---------11----------------------->

## 如何解决使用 Yarn 2 时的模糊进口噩梦

所以，如果你像我一样，想开发一个在根目录中使用自定义配置的 CLI/DevKit，你可能会遇到一个小问题。暧昧的电话。例如，当您的应用程序试图。从 Yarn 的角度来看，这是不明确的，因此产生了这些错误。

![](img/e286cc1ddde6928f07044633ba386c6c.png)

Photo by [Tim Gouw](https://unsplash.com/@punttim?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

一个快速简单的补丁是设置`pnpMode: "loose"`来关闭这些错误，但是当然这是不安全的。在[纱线](https://github.com/yarnpkg/berry)上打开一个问题之后，mal 提到了一个修复方法，就是使用 Node.js 的`module.createRequire`特性。按照医生的说法，在一个小时试图使用`import.meta.url`让它工作之后，我决定做我最擅长的事情:停止试图成为我梦想中的开发人员，做一个理性的开发人员。所以我在`.yarn/`下研究了 Yarn Modern 的本地插件代码。我用的配置文件是什么？ESLint，它有一个由 Yarn 开发的兼容性插件。于是我在`.yarn/sdks/eslint/bin/eslint.js`里找，找到了我需要的。那么，我是如何解决这个问题的呢？6 行代码。但是在我们开始之前，让我们假设我们有下面的配置，来自我的 WIP 项目 [Noderalis](https://github.com/Noderalis) 。

我们在这里有一些基本的功能，其中一些是为上下文添加的，以证明这是可行的。注意`console.log('Success!!!');`，因为它显示我们在导入后自动执行。这就是为什么我们不需要导出 Noderalis 的配置，因为它会在执行时自动创建它。最后，我添加了一个小对象`testy`，来展示我们可以抓取东西，就像普通的导入一样。那么我们如何解决这个问题呢？我们来看看代码！

我们使用`createRequire`和`createRequireFromPath`(后者已被否决，但有效)，然后我们使用`require('path/to/.pnp.js').setup()`，它设置环境来要求我们的脚本。这是一个手动步骤，我不能保证做任何事情，因为它应该只在没有检测到 PNP 版本时运行，但手动运行它是我的工作，所以它不会太痛。请记住，我的 require 指向它自己，因为它是唯一需要使用这个逻辑的文件，并且工作得非常好。然后我们像平常一样使用自定义逻辑导入我们的配置！

```
**PS C:\...>** yarn node .\packages\noderalis-core\sources\ProjectConfiguration.js
found package
found noderalis
C:\...\noderalis\.noderalis.js
Success!!!
Successfully created something
found package
found noderalis
C:\...\noderalis\.noderalis.js
req: [object Object]
req: { testy: { name: 'Grim', age: 22 } }
req: [object Object]
req: { testy: { name: 'Grim', age: 22 } }
req: undefined
req: undefined
{ testy: { name: 'Grim', age: 22 } }
```

这有点啰嗦，但经过几个小时的敲打我的头，它仍然是受欢迎的！这给了我们很大的帮助，正如我们可以看到的,`Success!!!`告诉我们，我们导入的配置自动执行了！我们还可以看到我们导出的`testy`对象，并在导入时记录下来。但是为什么它要重复前几行呢？回头看看配置，它导入了`ProjectConfiguration`,这也是导入它的脚本，所以它触发了两次，但这只有在记录您的步骤时才明显，就像我在事情没有按预期运行时做的那样。谈到调试，我有点模拟。

总之，这就是解决这个小问题所需的全部内容！另一方面，即使在删除自定义代码以尝试重现原来的错误后，它也不会出错。所以我假设`.pnp.js => setup()`可能为我们缓存了解析器。