# 如何在 Angular 应用程序中选择 constructor 和 ngOnInit

> 原文：<https://javascript.plainenglish.io/how-to-choose-between-constructor-and-ngoninit-in-your-angular-apps-f16987627312?source=collection_archive---------2----------------------->

有些事情你可以在类的构造函数中做，而不是在 ngOnInit 生命周期方法中。我来告诉你是哪些，为什么。

![](img/865a3b905d70e2a5251291379ebfa74d.png)

最近，我读了的一篇文章，文章最后建议*总是使用 ngOnInit 方法进行初始化。作为一个打字爱好者，我不敢苟同，或者至少在这个问题上给出另一种观点。*

# *关于 ngOnInit*

*OnInit 接口是 Angular 提供的最有用的生命周期钩子之一。您可以在组件/指令等中使用它来反应/执行特定的初始化任务。*

*为了遵守契约，您必须在您的类中实现一个 [ngOnInit](https://angular.io/api/core/OnInit#ngOnInit) 方法。Angular 将在初始化所有数据绑定属性后立即调用此方法。更准确地说，在第一个 [ngOnChanges](https://angular.io/api/core/OnChanges) 调用之后调用 ngOnInit(如果没有输入绑定，也是如此)。*

*这意味着当调用 ngOnInit 方法时，您确信您已经拥有了完全初始化您的类所需的一切。*

*乍一看，ngOnInit 似乎很适合做所有的初始化工作。对于*组件*的初始化来说，它确实是完美的，但是有一个基本的问题需要记住:类型安全。*

*让我们回到 TypeScript 基础来弄清楚这一点。*

# *关于构造函数*

*构造函数只为初始化而存在。它们包含实例化类时将执行的第一行代码，然后才能调用任何其他方法:*

*构造函数的主要作用确实是确保类的所有字段都被正确初始化。如果我们忘记了抽象类，那么非静态或在声明时直接初始化的字段必须要么在构造函数中初始化，要么标记为可选的，要么用[明确赋值断言](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-7.html#definite-assignment-assertions)(即`!`)操作符标记。我会回到这个话题。*

*另外，请注意，如果字段在声明时或在构造函数中被初始化，那么它们只能被标记为只读。*

# *TypeScript 严格属性初始化*

*我对我的关于打字稿的书[的一个遗憾是没有给严格的打字稿模式更多的空间。回想起来，我觉得我应该在这里或那里减少几页来更详细地介绍它，但由于它达到了 800 页，我的出版商并不太热衷于增加更多的页面；-)](https://www.amazon.com/gp/product/B081FB89BL)*

*启用 [TypeScript 的严格模式](https://mariusschulz.com/blog/the-strict-compiler-option-in-typescript)实际上是你可以在你的代码库上做的最好的事情之一，以使它更加健壮。*

*在这个标志后面，实际上有不同的子标志，当`strict`选项设置为`true`时，这些子标志都被启用。你可能没有那么大胆，只启用/禁用其中一些，但是我的建议是**大胆地启用所有的功能！***

*其中一面旗帜叫做`strictPropertyInitialization`。顾名思义，[严格属性初始化](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-7.html#strict-class-initialization)确保类的每个实例属性在构造函数中或者由属性初始化器初始化。这是一个很好的选择，因为它有助于消除一整类愚蠢的错误。*

# *ngOnInit 如何损害类型安全*

*如果您处于严格模式，或者至少启用了严格属性初始化，则以下示例不会编译:*

*这将因以下情况而失败:*

```
*TS2564: Property 'foo' has no initializer and is not definitely assigned in the constructor.*
```

*这段代码不能在严格模式下编译，因为`foo`字段初始化*太晚*。对我来说，这真的是`ngOnInit`对于类型安全的关键问题。正如我在上一节中所说的，有一些方法可以解决这个问题；一些比另一些更好。*

# *替代品概述*

*正如我们之前看到的，通过`ngOnInit`方法初始化字段对于类型安全是有问题的。我们已经看到了如何让前面的代码*编译*。*

*首先，你可以使用一个初始化式:*

*这是理想的，因为它不依赖于任何初始化。您应该尽可能使用这种方法。不幸的是，如果初始化依赖于其他“东西”，那么就不可能使用这种方法。*

*第二种选择是在构造函数中进行初始化:*

*另一种方法是使用明确的赋值操作符:*

*最后，还可以将`null`或`undefined`添加到允许值中:*

*例如，这对于角度分量输入很有用，但会强制进行全面的显式检查，并允许您在字段中返回 null 或 undefined，这可能不是最好的主意。*

*还有其他变体(例如，通过构造函数参数的声明/初始化)，但是让我们忽略它们。*

# *用什么，什么时候用？*

*当考虑替代方案时，您可能会认为明确的赋值操作符是最好的解决方案，因为它允许总是通过`ngOnInit`方法进行后期绑定。说实话，这只是你对编译器的一个*无力的*承诺:“嘿，别担心，我会在某个时候初始化它，相信我……”(著名的遗言)。*

*当您使用明确赋值运算符时，您告诉 TypeScript 根据需要处理字段，但不再检查初始化。这是危险的，因为你不能确定它是否被初始化，如果你没有这样做，那么 TypeScript 将不会帮助你；您只能通过测试来发现它(无论是自动化测试还是最终用户因为有问题而打电话给您)。*

*一般的建议，甚至得到了 Misko Hevery 的支持，是避免在构造函数中做繁重的初始化工作。主要的一点是，它会使你的类不灵活，更难测试。*

*就我个人而言，我更喜欢通过构造函数初始化任何我能初始化的东西，只要它是可能的并且不会使测试变得比它应该的更复杂。*

*例如，如果我需要订阅一些可以直接从构造函数中访问的可观察对象，那么我会这样做。*

*类似地，对于反应式表单，如果表单初始化不依赖于由`ngOnInit`方法提供的输入，我也倾向于使用构造函数。*

*如果初始化需要与协作者进行太多的交互(从而使测试更加困难)，依赖于组件输入，或者需要通过 ViewChild 等与 DOM 进行交互，那么我接受进行后期绑定。请记住，类初始化和组件初始化是分开的事情。前者一般应在构造函数中处理；后者在 ngOnInit 方法中，除非它可以在构造函数中更安全地完成。*

*这样做的时候，我通常更喜欢扩大字段的类型以包含`null`或`undefined`，而不是使用明确的赋值操作符。这样，我就能在编译时让代码更安全，即使这比告诉编译器保持沉默更烦人。我确实不时地使用明确赋值操作符(当我真的不想允许 null 或 undefined 的时候)，但是我不太喜欢它…*

# *结论*

*在这篇文章中，我试图让你了解为什么`ngOnInit`可能不总是*是做初始化工作的最好地方。这有点争议，当然也不是“100%的时间都这样做”的建议。尽管如此，我认为在选择初始化某些东西的位置时，适当地考虑利弊是很重要的。**

**今天到此为止！**

***PS:如果你想了解大量关于软件/Web 开发、TypeScript、Angular、React、Vue、Kotlin、Java、Docker/Kubernetes 和其他很酷的主题，那么不要犹豫，去* [*拿一本我的书*](https://www.amazon.com/Learn-TypeScript-Building-Applications-understanding-ebook/dp/B081FB89BL) *并订阅* [*我的简讯*](https://mailchi.mp/fb661753d54a/developassion-newsletter) *！***

# ****用简单英语写的便条****

**你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页[**plain English . io**](https://plainenglish.io/)上找到所有这些——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！****