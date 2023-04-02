# TypeScript 3.9 带来的改进

> 原文：<https://javascript.plainenglish.io/the-coolest-improvements-coming-with-typescript-3-9-90977bd73a24?source=collection_archive---------6----------------------->

TypeScript 继续以闪电般的速度前进。[我关于 TS 3.x](https://www.amazon.com/Learn-TypeScript-Building-Applications-understanding-ebook/dp/B081FB89BL/) 的书已经在 12 月份发布，现在 3.9 已经在眼前了！唷！

TS 3.9 计划在五月在[发布，但是](https://github.com/microsoft/TypeScript/issues/37198)[的测试版已经出来](https://devblogs.microsoft.com/typescript/announcing-typescript-3-9-beta/)供我们试用。让我们看看这个新版本会给我们带来什么！

# 速度！

正如测试版发行说明中所述，3.9 将带来许多[性能改进](https://devblogs.microsoft.com/typescript/announcing-typescript-3-9-beta/#speed-improvements)。TypeScript 团队和各种贡献者已经修复了围绕这个问题的多个问题。

他们已经解决了大型并集/交集/条件类型以及映射类型的问题。查看测试版发行说明，获得相关的拉请求的一个小列表(不出所料，许多请求是由 Anders 自己发起的:p)。

TypeScript 的项目经理 Daniel Rosenwasser 宣布 material-ui 的编译时间减少了约 40%,这是一个相当大的项目，其构建时间很慢。我不知道你怎么想，但这听起来很棒。

Daniel 还提到 TS 3.9 解决了文件重命名的问题。以前，在文件重命名操作之后，需要花费相当长的时间来确定哪些导入语句必须更新。我目睹了很多次这种情况，这确实很烦人；少了一个需要担心的问题！

最近切换到测试版，我确实可以确认编译时间更好了；尽管我的项目没有*那么*大！

# @ts-expect-error

有时候，在编写测试代码时，我们需要测试 TypeScript 编译器不让我们测试的东西。

例如，如果您编写的 API 将被用 JavaScript 编写的客户端使用，您的代码可能会被错误地调用，测试这种情况也是有用的。

让我们来看一个取自我当前项目的具体例子:

在上面的 Jest 测试中，我确保认证服务优雅地处理无效输入。

在第 3 行，如果我没有使用“未知程度”这个小技巧，TypeScript 编译器就会拒绝编译我的代码。

目前，这是让编译器满意的一种方式。事情是这样的，在将某个东西强制转换为未知的东西之后，我们可以将它再次强制转换为其他任何东西，编译器根本不会介意。即使不完美，这也是可行的，并且在测试环境中是有用的。

另一种方法是在相关行的上方添加一个 *ts-ignore* 注释:

```
*// @ts-ignore*
```

问题是，这只是告诉编译器忽略整行，在这种情况下甚至更糟，因为其他错误可能会悄悄出现，不会被 TypeScript 注意到。

有了 TypeScript 3.9，我们将多了一个选项:在代码行上方添加一个 *ts-expect-error* 注释:

```
*// @ts-expect-error*
```

这个新的“取消注释”告诉 TypeScript 不要报告错误(如果有错误的话)，而是在实际上没有错误的情况下警告我们。

这在我的场景中非常有用，因为我可以避免使用“未知”的技巧，而只需这样做:

不过，需要注意的是如何使用这个特性。在上面的例子中，我可能打错了服务变量的名称、handleError 方法的名称或 subscribe 方法的名称，在所有这些情况下，编译器都不会太介意。

所以最好把这一行分开，这样编译器就能准确地知道哪里会出错。这里有一个更安全的版本:

这样，编译器只希望 handleError 方法参数出错，这正是我在这个测试中想要的。

我想我会停止使用“像…一样不为人知”的伎俩，而是使用这个整洁的新功能，因为它对我来说感觉更安全。

当然，我将继续不惜一切代价避免使用 ts-ignore；-)

# 条件表达式中未调用的函数检查

显然，当 TS 3.7 检测到我们忘记在 if 语句中调用函数时，它已经开始报告错误。

以下是发行说明中的示例:

正如您在上面看到的，在 if 语句中，一个可怜的人忘记了实际调用 hasImportantPermissions 函数。因为函数确实被定义了，所以它总是真的。幸运的是，在 TypeScript 3.7 及更高版本中，编译器会注意到这一点并排除错误。干净利落。

实际上，我在发行说明中并没有注意到这一点，并且很幸运从来没有遇到过这个问题。

在 TypeScript 3.9 中，对检测这种讨厌问题的支持已经扩展到了三元条件。厉害！

我很少忘记调用我的函数，但现在我放心了，即使它发生在我身上，TypeScript 会帮我修复它！:)

# 编辑器改进

正如我在[我的上一篇文章](https://medium.com/@dSebastien/typescript-non-goals-43f47c1ecd84)中解释的，TypeScript 编译器的架构已经被设计成使得代码编辑器和 ide 能够轻而易举地包含 TypeScript 支持。事实上，也正是由于这一点，像 VSCode 这样的编辑器提供了出色的 JavaScript 支持，帮助修复 JavaScript 代码的问题，即使没有使用 TypeScript 编码。

TypeScript 3.9 带来了新的改进。

首先，VSCode 现在支持[在类型脚本版本](https://code.visualstudio.com/docs/typescript/typescript-compiling#_using-the-workspace-version-of-typescript)之间切换。如发行说明所述，请注意还有一个 TypeScript 扩展的夜间版本，您可以使用它从最新的改进中受益。

顺便问一下，你知道我[有一套 VSCode 扩展包](https://medium.com/@dSebastien/vs-code-extension-packs-to-boost-productivity-fa1ba44dfc2e)吗？当然有一个是用于 JavaScript/TypeScript 开发的；-)

关于编辑器改进的所有细节，你最好[看一下发布说明](https://devblogs.microsoft.com/typescript/announcing-typescript-3-9-beta/#editor-improvements)。

总结一下:

*   更好/更同质的 CommonJS 汽车进口(耶！)
*   现在保留换行符的代码动作(这个很好，但是我有我可爱的[更漂亮的](https://prettier.io/)来处理所有代码格式化的废话)
*   支持“解决方案风格”的 tsconfig.json 文件

# 重大变化

虽然听起来很悲伤，但 TS 3.9 也包含了一些突破性的变化。我不会在本文中涉及这些，但如果有兴趣的话，我可能会写一篇后续文章。就我而言，我很幸运，因为看起来我不需要修改我的项目。

无论如何，如果你想了解这些，一定要查看一下[测试版发布说明](https://devblogs.microsoft.com/typescript/announcing-typescript-3-9-beta/#breaking-changes)！

# 结论

在本文中，我简要解释了我认为即将发布的 TypeScript 版本中最有趣的新特性。

我一直在愉快地使用测试版。此外，更新是轻而易举的。所以，去尝试一下吧！如果你很谨慎，那没关系，它很快就会被释放。

今天到此为止！

## 喜欢这篇文章吗？点击下面“喜欢”按钮查看更多内容，并确保其他人也能看到！

PS:如果你想学习大量关于软件/Web 开发、TypeScript、Angular、React、Vue、Kotlin、Java、Docker/Kubernetes 和其他很酷的主题的其他很酷的东西，那么不要犹豫[去拿一本我的书](https://www.amazon.com/Learn-TypeScript-Building-Applications-understanding-ebook/dp/B081FB89BL)并订阅[我的简讯](https://mailchi.mp/fb661753d54a/developassion-newsletter)！

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****