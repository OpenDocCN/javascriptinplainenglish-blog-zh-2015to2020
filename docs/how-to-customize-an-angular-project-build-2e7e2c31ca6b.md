# 如何自定义 Angular 项目构建

> 原文：<https://javascript.plainenglish.io/how-to-customize-an-angular-project-build-2e7e2c31ca6b?source=collection_archive---------2----------------------->

## 迟早会出现这样的情况，当构建 Angular 应用程序时，会出现一个超出构建 Angular 开箱即用所能提供的任务。

![](img/f49e1635925416bebb0affda52f2de02.png)

Source: [Pexels](https://www.pexels.com/photo/build-builder-construction-equipment-585419/)

> 如你所知，这发生在我身上。

有一天天气很好，我想优化我们的应用程序，以提高它在[灯塔](https://developers.google.com/web/tools/lighthouse)中的分数，但马上就遇到了一个经典问题，出于某种原因，Angular 还没有找到通用的解决方案。我们使用的所有自定义字体都需要为浏览器添加信息，以便通过`[<link rel = ”preload” />](https://developer.mozilla.org/ru/docs/Web/HTML/Preloading_content)`预加载它们。这将允许字体开始加载，而无需等待 CSS 内容加载，并将提高性能。问题，坦率地说，不是很关键，但当从一个项目到另一个项目面对它时，我想一劳永逸地解决它。

那么你需要做什么？获取项目字体的信息，并将这些信息添加到 Angular 生成的【index.html】的*中。德洛夫什么的！但是，像往常一样，为了编写一个定制的程序集，需要经历接受不可避免的所有阶段(如果您不想和我一起经历这些阶段，只需转到有现成解决方案的部分)。*

# 1.否定。无需任何定制即可解决问题

根据字体的路径是如何编写的，Angular 下的 webpack 要么用一个唯一的哈希为它们生成新的名称，并把它们放在你的程序集的根目录下，要么根本不接触它们，然后在设置中，你可以启用简单的把源文件所在的文件夹复制到程序集…

我们不需要唯一的名称:在接下来的许多年里，我们几乎没有计划在我们的项目中改变字体，缓存也不会有问题。创建了资产的绝对路径后，就可以在 *angular.json* 的资产设置中注册一个带有字体的文件夹，并将我们需要的链接直接硬编码到 HTML 中。但是，正如您已经理解的，如果一切都这么简单，这篇文章就不会存在🙂

因为我们使用的是 Angular 9 内置的本地化，所以我们在不同语言的几个文件夹中生成一个项目。试图指定字体的绝对路径导致了路径的破坏，因为资产位于带有地区名称的文件夹中，语言前缀没有添加到资产之前的路径中，并且除了 Chrome 之外，全局前缀`<base href="/fr">`在任何地方都不能与绝对路径一起使用(你必须承认，这很奇怪，因为它不应该工作)。因此，试图通过良好的旧硬编码已经失败了。

# 2.愤怒和 npm 脚本

因为链接不能硬编码，所以在读取资产名称、生成缺失的 HTML 代码并将其添加到 Angular build 谨慎创建的*index.html*中的想法产生了。使用 post 命令，您可以在每次构建后自动执行 npm 脚本，任务就解决了。

这条路上的陷阱不是很深，而是在水面上。首先，必须编写脚本，以便同事可以在不同的操作系统上执行。其次，我们再次遇到了本地化问题:对于每种语言，Angular collector 都创建了一个单独的文件夹，每个文件夹都包含自己的 index.html 和自己的字体，浏览所有的文件夹并更改每个 HTML 文件有点太多了。我天生的懒惰没有为这个测试做好准备，我出发去寻找下一个解决方案。

# 3.讨价还价。Github 上的现成包

在这里的某个地方，很明显不定制装配是不可能的。很快就清楚了，在 Angular CLI 第 8 版中，有一个 CLI 构建器 API，允许您创建自己的构建器。他有[相当广泛的](https://angular.io/guide/cli-builder)文档，甚至有[俄文](https://angular24.ru/guide/cli-builder)！但是常见的是，伙计们，当有人可能已经为您编写了一个快速、现成且不是非常最优的解决方案(这也会将一打或两个不必要的依赖项拖到您的项目中)并将其发布在 GitHub 上时，谁想要阅读冗长乏味的文档呢？

事实上，在 GitHub 上快速搜索立即显示出 [npx-build-plus](https://github.com/manfredsteyer/ngx-build-plus) ，在撰写本文时有 915 颗星，最后一次提交是在 5 个月前，还有[@ angular-builders/custom-web pack](https://github.com/just-jeb/angular-builders/tree/master/packages/custom-webpack)，这是几天前更新的，但他只有 736 颗星。

群星压倒一切，我决定给 ngx-build-plus 一个机会，但很快就清楚了，Angular 中的*index.html*的生成不包括在标准的 webpack 汇编中，这意味着没有 [Preload Webpack 插件](https://github.com/GoogleChromeLabs/preload-webpack-plugin)可以再连接，因为这样你就必须生成自己的*索引。html* 带基础 hrefs 和语言，听起来甚至太复杂了。

第二个插件解决了所有问题:它允许你指定标准 webpack 配置的扩展和 index.html 的转换。在这两种情况下，都需要在设置中添加带有必要转换的文件路径。问题是，在一个文件中，我们需要从 webpack 中获取资产列表，在另一个文件中，将关于它们的信息添加到 HTML 中，但是为此，我们决定暂时使用对象`global`。

为了从 webpack 程序集提取我们需要的文件名，编写了以下微插件:

带有 webpack-config 扩展名的文件如下所示:

为了转换 HTML，我使用了正则表达式，并简单地将结果 HTML 添加到样式的第一个链接之前或结束标记`</head>`之前:

所有的缺点在这里立即可见:对于每个设置，我们需要一个单独的文件，使用它不是一个好主意`global`，这意味着我们需要一个 orchestrator。我们得到三个文件加上一个第三方的包，有了它的支持以后可能会有问题。当然，这个插件是通用的，允许你以不同的方式扩展配置，但是我们不需要这种通用性:我们想要的只是重复标准的组装，将关于字体的信息添加到 html 中。

在这里，我们顺利地来到了需要写自己的自行车。

# 4.抑郁症和不可避免的阅读文件

Angular CLI 中的构建器任务由一个名为 Architect 的特殊工具处理，该工具可以在@ angular-devkit / architect 包中找到。链接器任务是执行任务来构建和维护我们的代码的特殊函数。他们潜伏着等待命令，比如`ng build`、`ng lint`、`ng test`。

一个名为 architect 的特殊 *angular.json* 配置部分负责控制台中的命令和链接器之间的通信。在这里，您可以设置命令和执行命令的特定连接器之间的关系。

每个命令都有三个配置选项。键`builder`指示将使用哪个链接器，设置默认参数`options`，可选键`configurations`允许您覆盖各种配置的默认参数。在这种情况下，链接器名称`builder`由两部分组成:npm 包的名称和直接在这个包内的链接器的名称。

例如，我们最感兴趣的命令`build`(正如我们所知，正是构建启动了网络包并生成了*index.html*)使用了@ angle-dev kit/build-angle:浏览器链接器。因此，为了了解它是如何工作的，您可以在这里查看[(在@ angular-devkit / build-angular 软件包中)。](https://github.com/angular/angular-cli/tree/master/packages/angular_devkit/build_angular/src/browser)

架构师如何在这样的包中找到需要的任务，以及他如何理解他需要什么参数？在的*包. json 中，任何带有链接器的项目都必须有一个特殊的属性 [builders](https://github.com/angular/angular-cli/blob/2039f286d84efa743914b80bb9dec7815eeee8fe/packages/angular_devkit/build_angular/package.json#L8) ，该属性[指定了 JSON 文件的](https://github.com/angular/angular-cli/blob/2039f286d84efa743914b80bb9dec7815eeee8fe/packages/angular_devkit/build_angular/package.json#L8)路径(通常但不一定是 *builders.json* )，并且该文件中已经包含了当前项目中该[方案](https://github.com/angular/angular-cli/tree/master/packages/angular_devkit/build_angular)的所有链接器任务的信息。*

是的，是的，在胸( ***angular.json*** )一只野兔( **architect** ),在一只野兔(`architect.build.builder`)——一只鸭子(@**angular-devkit/build-angular**:browser)，在一只鸭子(**@ angular-devkit/build-angular**)——鸡蛋(***builder . JSON***，你在这里)，在鸡蛋针(builder code)里。也就是说，最后一步仍然是最终找到我们需要的收集器的代码。如果你还没有完全糊涂，让我们继续前进😉

我们的 ***builders.json*** 必须包含 [builders](https://github.com/angular/angular-cli/blob/2039f286d84efa743914b80bb9dec7815eeee8fe/packages/angular_devkit/build_angular/builders.json#L3) 键。它的值将是一个对象，其键是各个链接器任务的名称(特别是，对于这样的对象中的**@ angular-dev kit/build-angular**，我们肯定会找到 [browser](https://github.com/angular/angular-cli/blob/2039f286d84efa743914b80bb9dec7815eeee8fe/packages/angular_devkit/build_angular/builders.json#L9) 键)，值是关于从哪里获得实现细节的信息。

实现细节有三个部分:代码的路径(实现)，描述链接器所需参数的模式的文件的路径(模式)，最后只是描述。因此，我们看到角度收集器代码在处显示为[。](https://github.com/angular/angular-cli/blob/master/packages/angular_devkit/build_angular/src/browser/index.ts)

我保证，我们还需要耐心一点，通过手指在键盘上的灵巧移动，我们将扩展角状收集器😉

所以，说几句关于实现的话。

创建任何链接器都需要一个由**@ angular-dev kit/architect**提供的方法`createBuilder()`。这个方法接受一个异步函数，这个函数执行我们所有的链接器逻辑。对于来自**@ angular-dev kit/build-angular**的浏览器构建器来说，它看起来像[这个](https://github.com/angular/angular-cli/blob/2039f286d84efa743914b80bb9dec7815eeee8fe/packages/angular_devkit/build_angular/src/browser/index.ts#L840):

同时也是`buildWebpackBrowser`后来以`[executeBrowserBuilder](https://github.com/angular/angular-cli/blob/2039f286d84efa743914b80bb9dec7815eeee8fe/packages/angular_devkit/build_angular/src/index.ts#L37)`的名义对外输出(意思是——可以重复使用！).

我们最后来看看`[buildWebpackBrowser](https://github.com/angular/angular-cli/blob/2039f286d84efa743914b80bb9dec7815eeee8fe/packages/angular_devkit/build_angular/src/browser/index.ts#L245)`是什么功能:

无需深入实施细节，很容易看出该方法采用可选参数`webpackConfiguration`和`indexHtml`，其中

**什么意思？标准的 Angular builder 实际上允许您添加 webpack 配置和 HTML 内容的异步转换，并且您不需要完全重新发明轮子！这就是:有了这些知识，您终于可以看到我是如何创建链接器并向它传递预加载资产的必要逻辑的了。**

# 5.领养。我们为 angular 编写自己的收集器

要解决的第一个问题是将自定义链接器(收集器)放在哪里？在官方文档和大多数示例中，建议为此创建一个单独的项目，并将其发布到 npm。但事实上——哒哒！—这不是必需的，如果您不想担心支持一个单独的包，只需记住您已经有了至少一个包含 *package.json* 的存储库，准备好指向所需的链接器:您的项目本身。

因此，在我的项目的 *package.json 中，我指定了*

```
**"builders"**: "builders.json",
```

并且我确保@ angular-devkit / architect 和@ angular-devkit / build-angular 出现在我的依赖项中。

因为我扩展了标准的 Angular 构建器，所以我将构建器命名为相同的名称:browser。我的构建器不需要任何不同于标准构建器的新参数，所以我也决定不创建模式，而是轻率地添加了一个来自**@ angular-devkit/build-angular**的到浏览器构建器模式的链接和某种描述:

为了`ng build`使用自定义收集器而不是标准收集器，在 *angular.json* 中，用新的收集器替换标准收集器就足够了(由于链接器配置在当前项目中，而不是包名中，所以需要用 ***package.json*** 指定目录的相对路径):

```
**"architect"**: {
  **"build"**: {
    **"builder"**: "./:browser",
```

因为所有参数都保持不变，所以不需要改变任何东西！

很好，现在您所要做的就是编写收集器本身的实现。为了简单起见，我抛弃了 TypeScript，使用了纯 JavaScript。

一个不会做任何新事情的自定义收集器看起来像这样:

在这里，我们可以使用 webpack 插件和方法来添加缺少的 HTML 部分，上面写的使用**@ angular-builders/custom-web pack**，和`global`用一些局部变量(例如`sharedSpace`)替换使用:

就这些，我们的收集器已经准备好了，并且很好地完成了它的新任务！总的来说，我们有一个 JSON 配置，一个带有实现的 JS 文件，以及对 *package.json* 和 *angular.json* 的一些更改。看起来删除它并不困难，如果😀

而你，如果你把这篇课文读完了，甚至明白了一些东西，就是一个了不起的家伙。

借此机会，我想感谢这些酷家伙们关于 linkers 的文章，并建议你进一步研究这篇文章和官方文档(现在你肯定会掌握它):

*   [亚历山大波什塔鲁克角 CLI 流。大图](https://medium.com/angular-in-depth/angular-cli-flows-big-picture-9ed1a0d1930)；
*   [杰布巴拉巴诺夫。引擎盖下的 Angular CLI 建设者揭秘 v2](https://medium.com/angular-in-depth/angular-cli-under-the-hood-builders-demystified-v2-e73ee0f2d811)；
*   [杰布巴拉巴诺夫。定制 Angular CLI 构建——ng eject(v2)的替代方案](https://medium.com/angular-in-depth/customizing-angular-cli-build-an-alternative-to-ng-eject-v2-c655768b48cc)；
*   [Angular CLI Builders API 官方文档](https://angular.io/guide/cli-builder)。