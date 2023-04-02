# 使用 ngx-translate 在 JS 包中嵌入翻译

> 原文：<https://javascript.plainenglish.io/embedding-translations-in-your-js-bundles-with-ngx-translate-526ce847acab?source=collection_archive---------3----------------------->

您可能不总是希望通过额外的 HTTP 请求来获取翻译。让我向您展示如何使用 TypeScript 和 [ngx-translate](http://www.ngx-translate.com/) 安全地将它们嵌入 JS 包中。

![](img/345ba8c8171f36ba833ee28f6c2ccf61.png)

Image courtesy of [Maybritt Devriese](https://unsplash.com/@maybritt)

据我所知，似乎大多数人更喜欢在运行时加载当前语言的翻译，在应用程序启动时通过 HTTP 请求，甚至在用户决定切换到另一种语言时。不过，在我的应用程序中，我选择在主 JS 包中包含所有的翻译。

使用这种方法，以后就不需要加载翻译了；一切从一开始就存在。这确实增加了初始加载时间，但是因为这是一个离线优先的应用程序，所以对我来说这不是一个大问题。还可以对其进行优化，例如通过将翻译隔离在它们自己的包中，限制影响并允许长期缓存。不过我不打算在这里讨论这个问题。

在本文中，我将描述我是如何实现这一点的。此外，我还将分享我在 TypeScript 中使用的类型，以防止一些错误。

# 翻译文件

首先，让我们创建两个翻译文件。这里我假设 en_US 和 fr_BE:

en_US.json:

fr_BE.json:

如你所见，这里没什么特别的。我只是将所有翻译包装在一个“I18N”对象中，这样所有翻译都以“I18N”为前缀无论何时在应用程序中使用它们；使得以后很容易找到它们。

# 编译时安全导入翻译

为了在我们的包中包含这些翻译，我们首先要使用 TypeScript 来加载它们:

上面的导入语句利用了 TypeScript 2.9 中引入的一个特性[，称为“resolveJsonModule”，它允许轻松地导入 JSON 文件。可以通过将 tsconfig.json 中的相应属性设置为`true`来启用此功能:](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-9.html#new---resolvejsonmodule)

```
"resolveJsonModule": true,
```

最后一行很有趣，因为它创建了一个表示翻译文件形状的自定义类型。我将马上使用它来确保所有的翻译文件都具有相同的形状，这样我就可以确保我没有忘记用应用程序支持的语言之一来翻译一些东西。虽然不完美，但还是有帮助的。我所做的是，我先写“主”翻译(英语)，然后我翻译成其他支持的语言。

加载后，我将翻译添加到地图/字典结构中:

注意与映射中每个键相关联的类型；这确实是我们根据英语翻译文件创建的自定义类型。这就是我如何确保所有的翻译文件都有预期的*形状*。如果任何翻译文件与“主文件”不一致，此代码将无法编译。

接下来是单一语言的表示。我已经从我当前的项目中 1:1 地得到了这个；您可以随意丰富或简化它:

它只是给出了一种语言的基本信息:

*   一个 ISO(ISO 639-1+ISO 3166-1 阿尔法-2)代码
*   它的每个部分都是独立的(代码和地区)
*   语言的翻译关键字:对语言更改组件有用)(例如，英语)
*   语言的简短翻译关键字:对于压缩的语言更改组件(例如 EN)非常有用

我们还可以定义一个类来保存语言列表:

这个类很有用，因为它允许获得一种具有良好/可读语法的特定语言:

```
Languages.EN_US
```

此外，由于有了`supportedLanguages`数组，我们可以很容易地实现一个基于代码或 ISO 代码查找`Language`对象的函数:

太好了，现在我们有了简单且类型安全的方法来访问我们的语言和翻译。

# 额外收获:注意到缺失的翻译

有时，我们太专注于下一个任务，以至于忘记添加所需的翻译。如果我们这样做了，那么我们需要看到他们失踪了。

让遗漏的翻译更加明显的一个方法是实现 ngx-translate 的`MissingTranslationHandler`接口:

至少这不会被忽视太久；-)

# 将我们的翻译注入 ngx-translate

现在，让我们看看如何将我们的翻译注入 ngx-translate。

幸运的是，ngx-translate 有一个 [TranslateLoader](https://github.com/ngx-translate/core/blob/master/projects/ngx-translate/core/src/lib/translate.loader.ts) 的概念。该接口的目标是提供检索给定语言翻译的方法。

ngx-translate 默认使用 [HTTP loader](https://github.com/ngx-translate/http-loader) ，它使用 Angular 的 HttpClient 通过 HTTP 请求在运行时检索翻译。由于我们想跳过这一步并嵌入我们的翻译，我们需要一个自定义实现:

因为我们已经导入了我们的翻译，所以没有必要获取它们；我们只需要从翻译地图中提取相应的语言。

为了配置 ngx-translate，我们需要创建一个`TranslateModuleConfig`对象:

最后，为了使用它，我们需要在导入模块时传递它:

顺便说一下，注意我在最后对 Angular 的`registerLocaleData`方法的调用。这就是我们如何加载 Angular 提供的附加语言环境信息，如果你使用像`DatePipe`、`CurrencyPipe`、`DecimalPipe`或`PercentPipe`这样的东西，这是必要的。默认情况下，Angular 只包含 en-US 的区域设置数据。因为我们已经通过`Languages`类在应用程序中对此进行了编码，所以我们可以很容易地在这里重用它。如果你想了解更多，请查看 Angular 的[国际化指南。](https://angular.io/guide/i18n)

# 结论

在本文中，我分享了我在当前应用程序中静态加载翻译并将它们作为主要 JS 包的一部分的代码。

然后，我解释了如何使用定制加载器将静态翻译与 ngx-translate 集成。对于像 [Transloco](https://ngneat.github.io/transloco/) 这样的库来说，这应该也是可能的，但是我还没有深入研究这个库；-)

最后，我还解释了如何利用一些很酷的 TypeScript 特性来避免忘记一些翻译。

今天到此为止！

PS:如果你想学习大量关于软件/Web 开发、TypeScript、Angular、React、Vue、Kotlin、Java、Docker/Kubernetes 和其他很酷的主题的其他很酷的东西，那么不要犹豫[去拿一本我的书](https://www.amazon.com/Learn-TypeScript-Building-Applications-understanding-ebook/dp/B081FB89BL)并订阅[我的时事通讯](https://mailchi.mp/fb661753d54a/developassion-newsletter)！

# **用简单英语写的便条**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页[**plain English . io**](https://plainenglish.io/)找到所有这些内容——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**