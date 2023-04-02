# 利用 TypeScript 3.8 的仅类型导入和导出

> 原文：<https://javascript.plainenglish.io/leveraging-type-only-imports-and-exports-with-typescript-3-8-5c1be8bd17fb?source=collection_archive---------1----------------------->

几个月前，我开始在 Webpack 上看到关于找不到出口的奇怪警告。

我很困惑。显然，有什么地方出错了，因为出口确实在那里:

我不能注意到我用那种字体的地方出了什么问题:

你能发现问题吗？

# 根本原因…

在没有任何线索的情况下，我挖了一会儿，无意中发现了下面这个问题:[https://github.com/webpack/webpack/issues/7378](https://github.com/webpack/webpack/issues/7378)

我有一种感觉，这与 TypeScript & transpilation 有关，因为我正在导入/使用 TypeScript 类型，但不清楚…

然后就咔嚓一声；[托拜厄斯·科珀斯立刻看到了](https://github.com/webpack/webpack/issues/7378#issuecomment-392264102)我没能理解的东西。我感到“惭愧”,因为我刚刚[出版了一本关于打字稿](https://www.amazon.com/Learn-TypeScript-Building-Applications-understanding-ebook/dp/B081FB89BL/ref=sr_1_4?dchild=1&keywords=typescript+3&qid=1587049779&sr=8-4) :D 的书

事情是，一旦类型脚本代码被编译，接口/类型就消失了。它们已经不存在了；它们不存在于*发出的*文件中。

我之前没有考虑太多的是，虽然类型被删除，但导入/导出不一定被删除。原因是存在歧义，编译器并不总是确切知道导出的东西是类型还是值。

在我的例子中，Webpack 在处理生成的 JavaScript 代码时看到的是一个并不存在的导入。

Webpack 是宽松的，所以它只发出一个警告，但问题确实存在，我无法控制在这种情况下 TypeScript 会生成什么。

# TypeScript 3.8 拯救世界

当时我没有太多时间来进一步研究这个问题，所以我让它休息了一段时间(它没有阻塞)，但是后来，一旦 TypeScript 3.8 发布，我注意到发行说明中有一些真正有趣的东西:[仅类型的导入和导出](https://devblogs.microsoft.com/typescript/announcing-typescript-3-8/#type-only-imports-exports)。

这直接提醒了我的进口问题…

TypeScript 的这一新特性增加了将元素仅作为类型导入的可能性，这正好适用于导入的类型仅用作类型而从不作为值的情况。这正是我所需要的；多亏了这一点，我终于可以准确地告诉编译器我想做什么。

要解决这个问题，我只需更换它

```
import { MyCustomExpressRequest } from "../../../shared";
```

通过以下方式:

```
import **type** { DidowiExpressRequest } from "../../../shared";
```

精彩！

但是这在实践中改变了什么呢？

嗯，正如 [TS 发布说明](https://devblogs.microsoft.com/typescript/announcing-typescript-3-8/#type-only-imports-exports)所解释的，通过使用“导入类型”来导入元素，它告诉编译器该元素只是被导入来用作类型注释/声明。由于这一点，编译器知道它可以删除发出的代码中的导入。

完成这一项更改后，Webpack 立刻变得更开心了:不再有奇怪的导入了！

请注意，您还可以使用“导出类型”来表示某些导出将只用作类型注释/声明。

最后，可以通过“importsNotUsedAsValues”标志进一步配置行为。

# 结论

TypeScript 仅类型的导入/导出是该语言的一个有用的补充，允许我们对 TypeScript 为我们做的事情进行更细粒度的控制，从而允许我们处理一些像这样令人讨厌的“边缘情况”。

不过，给你一个警告:不要做得太过火；-)

PS:如果你想学习大量关于 TypeScript、Angular、React、Vue 和其他酷主题的其他酷东西，那么不要犹豫，去[拿一本我的书](https://www.amazon.com/Learn-TypeScript-Building-Applications-understanding-ebook/dp/B081FB89BL)并订阅[我的简讯](https://mailchi.mp/fb661753d54a/developassion-newsletter)！

今天到此为止！