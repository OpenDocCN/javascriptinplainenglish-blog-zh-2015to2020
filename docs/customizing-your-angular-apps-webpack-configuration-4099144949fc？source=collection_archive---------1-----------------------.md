# 自定义 Angular 应用程序的 Webpack 配置

> 原文：<https://javascript.plainenglish.io/customizing-your-angular-apps-webpack-configuration-4099144949fc?source=collection_archive---------1----------------------->

## 让我们看看如何定制我们的 Angular 应用程序的 Webpack 配置

![](img/91a8a70f23472a914c1ccb569c18cc5d.png)

Picture courtesy of [Alen Jacob](https://unsplash.com/@alenjcreative)

在这篇文章中，我将解释*为什么*你可能需要定制你的 Angular 应用程序的 Webpack 配置，我还将向你展示*如何做*。

这实际上是我已经在[的上一篇文章](https://medium.com/javascript-in-plain-english/adding-tailwind-support-to-a-nrwl-nx-workspace-with-angular-and-storybook-bf890ea882e)中涉及到的东西，但是在这里我想集中在 Webpack 部分(为什么和如何)。

# 为什么不呢？

通常，没有必要摆弄 Angular 的构建系统，这是一件*好事*。

大约在 2016 年，我是一系列项目的发起人、PM 和技术负责人，这些项目旨在使比利时国家银行创建 Web 应用和 API 的方式现代化。作为该项目的一部分，我想创建一个好的解决方案来构建我们的应用程序。

在业余时间，我花了几十个小时为 Web 应用程序创建了一个基于 gulp 的现代构建系统。当时，我发现网络上许多很酷的工具缺乏集成，想要一个简单易用的解决方案来解决反复出现的问题。

我的项目有很多优点:ES2015 & TypeScript 支持、SASS 支持、使用 CORS 的实时重新加载和跨设备同步(使用 BrowserSync)、sourcemaps、CSS 优化/缩小、JS 缩小/捆绑、HTML 缩小、图像优化、JS/TS/CSS 代码质量和代码风格检查等。我甚至为它创造了一个约曼发电机。这个想法很吸引我:定义一次构建系统并重用它，而不是在每个应用程序中维护一次性的构建系统。

问题是，当我在那个副业项目上取得进展时，我让自己沉浸在复杂的海洋中(这是 TBH 的乐趣)。然后，Webpack 变得超级流行(理由很充分)，因为它让很多事情变得简单多了。我很快意识到吞咽已经成为过去。然后，随着时间的推移，Angular CLI 取得了巨大的进步，在某种程度上，很明显 CLI 是正确的选择。

长话短说，除非您深入研究了 CLI 构建系统处理的集成问题，否则您可能不知道创建类似的东西有多复杂，它不仅仅是传输或捆绑您的代码。这里面隐藏着无数的技术挑战。它不仅仅创造了一次。随着时间的推移，随着拼图的所有部分继续进化，它也保持其功能。你只是不想做所有的工作。

想要个例子吗？我想你已经听说了 [TSLint 被](https://github.com/palantir/tslint/issues/4534)抛弃，取而代之的是 [ESLint](https://eslint.org/) 。这对 TS 社区来说太棒了。但是 TSLint 仍然是 Angular CLI 的默认设置，现在无数的应用程序依赖于它，有时甚至不知道。Angular 团队现在正在[准备淘汰和向 ESLint](https://github.com/angular/angular-cli/issues/13732) 迁移。而且这事[也没那么容易](https://github.com/angular/angular-cli/issues/13732#issuecomment-575796158)。对我们来说幸运的是，他们正在处理这件事，而我们可以专注于构建酷的 Web 应用程序。正如他们所说，无知是福…

在过去，定制由 Angular CLI 管理的构建系统的唯一选项是使用`ng eject`命令，但是从 Angular 6 开始不再支持[。无论如何，弹射的问题在于它是一列单程列车。如果您使用了它，那么就没有(容易的)返回方法，并且您继承了所有的复杂性和维护负担。所以“驱逐”基本上是不可以的(除了拥有足够资源的大型组织)。](https://github.com/angular/angular-cli/issues/10618)

另一个试图避免定制你的构建的原因是 Angular 越来越接近能够支持 [Bazel](https://bazel.build/) 的事实，这对每个人的性能都有重要影响。当 Bazel 支持变得广泛可用时，如果您深陷定制领域，那么[您可能会后悔](https://medium.com/@martindzejky/from-custom-webpack-build-to-angular-cli-9d87c3da6925)，因为您将被“旧”所困。

# 为什么

好了，你知道为什么不应该定制构建了。但是如果你真的需要呢？在很多情况下，定制/扩展构建是有意义的，甚至是解决某些问题的唯一方法。

如果你想要/需要使用像 [Tailwind](https://tailwindcss.com/) 这样的工具，它们必须[与构建](https://medium.com/javascript-in-plain-english/adding-tailwind-support-to-a-nrwl-nx-workspace-with-angular-and-storybook-bf890ea882e)集成，那么你没有太多的选择。

例如，上周，另一件需要我定制构建的事情是， [MomentJS](https://momentjs.com/) 地区都被添加到了我的应用程序的 JS 包中，尽管我只需要 2-3 个。我本来可以忽略它，但是对包大小的影响太大了。

因此，虽然您希望保持谨慎，不要走极端，但是有充分的理由不时地定制构建。你只需要小心不要走得太远。

# 方法

你可能知道，Angular CLI 仍然在引擎盖下使用 [Webpack](https://webpack.js.org/) 。

要定制构建系统，有两种主要的解决方案:

*   由 [Manfred Steyer](https://twitter.com/ManfredSteyer?ref_src=twsrc%5Egoogle%7Ctwcamp%5Eserp%7Ctwgr%5Eauthor) 创建/维护的 [ngx-build-plus](https://github.com/manfredsteyer/ngx-build-plus) 实用程序
*   由[“杰布”巴拉巴诺夫](https://twitter.com/_Just_JeB_)创建/维护的 [angular-builders 自定义网络包](https://github.com/just-jeb/angular-builders)实用程序

这两个项目都支持使用定制的 Webpack 配置来扩展默认构建，这正是我们主要追求的。ngx-build-plus 提供了一些原理图和对角度元素的支持；如果你在那之后，那么你知道使用什么；-)

就我而言，我选择了 angular-builders 定制 webpack，到目前为止，我是一个快乐的用户。最酷的事情(实际上也是 ngx-build-plus，afaik 所支持的)是它没有取代 CLI 的默认版本。相反，这个项目允许你扩展它，甚至对不同的部分使用不同的合并策略。

# 怎么

好了，让我们看看如何使用 angular-builder custom-webpack 定制构建。

首先，您需要安装以下依赖项:

```
npm install --save-dev @angular-builders/custom-webpack
```

当然，我将它保存为`devDependency`，因为它只在开发过程中需要。完成后，您需要修改项目中的 **angular.json** 文件，以便使用自定义构建器并提供自定义 Webpack 配置的路径:

这里，我为`build`和`serve`目标重用了同一个 **webpack.config.js** 文件，但是如果需要的话，你可以使用不同的文件。这完全取决于你想要达到的目标。

接下来当然是 **webpack.config.js** 文件:

正如你所看到的，这很简单。您只需要导出一个标准的 Webpack 配置对象。冷静点。从现在开始，只需要知道 Webpack 是如何工作的。如果您还不熟悉网络包，您肯定应该了解它。这是一个很好的时间投资。

默认情况下，angular-builder web pack-config 使用[https://github.com/survivejs/webpack-merge](https://github.com/survivejs/webpack-merge)及其选项自动将您的配置与 CLI 的默认配置合并。

这非常有效，可以使用`mergeStrategies`和`replaceDuplicatePlugins`选项进行调整。多亏了这些，您可以决定您的自定义配置是附加到默认配置(默认行为)上，还是应该完全替换默认配置。`replaceDuplicatePlugins`选项让你按照自己的定义替换插件。

对于更复杂的场景，您也可以导出函数而不是对象。当您这样做的时候，angular-builder custom-webpack 将调用带有默认配置、默认选项等的函数，并让您决定具体做什么来生成最终配置。我不建议您这样做，除非您确定您需要这样做，因为您将失去默认行为和对自动配置合并的支持。

查看文档，了解更多关于铃声和哨声的信息！JeB 还在[角度深度](https://medium.com/angular-in-depth/customizing-angular-cli-build-an-alternative-to-ng-eject-v2-c655768b48cc)中写了一篇关于他的工具的有趣帖子。

# TypeScript 支持

正如你可能知道的，我是 TypeScript 的超级粉丝(以至于我甚至为此写了一本该死的书)。我很高兴地发现，用于角度构建器 webpack-config 的自定义 Webpack 配置文件可以用 TypeScript 编写。太棒了。

因此，这里有一个早期定制 Webpack 配置文件的 TS 版本:

您只需要将其名称修改为`webpack.config.ts`并相应地更新您的`angular.json`文件。

生活难道不美丽吗？；-)

# 结论

在本文中，我给了你一些避免定制你的 Angular 应用程序的理由。然而，由于有相当多的情况下确实需要定制，所以我给出了不同的方法，您可以使用这些方法来扩展甚至覆盖由 Angular CLI 提供的默认构建管道的某些部分。

正如我们所看到的，在实践中很容易做到，甚至可以用 TypeScript 编写。

无论如何，如果可以的话，尽量避免这种情况，这样一旦 Angular 及其 CLI 转向 Bazel，您就不会被一个难以维护的自制构建系统所困。

今天就到这里吧！

# 喜欢这篇文章吗？

如果你想了解很多关于软件/网络开发的很酷的东西，比如 TypeScript、Angular、reactor、Vue、Kotlin、Java、Docker/Kubernetes 以及其他很酷的主题，那么请不要犹豫[拿一份我的书](https://www.amazon.com/Learn-TypeScript-Building-Applications-understanding-ebook/dp/B081FB89BL)并订阅[我的时事通讯](https://mailchi.mp/fb661753d54a/developassion-newsletter)！