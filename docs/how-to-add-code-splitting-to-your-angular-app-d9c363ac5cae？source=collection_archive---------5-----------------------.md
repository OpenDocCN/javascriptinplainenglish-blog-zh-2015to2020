# 如何将代码拆分添加到 Angular 应用程序中

> 原文：<https://javascript.plainenglish.io/how-to-add-code-splitting-to-your-angular-app-d9c363ac5cae?source=collection_archive---------5----------------------->

![](img/612b98cc320cc3e116faca19c0926012.png)

随着 web 应用程序功能的增长，我们也看到了它们的捆绑包大小的增长。一方面，我们需要为很酷的新功能发布更多代码。另一方面，我们也需要第三方库和他们的代码来让一切工作。

结果呢？巨大的初始包大小，因此，更多的加载时间。这反过来会导致更多的用户离开你的应用，从而导致业务损失。代码分割是处理这个问题的一种有用的技术。

# 什么是代码拆分？

*“代码分割是将代码分割成不同的包或组件，然后按需或并行加载。”—* [*MDN 网络文档*](https://developer.mozilla.org/en-US/docs/Glossary/Code_splitting#:~:text=While%20the%20total%20amount%20of,be%20dynamically%20loaded%20at%20runtime.)

换句话说，您将您的应用程序划分成更小的包，并使您的初始包尽可能小。然后，当用户需要时，可以下载某个特性的包。另一种说法是延迟加载。

在这篇文章中，我将介绍三种对 Angular 应用进行代码拆分的方法——在模块、组件或包的级别。

# 模块级代码拆分

这是对 Angular 应用程序进行代码拆分的最好方法之一。

Angular 中的模块通常是指可以由导航路线分隔的不同部分。

例如，如果我们正在建立一个学校管理应用程序，我们可以有一个模块来管理学术，一个模块来管理体育等。

假设我们称我们的学术模块为`AcademicsModule`。在我们的应用程序中创建它之后，下一步是为它添加一条路线。我们需要做的就是在我们的路由配置中指定以下内容。

确保您的`AcademicsModule`没有被导入到您的任何其他模块中，否则它可能会包含在您的主包中。使用上面的代码，webpack 将识别 import 语句，并为包含代码及其依赖项的模块创建一个单独的包。

一旦用户导航到/academics 路径，`AcademicsModule`就会被延迟加载。

关于模块上代码分割的更多细节，请查看 Minko Gechev 的这篇优秀文章。

[](https://web.dev/route-level-code-splitting-in-angular/) [## 角度路由级代码拆分

### 通过使用路由级代码分割来提高应用程序的性能！这篇文章解释了如何设置路由级别…

网络开发](https://web.dev/route-level-code-splitting-in-angular/) 

# 组件级代码拆分

组件是 Angular app 的构建块，粒度比模块高。虽然我们应该总是努力使组件简单和可重用，但有时这是不可能的。您的组件可能会使用大量的服务，或者在其中包含子组件。在初始包中包含这样的组件会不必要地增加应用程序的臃肿。

# 如何在 Angular 中对组件进行代码拆分？

嗯，以前没那么容易。但是有了棱角分明的常春藤，就方便多了。与模块一样，我们将使用 webpack 动态导入语句。

## 动态导入 ala webpack

动态导入是 webpack 的一个特性，通过它 webpack 可以识别模块、组件等。它们需要延迟加载，并在编译时为它们创建单独的包。

在运行时，import 语句解析为一个 Promise，因此应用程序应该使用 async/await 组合或 Promise.then 来等待它。然后，导入的模块可以在您的代码中使用。

欲了解更多信息，请点击这里查看 [webpack 官方文档](https://webpack.js.org/guides/lazy-loading/)。

看看下面的学校管理应用程序的 ReportCardComponent 的代码示例。

然后是你的模板文件。

```
<ng-template [ngComponentOutlet]="dynamicComponent">
</ng-template>
```

所以我们在这里做的是声明一个组件类型变量——它将包含我们的惰性加载组件。然后我们使用一个函数来加载我们想要延迟加载的`ReportCardComponent`,并将它赋给这个变量。

最后，我们使用 Angular 提供的`ngComponentOutlet`指令并在那里指定我们的变量。这将在组件加载后立即将其添加到我们的 UI 中。我们可以随时调用该函数，无需将它与路线变更相关联！这使得它成为以组件的形式动态加载我们的应用程序的一种非常灵活的方式。

还有很多内容超出了本文的范围。如果你对此感兴趣，可以看看 [Netanel Basal](https://medium.com/u/b889ae02aa26?source=post_page-----d9c363ac5cae--------------------------------) 的这篇精彩文章。他解释了以这种方式动态加载组件的不同方式。

[](https://netbasal.com/welcome-to-the-ivy-league-lazy-loading-components-in-angular-v9-e76f0ee2854a) [## 欢迎来到常春藤联盟:Angular v9 中的延迟加载组件

### 用 Ivy 延迟加载组件

netbasal.com](https://netbasal.com/welcome-to-the-ivy-league-lazy-loading-components-in-angular-v9-e76f0ee2854a) 

# 封装级代码拆分

最后，但同样重要的是，我们还可以在 npm 包级别应用代码拆分。通常，在构建一个特性时，我们会求助于使用现有的第三方 javascript 库。

这很方便，可以节省你的时间！

但是如果依赖项很大，会增加初始加载时间。如果你使用依赖的唯一地方是在应用程序的特定部分，这尤其成问题。为什么我们要为所有用户预先加载这个，而大多数用户甚至不需要它？

这是一个显而易见的事实，当用户需要时，这些库应该被延迟加载。开发人员包含 javascript 库的典型方式是使用单独的服务，并在文件顶部使用静态导入语句，就像这样(这是针对 PDFMake 库的)。

```
import pdfMake from 'pdfmake/build/pdfmake';
import pdfFonts from 'pdfmake/build/vfs_fonts';
pdfMake.vfs = pdfFonts.pdfMake.vfs;
```

这里的问题是这会将库添加到主包中。如果要应用代码拆分，请使用下面的方法。

这确保了 PDFMake 库只在用户需要时，在调用 loadPdfMaker 函数时才被加载。可以添加一个加载器来给用户一个可视的指示。还要注意加载的库是如何保存为服务变量供将来使用的。

这种技术可以用于几乎所有的包，如果你有一个大的库，拖下你的应用程序，它会变得非常灵活。

要了解更多细节和详细的工作代码示例，请查看我不久前发表的关于以这种方式将 **PDFMake** 添加到 Angular 应用程序中的[帖子](https://zoaibkhan.com/blog/generate-pdf-in-angular-with-pdfmake/)。

[](https://zoaibkhan.com/blog/generate-pdf-in-angular-with-pdfmake/) [## 使用 PDFMake 以角度生成 PDF "

### 最近，我为一个客户开发了一个功能，包括从他的 Angular web 应用程序生成一个 PDF 文档…

zoaibkhan.com](https://zoaibkhan.com/blog/generate-pdf-in-angular-with-pdfmake/) 

# 结论

总之，Angular 中的这三种代码拆分技术打开了一扇可能性的大门。我们可以为新用户提供一个轻薄智能的初始应用，同时在用户需要时按需加载复杂的功能。

在哪里使用模块，组件和包，取决于你作为一个开发者，在某种程度上，取决于你的应用程序的结构。好好用，有很棒的灯塔评分当奖品！

我希望这篇文章对你的编码之旅有所帮助。

感谢阅读。

再见:)

本帖原载于 zoaibkhan.com*[T5。关注我的](https://zoaibkhan.com/blog/how-to-add-code-splitting-to-your-angular-app/)* [*推特*](https://twitter.com/zoaibdev) *了解更多！*