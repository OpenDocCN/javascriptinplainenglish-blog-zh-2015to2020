# 使用 ngx-translate 动态翻译菜单

> 原文：<https://javascript.plainenglish.io/dynamically-translating-a-primeng-menu-using-ngx-translate-4e758eb5aee0?source=collection_archive---------2----------------------->

最近，为了我们的*令人敬畏的*启动项目，我们决定远离棱角分明的材料，转而使用[顺风](https://tailwindcss.com/) + [顺气](https://www.primefaces.org/primeng/)。

采取这一举措的原因是，我们设计师的工作感觉与材料设计相去甚远，我们不愿意逆流而行。我们觉得覆盖所有有棱角的材质风格并不容易，而且会很粗糙。

作为向 Prime 过渡的一部分，我们必须重写我们的 [mat-menu](https://material.angular.io/components/menu/overview) 用法，以 PrimeNG 的“等价”[菜单组件](https://www.primefaces.org/primeng/showcase/#/menu)。

在这篇文章中，我将解释我们如何成功地动态翻译 PrimeNG 菜单。

# 问题是

Prime 的菜单系统的好处和坏处在于它有一个强大的[菜单模型](https://www.primefaces.org/primeng/showcase/#/menumodel)作为后盾。

这个模型的好处在于，我们可以静态地定义菜单的配置，将它作为输入传递给 p-menu，让它为我们呈现一切。

不太好的是，我们对菜单项如何呈现的控制非常有限，这与 Angular Material 的 mat-menu 组件相反，它使用内容投影，因此让我们可以轻松地将我们想要的东西插入菜单。

这至少在国际化的环境中是有问题的，因为我们传递给菜单的标签不是动态的。它们只设置一次，不会自动更新。真扫兴。

有了 mat-menu，我们只需使用 ngx-translate 的翻译管道，就可以很好地工作。对于 PrimeNG 的菜单，我们不得不求助于 translateService.instant(…)，它只做一次性翻译，不会自动更新。

以下是我们的菜单转换成 PrimeNG 后的样子:

和模板:

问题是，有了这个，菜单翻译在构建时(在 ngOnInit 期间)只被插入*一次*，但是当语言改变时，这些不会被重新评估。真扫兴。

# 解决方案

经过一番调查，只有两个解决方案脱颖而出:

*   Fork PrimeNG 的菜单来按照我们的意愿弯曲它，并且能够直接对菜单模型中提供的标签使用 translate 管道
*   每当语言发生变化时，重新构建菜单模型，使其包含正确的翻译文本

现在，我们从第二个选项开始。出于其他原因，第一种方法可能更好，但也有问题，因为它会引入技术债务。

所以我们的计划是:订阅语言变化事件，并在每次语言变化时重建整个菜单。

在我们的应用程序中，我们将当前语言存储在 NGRX store 中，我们的 facade 简单地公开了一个选择器来让我们观察值的变化，这使事情变得非常简单，但是同样的事情可以通过使用由 [ngx-translate](https://github.com/ngx-translate/core) 的 TranslateService 服务公开的 *onTranslationChange* 事件发射器来实现:

有了它，每当语言发生变化时，我们都会得到警告。当这种情况发生时，我们需要重建菜单。请注意，上面的示例代码没有正确处理订阅；但是不要忘记在应用程序中使用它，否则会导致内存泄漏；-)

为了更容易地做到这一点，我们简单地将菜单模型的构造代码移到了工厂方法中:

每当调用语言更改回调时，我们只需调用我们的 buildUserProfileMenu 方法来获得带有正确翻译的新版本菜单。

下面是代码的概述:

问题解决了！

# 结论

在这篇小文章中，我描述了我们如何处理新 PrimeNG 菜单的国际化。

我对我们不得不这样做并不满意，但这是我能找到的最干净的方法。

今天到此为止！

# 喜欢这篇文章吗？点击下面“喜欢”按钮查看更多内容，并确保其他人也能看到！

PS:如果你想学习大量关于软件开发、Web 开发、TypeScript、Angular、React、Vue、Kotlin、Java、Docker/Kubernetes 和其他很酷的主题的其他很酷的东西，那么不要犹豫去[拿一本我的书](https://www.amazon.com/Learn-TypeScript-Building-Applications-understanding-ebook/dp/B081FB89BL)并订阅[我的简讯](https://mailchi.mp/fb661753d54a/developassion-newsletter)！

## 简明英语笔记

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**此外，我们总是有兴趣帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****