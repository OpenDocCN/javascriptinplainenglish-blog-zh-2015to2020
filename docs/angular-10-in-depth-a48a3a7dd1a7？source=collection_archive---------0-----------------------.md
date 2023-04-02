# 深度为角度 10

> 原文：<https://javascript.plainenglish.io/angular-10-in-depth-a48a3a7dd1a7?source=collection_archive---------0----------------------->

## Angular 的最新主要版本 Angular 10 刚刚发布。是时候发现新的东西了！

*   **更新#2** : Angular 12 出了。看看我关于它的文章:[https://JavaScript . plain English . io/angular-12-in-depth-7741 e 759 c 72](/angular-12-in-depth-7741e759c72)
*   **更新#1** : [角 11 出来了。查看我的文章，了解关于它的一切。](https://dsebastien.medium.com/angular-11-in-depth-9a7372b4a600)

![](img/248e6e622e8709349a265ea3e9255a66.png)

在本文中，我将(几乎)回顾这个全新版本中值得注意的一切。我还将重点介绍 Angular 的变化。

如果你想从直升机上看到包含的内容，那就去看看官方 Angular 博客。在这里，我将尝试更深入地研究发行说明。

Angular 10 已经在这里了，距离版本 9 仅仅四个月。当然，在这短暂的时间里，并没有太多的变化。尽管如此，除了这个版本带来的大量错误修复之外，还有相当多值得注意的特性。

提醒一下，Angular 团队试图每年发布两个主要版本，所以 Angular 11 应该会在今年秋天发布。

# 支持 TypeScript 3.9.x

