# 对第三方角度组件进行强制更改检测

> 原文：<https://javascript.plainenglish.io/forcing-change-detection-on-a-third-party-angular-component-e6008beab6ef?source=collection_archive---------1----------------------->

有一种方法可以解决第三方角度组件的变化检测问题。

![](img/f4e2e599951eea1761e8f19d76ad4704.png)

Picture courtesy of [35mm](https://unsplash.com/@35mm)

在我们的应用程序中，我们应该尽最大努力继续使用 [OnPush 变化检测策略](https://blog.angular-university.io/onpush-change-detection-how-it-works/)。不幸的是，有时我们会被无法正确检测变化的第三方组件所困扰。

例如，我经常遇到变化检测和灌注组件的问题，它们(在这一点上)仍然没有使用 OnPush 策略。

当然，变更检测问题通常是由潜伏在周围的 bug 引起的，但是我们并不总是有等待修复的奢侈。

在这篇文章中，我将分享一个由[安东尼·米勒](https://twitter.com/nthonymiller/status/1283218988923682816)给出的技巧，来强制检测任何组件的变化；不管是你的还是第三方的。

# 强制变化检测

您可能知道，在角度应用中，我们可以使用[changededetorref](https://angular.io/api/core/ChangeDetectorRef)来强制检测变化，我们可以简单地将它注入我们的组件中。

ChangeDetectorRef 有多种方法，如`markForCheck`和`detectChanges`，可以做我们想做的事情。

正如我在我的书中所解释的，有一个与组件树平行的变更检测器树，所以每个组件实际上都有它自己的变更检测器。

问题是，取决于我们使用哪一个，我们或多或少地引起工作。例如，当应用程序组件使用它的 ChangeDetectorRef 强制执行更改检测时，它会在一棵大树上强制执行更改检测。

显然，建议只在您需要的特定组件上强制进行更改检测(理想情况下，没有，但现实生活中会发生)。

因此，如果某个特定的组件存在变更检测问题，那么我们实际上希望通过该特定组件的变更检测器来强制进行变更检测，以便它只为该组件(及其子组件)触发变更检测。

# 访问其他组件的注射器

要访问组件的 ChangeDetectorRef 实例，我们只需通过构造函数注入它:

```
constructor(private changeDetectorRef: ChangeDetectorRef) { ... }
```

一旦注射，我们可以简单地使用它。当然有一个陷阱:一个组件只能使用它自己的[注入器](https://angular.io/guide/dependency-injection) …或者你会这么想！

实际上有一种方法可以将另一个组件的注入器访问到目标组件的 [ViewContainerRef](https://angular.io/api/core/ViewContainerRef) :

在上面的例子中，`CoolComponent`使用了`Whatever`组件。为了访问`Whatever`组件的变更检测器实例，我们首先在模板中创建一个[模板引用变量](https://angular.io/guide/template-syntax)，然后我们使用 [ViewChild](https://angular.io/api/core/ViewChild) decorator 来获取该组件的`ViewContainerRef`。

最后，在我们组件的`ngAfterViewInit`方法中，我们简单地使用了组件的注入器，我们可以通过`ViewContainerRef`访问它来检索组件的`ChangeDetectorRef`。整洁！

# 结论

感谢[安东尼·米勒](https://twitter.com/nthonymiller)，现在我们的工具箱里又多了一个工具来帮助我们解决讨厌的变更检测错误，而不用求助于丑陋的黑客。

正如我在本文中所解释的，我们可以通过任何组件的`ViewContainerRef`来访问它的注入器，并使用它来检索它的`ChangeDetectorRef`(或者其他任何东西)。这样，我们可以非常精确地触发变更检测，并避免在不需要时导致大量的变更检测周期。

今天到此为止！

# 喜欢这篇文章吗？

如果你想了解关于软件/Web 开发、TypeScript、Angular、React、Vue、Kotlin、Java、Docker/Kubernetes 和其他很酷的主题的大量其他很酷的东西，那么不要犹豫[拿一本我的书](https://www.amazon.com/Learn-TypeScript-Building-Applications-understanding-ebook/dp/B081FB89BL)并订阅[我的时事通讯](https://mailchi.mp/fb661753d54a/developassion-newsletter)！