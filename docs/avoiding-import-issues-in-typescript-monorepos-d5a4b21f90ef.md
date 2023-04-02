# 避免 TypeScript Monorepos 中的导入问题

> 原文：<https://javascript.plainenglish.io/avoiding-import-issues-in-typescript-monorepos-d5a4b21f90ef?source=collection_archive---------3----------------------->

当你使用 TypeScript monorepo 时，例如使用 awesome [Nrwl NX](https://nx.dev/) (无论是 Angular、React、Nest 等)，你应该创建许多小的/可重用的库，并使你的应用程序尽可能地轻便，只需根据需要组合这些库。

从类型脚本的角度来看，这些库通常是通过它们的公共 API 使用的，这些 API 可以通过[类型脚本路径映射](https://www.typescriptlang.org/docs/handbook/module-resolution.html#path-mapping)轻松导入，如下所示:

由于这些路径映射，我们可以很容易地从一个容易记住/容易键入的名称中导入内容，而不是使用长的相对路径。

此外，当结合一些林挺规则时，你可以在你的库/应用程序之间强制执行清晰的边界，并确保元素只导入/使用它们应该导入/使用的东西，而不是其他东西。例如，使用 Nx，您可以确保库/应用程序只依赖于它们所使用的公共 API，并且它们从不直接使用不应该被访问的内部元素。

这在一些主流平台和编程语言中是微不足道的，但在现代网络中却不是这样:)

有了路径映射，从其中一个库导入就变得简单了:

```
import { whatever } from "[@didowi/common-api](http://twitter.com/didowi/common-api)";
```

顺便说一句，如果你想知道，Didowi 是我目前正在开发的产品的名字。希望我很快能写更多关于这个的东西；-)

路径映射也很棒，因为它们允许轻松地重新组织库内部，同时限制对 API 客户机的影响。

“导入”方面的另一个优点是，您可以重新组合多个导入，而不是每个导入文件都有一行。干净利落。

到目前为止一切顺利，路径映射非常棒*…*

# *路径映射和 IDE 导入的问题*

*不幸的是，没有什么是完美的。虽然路径映射非常酷，但是它们也会带来问题。*

*正如我在上一篇文章中提到的，库中的元素不应该通过公共 API 桶、通过库路径映射甚至通过桶(因为桶是邪恶的，好吗？).改天我会写一篇关于为什么我现在认为 TS 桶文件是邪恶的文章…*

***通过库的路径映射/通过桶从同一个库中导入元素肯定会导致问题**，无论是循环依赖(Webpack 和其他人会对此大喊大叫)还是偷偷摸摸的问题，比如我在[我的上一篇文章](https://medium.com/@dSebastien/fixing-the-cant-resolve-all-parameters-exception-with-angular-di-1091af1b080)中提到的问题。*

*那么我们能做些什么来避免这样的错误呢？*

*首先，通过将搜索范围限制在一个库中，并确保在导入中找不到该路径映射的任何用法，您可以很容易地搜索和识别这样的无效导入。这将让您快速修复已经出现的任何问题，但不会阻止问题的出现。*

# *IntelliJ / WebStorm 配置*

*在写这篇文章的时候，我在 Jetbrains 的问题跟踪器中偶然发现了这个问题:【https://youtrack.jetbrains.com/issue/WEB-44482*

*根据它，在“**设置|编辑器|代码样式| TypeScript，导入”**到**仅指定路径外的文件”**下设置“**使用来自 tsconfig.json** 的路径映射”选项应该有帮助。*

*这指示 IntelliJ/WebStorm 在导入中只对对应路径之外的文件*使用路径映射。这意味着从同一个库中的元素导入一个库中的元素将使用相对导入来完成，这正是我们对于 monorepo 库所需要的。整洁！**

*根据我的测试，这非常有效，但有一点需要注意；似乎只有当当前文件是与之相关联的入口点(桶)文件的一部分时，IJ/网络风暴才认为当前文件在为路径映射指定的路径的“内部”。如果不是(例如，库内部)，那么 IJ/网络风暴将使用桶导入。*

# *结论*

*在本文中，我们已经看到了为什么 TS 路径映射对于 monorepos/libraries 来说是非常棒的，但是当它们被误用时也会带来问题。*

*ide 经常在导入时出错，但我不能责怪他们，因为 JS/TS 生态系统真的很复杂。令人欣慰的是，我们还发现了隐藏在 IntelliJ/WebStorm 中的一个很酷的小设置，使 IDE 更智能地导入 monorepo 库。我认为使用 VSCode 也可以做到这一点，但是我还没有深入研究过。*

*今天到此为止！*

*PS:如果你想了解大量关于 TypeScript、Angular、React、Vue 和其他很酷的主题的其他很酷的东西，那么不要犹豫[拿一本我的书](https://www.amazon.com/Learn-TypeScript-Building-Applications-understanding-ebook/dp/B081FB89BL)并订阅[我的时事通讯](https://mailchi.mp/fb661753d54a/developassion-newsletter)！*

## ***来自 JavaScript 的简单英语注释***

*我们推出了三种新的出版物！通过以下方式表达对我们新出版物的热爱: [**通俗易懂的 AI**](https://medium.com/ai-in-plain-english)、[、**通俗易懂的 UX**、](https://medium.com/ux-in-plain-english)、[、**通俗易懂的 Python**、](https://medium.com/python-in-plain-english)、**、**——谢谢您，继续学习！*

*我们也一直对帮助推广高质量内容感兴趣。如果您有一篇文章想提交给我们的任何出版物，请用您的 Medium 用户名在[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**发邮件给我们，我们会将您添加为作家。另外，请告诉我们您想添加到哪个出版物中。***