我能说什么呢？你知道我的，[我喜欢打字稿](https://www.amazon.com/gp/product/B081FB89BL)。所以这个 Angular 版本让我高兴的第一件事是它支持 [TypeScript 3.9](https://medium.com/@dSebastien/whats-new-with-typescript-3-9-9095ff9f68a5) 。

我已经发表了一篇关于 TS 3.9 的[新特性的文章，所以如果你没有看过的话，](https://medium.com/@dSebastien/whats-new-with-typescript-3-9-9095ff9f68a5)[继续吧](https://medium.com/@dSebastien/whats-new-with-typescript-3-9-9095ff9f68a5)尽快升级，真的很值得！我还写了另一篇关于 TypeScript 4.0 的文章。

注意 Angular 10 已经掉了对 TS 3.6，3.7，3.8 的支持！我希望这不会阻碍你。

由于支持 TS 3.9.x 和编译器 CLI 中的其他改进，Angular 10 中的类型检查比以往任何时候都快，这对大多数项目来说都是积极的；尤其是大的。

除此之外，Angular 10 还升级到了 [TSLib 2.0](https://github.com/microsoft/tslib) 。对于那些不知道的人来说，TSLib 是一个官方库，它提供了可以在运行时使用的 TypeScript helper 函数。TSLib 与“tsconfig.json”的`importHelpers`标志结合使用；启用时，它允许编译器生成更精简/可读的代码。不管怎样，没什么好担心的；TSLib 变化不大..

# 可选的更严格的设置

胜利的严格模式！

Angular 10 带来了在创建时创建更严格的项目的可能性，这很好，当然应该用于所有的新项目。要创建具有更严格默认值的项目，请使用:

```
ng new --strict
```

这将允许您更快地发现问题(在构建时发现错误比在运行时更好，对吗？).

这个新选项启用了 TypeScript 严格模式(您应该在您的项目中启用它！).

除此之外，它还支持严格的[角度模板类型检查](https://medium.com/javascript-in-plain-english/angular-template-type-checking-e2c99c50f999)，我在上周[写了这个](https://medium.com/javascript-in-plain-english/angular-template-type-checking-e2c99c50f999)。

它还大幅降低了“angular.json”的预算:

这很好，因为它将鼓励新用户关注他们的应用程序的捆绑大小(关于这一点，我正在计划一篇关于如何分析你的应用程序的捆绑大小的文章)。

它还实施了更严格的 TSLint 配置，禁止“any”(“no-any”被设置为`true`)，并且还启用了由 [codelyzer](http://codelyzer.com/rules/) 提供的相当多的有趣规则。请注意，即使是严格的，您仍然可以使用 TSLint 走得更远。例如，[这是我的一个项目](https://gist.github.com/dsebastien/19acb20bf30fc2f4a09b60d974f7097c)的配置，你可以用它作为起点。

我认为这个新的“严格”选项很棒，但我有点难过，它不是默认的，而是一个可选的标志。我觉得更严格意味着更安全，那为什么要让更安全可选呢？我想基本原理是默认情况下更加宽松，Angular 起初感觉不那么可怕？

不管怎样，如果你创建了一个新的项目，请启用它，甚至更进一步；你以后会感谢我的。

# 新的 TypeScript 配置布局

在这个新版本中，新项目中默认提供的 TypeScript 配置发生了变化。现在除了“tsconfig.json”、“tsconfig.app.json”和“tsconfig.spec.json”之外，还有一个“tsconfig.base.json”文件。

那么为什么会有这些配置文件呢？为了更好地支持 ide 和构建工具查找类型和编译器配置的方式。

在新的设置下，“tsconfig.json”仅仅包含了基于所谓的[“解决方案样式”](https://devblogs.microsoft.com/typescript/announcing-typescript-3-9/#solution-style-tsconfig) (Visual Studio 又回来了？:p)由 TypeScript 3.9 带来，这对于缩短编译时间和在项目的各个部分之间实施更严格的分离非常有用:

在这种情况下，分离是为了将应用程序代码(由“tsconfig.app.json”处理)与测试(由“tsconfig.spec.json”处理)完全隔离开来。

如果您查看“tsconfig.base.json”文件，您会发现大部分的 TypeScript 配置:

注意，这个是使用上一节讨论的`strict`选项生成的。

从上面可以看到，这个文件只配置了 TypeScript 编译器和 Angular 编译器选项；它没有列出/包含/排除要编译的文件。

答案确实在“tsconfig.app.json”文件中，该文件列出了“main.ts”和“polyfills.ts”:

如果您有一个没有这种布局的现有项目，那么您可能应该检查您的 TypeScript 配置，以便保持一致并从中受益

好了好了，关于 TypeScript 配置已经说得够多了。

# NGCC

如果您还没有这样做(NG9 已经这样做了)，请确保您的“package.json”文件中有一个`postinstall`脚本，以便在安装后立即执行 NGCC:

注意，在这个版本中，NGCC 比 T7 更有弹性。以前，当它的一个工作进程崩溃时，它总是不能恢复。因此，如果你有时看到 NGCC 悬挂的问题，现在应该是固定的。

NGCC 也有相当多的改进，包括与性能相关的，这显然是我在 NGCC 最大的痛点；-)

# 新的默认浏览器配置

网络浏览器的速度比以往任何时候都快。Angular 跟随航向，现在使用更新的[浏览器列表](https://github.com/browserslist/browserslist)文件(。browserslistrc)。

正如官方博客中解释的那样，新配置的副作用是新项目默认禁用 ES5 构建。

当然，此时生成 ES5 代码已经没有多大意义了。现代网络浏览器至少支持 ES2015。如果你还在用 ie，那么显然是时候放下过去了！

要获得支持的 Web 浏览器的准确列表，只需在项目中执行以下命令:

```
npx browserslist
```

输出是基于"的内容生成的。browserslistrc "文件的根目录；默认情况下，它现在看起来如下:

你可以在这里找到更多关于这个[的信息。](https://angular.io/guide/browser-support)

# 巴泽尔

抱歉让你失望了，但是你知道吗[安格尔·巴泽尔已经离开了安格尔实验室](https://dev.to/bazel/angular-bazel-leaving-angular-labs-51ja)？Alex Eagle [不久前在 dev.to](https://dev.to/bazel/angular-bazel-leaving-angular-labs-51ja) 上写了这篇文章。基本上，对 Bazel 的支持不再是 Angular 项目的一部分。

Bazel 永远不会成为 Angular CLI 中的默认构建工具…

我不会在这里重复原因，但是一定要看看 Alex 的文章，因为它非常有趣(像往常一样)。

# @ angular-dev kit/build-angular 0 . 1000 . 0)

这个野蛮名字的背后(还有版本？？？！)，隐藏了 Angular 应用构建方式的一个重要部分。

这个软件包的最新版本给我们带来了一些很酷的新功能。

最酷的一点(如果你使用 SASS 的话)是 build-angular 现在将资产的相对路径重新排序。

正如在[提交](https://github.com/angular/angular-cli/commit/c034477dc5e64259fa1cff23a8d0646748a49521)中所述，以前，在样式表中引用并在其他样式表中导入的类似`url(./foo.png)`的路径将保留确切的 URL。这是有问题的，因为一旦导入的样式表不在同一个文件夹中，它就会中断。现在，将找到所有使用相对路径的资源。酷！

该版本中另一个隐藏的亮点是 build-angular 现在[对](https://github.com/angular/angular-cli/commit/a78d1c3ed14350bbe85bbf01ab30bcf6ad319f29) [Webpack 不能处理的](https://github.com/webpack/webpack/issues/5593)重复模块进行重复数据删除。这是通过一个定制的 Webpack 解析插件完成的。

# 还有更多…

## 增量模板类型检查

在这个版本中，编译器 CLI 现在能够执行[模板类型检查](https://medium.com/javascript-in-plain-english/angular-template-type-checking-e2c99c50f999) [增量](https://github.com/angular/angular/commit/ecffc35)。希望这将拯救相当多的树木(也许还有一两台笔记本电脑)！:)

## 罐头装载

以前`CanLoad`守卫[只能返回布尔](https://github.com/angular/angular/issues/28306)。现在，有可能返回一个`UrlTree`。这与`CanActivate`警卫的行为相符。

请注意，这不会影响预加载。

## I18N/L10N

以前，每个语言环境只支持一个翻译文件。现在，可以为每个地区指定多个文件。然后，所有这些都按照消息 id 进行合并。

关于这个我不能说太多，因为这些天我只使用 ngx-translate & transloco……查看这个[问题](https://github.com/angular/angular/pull/36792)了解更多细节。

## 服务人员

默认`SwRegistrationStrategy`已改进。以前，存在服务工作者[从未注册](https://github.com/angular/angular/issues/34464)的情况(例如，当有长时间运行的任务，如间隔和重复超时)。

同样，我不能多说什么，因为我使用的不是 NGSW 而是 Workbox 。

## 角状材料

像往常一样，Angular Material 的发布遵循 Angular 的发布，所以 Angular Material 10 在这里，有很多变化。

我不会在这篇文章中重复这些，因为它已经很长了，所以如果你感兴趣的话，去[看看发布说明](https://github.com/angular/components/releases)！

# 大量错误修复

正如几周前提到的，Angular 团队已经在 bug 修复和积压整理上投入了大量的时间和精力。他们已经减少了 700 期，这是相当可观的。

如果你是 Angular 以前版本中已知错误的受害者，那么可能是时候看看 Angular 10 是否没有修复这些错误了。

有趣的是，启用[严格模板类型检查](https://medium.com/javascript-in-plain-english/angular-template-type-checking-e2c99c50f999)会导致 routerLinks 出现问题，因为它们的底层类型不包含空/未定义的。另一个被修理的是`KeyValuePipe`，它[没有很好地配合](https://github.com/angular/angular/issues/35743)管子`async`。

当我们讨论模板时，请注意 Angular [的语言服务现在支持](https://github.com/angular/angular/pull/36312)更多类似数组的对象，例如`ReadonlyArray`和`readonly`属性数组用于`*ngFor`循环。多酷啊。:)

# 折旧和移除

正如官方博客文章中所述，之前作为 [Angular Package Format](https://g.co/ng/apf) 一部分的 ESM5/FESM5 捆绑包现在已经不存在了，因为向下升级到 ES5 已经在构建过程的最后完成了。如果您不使用 Angular CLI 来构建您的应用程序/库，并且仍然需要 ES5 包(可怜的灵魂..)，那么你就需要自己把角度代码降级到 es5。

不再支持 IE 9、10 和 Internet Explorer Mobile。但是，如果你问我，你应该在这一点上完全抛弃 IE。把丧尸留在身边简直是扯淡。

有相当多的弃用成分，如带有反应形式的`ReflectiveInjector`、`CollectionChangeRecord`、`DefaultIterableDiffer`、`ReflectiveKey`、`RenderComponentType`、`ViewEncapsulation.Native`、`ngModel`、`preserveQueryParams`、`@angular/upgrade`、`defineInjectable`、`entryComponents`、`TestBed.get`等。

你可以在这里查看完整的列表。

# 不再支持使用不带角度装饰器的角度特征的类

直到版本 9，在没有指定任何装饰器(@Component、@Directive 等)的情况下，拥有一个使用角度特性的类是可以的。

在 Angular 10 中，如果一个类使用角度特性，现在必须添加一个角度装饰器。这一变化影响了所有从基类扩展的组件，并且两者之一(即父类或子类)缺少角形装饰的情况。

为什么这种改变是强制性的？简单来说，因为常春藤需要！

当一个类上没有 Angular decorator 时，Angular 编译器不会为依赖注入添加额外的代码。

正如[官方文档](https://next.angular.io/guide/migration-undecorated-classes)中所述，当父类中缺少 decorator 时，子类将从编译器没有为其生成特殊构造函数信息的类中继承一个构造函数(因为它没有被修饰为指令)。当 Angular 试图创建子类时，它没有正确的信息来创建它。

在视图引擎中，编译器具有全局知识，因此它可以查找丢失的数据。然而，Ivy 编译器只单独处理每个指令。这意味着编译可以更快，但编译器不能像以前一样自动推断出相同的信息。添加`@[Directive](https://next.angular.io/api/core/Directive)()`明确地提供了这个信息。

当子类缺少装饰器时，子类从父类继承，但没有自己的装饰器。没有装饰器，编译器无法知道这个类是`@[Directive](https://next.angular.io/api/core/Directive)`还是`@[Component](https://next.angular.io/api/core/Component)`，所以它不会为指令生成正确的指令。

这个变化的好处是它给角世界带来了更多的一致性(一致性是好的:p)。现在事情很简单:如果你使用角度特征，那么你必须添加一个装饰器。

举个例子，下面的代码不能用 Ivy 编译:

要解决这个问题，您需要向`Base`类添加一个装饰器。

你可以在这里了解更多关于这个变化的[。](https://next.angular.io/guide/migration-undecorated-classes)

# ModuleWithProviders 的强制泛型类型

在以前的版本中， [ModuleWithProviders](https://angular.io/api/core/ModuleWithProviders) 已经接受了一个泛型类型，但是它不是强制的。对于 NG 10，需要泛型参数。

无论如何，这对类型安全来说是件好事，所以希望您已经定义了参数:

如果您因为正在使用的库而偶然发现以下错误:

```
error TS2314: Generic type 'ModuleWithProviders<T>' requires 1 type argument(s).
```

然后你应该联系库的作者来修复它，因为 ngcc 在那里帮不上忙。一个解决方法是将`skipLibChecks`设置为假

# 其他突破性变化

以下是值得注意的突破性变化:

*   [解析器](https://angular.io/api/router/Resolve)表现不同；那些返回`EMPTY`的现在会取消导航。如果您想让导航继续，那么您需要确保您的解析器发出一个值；例如使用`defaultIfEmpty(...)`、`of(...)`等
*   依赖于带有`Vary`头的资源的服务工作者实现将不会像以前那样工作。将忽略 Vary 标头。建议的“解决方案”是避免缓存这样的资源，因为它们往往会根据用户代理导致不可预测的行为。因此，即使资源的标头不同，也可以检索资源。注意[缓存匹配选项](https://github.com/angular/angular/pull/34663)现在可以在 NGSW 的配置文件中配置
*   如果`fubar`值与前一个值相同，诸如`[foo]=(bar$ | async).fubar`的属性绑定将不会触发变化检测。如果您依赖前面的行为，解决方法是手动订阅/强制更改检测或修改绑定，以确保引用确实发生了更改
*   以下`formatDate()`和`DatePipe`的格式代码发生变化；显然，前面的行为对于午夜过后的白天是不正确的
*   位于`UrlMatcher` [实用程序类型](https://angular.io/api/router/UrlMatcher)(函数别名)之后的函数现在可以正确地声明其返回类型可能是`null`。如果您有一个自定义的路由器或识别器类，那么您需要修改它们
*   额外出现的`ExpressionChangedAfterItHasBeenChecked`现在可以由 Angular 引发，因为它以前没有检测到错误
*   Angular 现在会在发现模板中的未知元素/属性绑定时记录错误级别。这些是以前的警告
*   反应式表单的`valueChanges`有一个绑定到类型`number`输入的表单控件的 bug([他们自 2016 年以来触发了两次](https://github.com/angular/angular/issues/12540)！第一次是在输入域中键入之后，第二次是当输入域失去焦点时)。现在，数字输入不再听`change`事件，而是听`input`事件。不要忘记相应地调整你的测试。注意，这破坏了 IE9 的兼容性，但这对任何人来说都不是问题..对吗？；-)
*   `minLength`和`maxLength`验证器现在确保关联的表单控件值有一个数字`length`属性。如果不是这样，那么这些就不会被验证。以前，没有`length`属性(例如`false`或`0`)的 falsy 值会触发验证错误。如果您依赖于该行为，那么您应该添加其他验证器，如 [min](https://angular.io/api/forms/Validators#min) 或 [requiredTrue](https://angular.io/api/forms/Validators#requiredTrue)

# 升级

像往常一样，有一个完整的[升级指南可用](https://update.angular.io/#9.0:10.0l3)和 ng 更新将帮助你:[https://update.angular.io/#9.0:10.0l3](https://update.angular.io/#9.0:10.0l3)

如果你手动升级并且仍然使用量角器(以防万一)，那么不要忘记将量角器更新到 7.0.0+,因为以前的版本有一个[漏洞](https://www.npmjs.com/advisories/1500)。

# 结论

在这篇文章中，我探索了 Angular 10 的新特性，以及不赞成、删除和突破性的变化。

总而言之，即使这不是一个惊天动地的版本，它显然是一个坚如磐石的版本，有大量的错误修复和一些宝石。

像往常一样，我们只能感谢 Angular 团队和围绕它的社区所做的一切努力！

今天到此为止。

# 喜欢这篇文章吗？

如果你想了解关于软件/Web 开发、TypeScript、Angular、React、Vue、Kotlin、Java、Docker/Kubernetes 和其他很酷的主题的大量其他很酷的东西，那么不要犹豫[拿一本我的书](https://www.amazon.com/Learn-TypeScript-Building-Applications-understanding-ebook/dp/B081FB89BL)并订阅[我的简讯](https://mailchi.mp/fb661753d54a/developassion-newsletter)！