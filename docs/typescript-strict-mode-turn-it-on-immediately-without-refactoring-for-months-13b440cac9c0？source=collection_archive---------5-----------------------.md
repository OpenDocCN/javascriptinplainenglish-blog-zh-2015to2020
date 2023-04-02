# TypeScript 严格模式:几个月不重构立即打开！

> 原文：<https://javascript.plainenglish.io/typescript-strict-mode-turn-it-on-immediately-without-refactoring-for-months-13b440cac9c0?source=collection_archive---------5----------------------->

![](img/2118ed391acb9e3dd670f28710e1f316.png)

Photo by [Aziz Acharki](https://unsplash.com/@acharki95?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

TypeScript 使我们能够利用类型安全和代码文档来改进 JavaScript 应用程序的架构。然而，默认情况下，它不会打开“严格模式”,以降低新适配器的准入门槛。Angular 采用了同样的方法。除非明确指定，否则在生成新的角度应用时，它不会切换到严格模式。这可能会妨碍我们充分利用 TypeScripts 的强类型特性，这种特性允许我们在编译时检测一整类错误。

**有一种方法可以立即获得严格模式的好处，即使您现有的应用程序有成百上千个类型脚本文件**。继续阅读了解更多！

# 架构影响软件的经济性

马丁·福勒给出了软件架构的一个流行定义:

> "软件架构是那些既重要又难以改变的决定."—马丁·福勒

这些决定深刻地影响了添加一个新特性所花费的时间成本和精力(软件工程经济学)。

作为软件工程师/架构师，我们的目标是保持最高水平的内部代码质量，以确保代码库是可维护的。这确保了通过像乐高积木一样将特性插入到代码库中，并持续地将它们发送给我们的最终用户，可以快速地开发出特性。如果你想了解更多关于软件架构的知识，可以看看由[思想工作者](https://www.thoughtworks.com/books)写的[这本伟大的书。](https://amzn.to/2Fo6uYc)

`strict`标志是一个伞状标志，可同时切换以下六个标志:

*   `noImplicitAny`
*   `noImplicitThis`
*   `strictNullChecks`
*   `strictPropertyInitialization`
*   `strictBindCallApply`
*   `strictFunctionTypes`

这些标志的目的是加强 TypeScript 提供的类型安全，以避免编译时出现一整类错误。这里有一篇文章解释这些标志。这对于减少代码库中的错误流量，降低软件熵，从而减少添加新功能的工作量/成本非常有用。但这在现实中并不容易做到。

# 立即打开它可能会导致 1000 个错误

如果您正在处理一个具有成百上千个 TypeScript 文件的中型到大型应用程序，那么立即打开严格模式可能并不可行。您肯定会遇到多个编译时错误并破坏您的构建。暂停特性开发并花费几个月的工程时间来修复这些编译错误是不理想的。

**有一种方法可以在改进代码库的类型安全性和架构的同时继续发布新特性:**

*   在打开 strict 标志的情况下，为您的代码编辑器使用单独的`tsconfig.json`文件
*   当开发人员在文件中遇到代码编辑器显示的林挺/编译器错误时，在您的组织中设置一个规则来修复它们。
*   专业提示:在 CI/CD 管道中添加一个构建步骤，使用 strict `tsconfig.json`文件构建您的应用程序，为每个版本生成一个报告，以绘制和监控编译器错误数量随时间减少的情况

![](img/4a023ee9938e8f145013ab726776bf59.png)

Photo by [Mahir Uysal](https://unsplash.com/@mahiruysal?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 诀窍是

如果我们能为 VSCode 设置一个自定义的`tsconfig.json`就好了(我假设我们大多数人都在使用 vscode)。但是由于根据这个 Github 问题这还不可能，我们可以使用一个变通方法。让我们假设您正在使用一个 [Angular](https://angular.io/) 应用程序，但是这个技巧适用于浏览器或节点运行时中的任何 TypeScript 应用程序

*   将您的 tsconfig.json 文件重命名为`tsconfig.build.json`
*   在您的项目根目录下运行`tsc --init --strict`来创建一个`tsconfig.json`文件，打开严格标志进行严格的类型检查
*   修改`tsconfig.app.json`文件以扩展`tsconfig.build.json`和`tsconfig.spec.json`并保存。

好了🎉🎉🎉

我们已经让 VSCode 使用新创建的`tsconfig.json`

**现在，您将在不破坏 Angular/Webpack 构建的情况下收到针对您的 TypeScript 文件中的严格键入错误的视觉反馈**

# 警告

像任何伟大的决定一样，总会有权衡取舍。通过使用此解决方案，我们将注意到以下注意事项:

*   大多数 TypeScript 文件中都会出现许多错误，这可能会令人望而生畏
*   由于严格模式，很难将正在进行的开发中的新错误与先前存在的错误区分开来，这些新错误可能会破坏构建

然而，在我看来，在编辑器中显示错误的好处超过了它的不便。

# 摘要

作为软件工程师/架构师，我们的主要目标之一是保持应用程序的高质量。在本文中，我们看到了如何在现有的大中型应用程序中启用严格的类型检查设置。我们学会了如何立即利用严格的类型检查，而无需重构成百上千的错误。我们学习了如何使用它来检测代码编辑器中的输入错误，从而在编译时消除许多错误。我们还了解到，所有的架构决策总是有权衡。

感谢你的阅读，我希望这能帮助你，就像它帮助了我一样。

我很想在下面的评论中知道这是否有用！

可以在 [GitHub](https://github.com/nivrith) 或者 [Twitter](https://twitter.com/_Nivrith_)
快乐工程上关注我！