# Enums TypeScript 4.0 和 JavaScript 指南—您需要知道的一切

> 原文：<https://javascript.plainenglish.io/enums-typescript-4-0-and-javascript-guide-all-you-need-to-know-5e090355bff6?source=collection_archive---------2----------------------->

## 你将读到的关于 enums 的最后一本指南！

![](img/fc86be3dec8bfffb13cba4f13d8680e8.png)

[Black ice](https://www.pexels.com/@black-ice-551383), by [pexels](https://www.pexels.com/photo/blue-background-with-text-overlay-1314536/) (CC0)

JavaScript 知道 7 种数据类型。命名它们:**数字、字符串、布尔、对象、函数、符号**和**未定义**。如果你认真的话，你也可以数一数 **null** 。然后会有 8 个，但是我不指望这个，因为 null 是不可调用的。但是 enums 呢？它们在 JavaScript 里面吗？不，不幸的是没有。这是非常可悲的，因为我有 C#背景，枚举对于计数和符号编程来说是一件光荣的事情。它们允许您为常量表示赋值。也可以将一个值多重分配给不同的表示。但是不要失去希望，我们可以在 JavaScript 和剧透警报中实现相似性，Typescript 完全实现了它们！

# JavaScript 中的枚举？两种选择

在 JavaScript 中没有枚举，编码人员想出了独特的解决方案。我们可以把所有这些归纳成两个基本概念。第一个是定义数字的标识符:

如您所见，该方法基于一个对象，将显式允许的值保存为键，并将相应的数字保存为值。随后，您可以使用这些值。除此之外，你还可以读取和赋值这些值，它们是可比较的，并且有底层的名字。

JavaScript 中最大的缺陷是，当您想要读取属性值时，拼写错误的属性不会引发任何错误。在运行时，它只会返回值“未定义”。反之亦然，如果你想写入一个对象，却拼错了你想写入的属性，JavaScript 只是把拼错的属性作为“新”属性附加上去。

无论如何，这是一个巨大的问题。当你犯了这样一个错误，并且它最后在运行时引起了注意，你将有一段调试的美好时光。找出代码中的拼写错误。这个解决方案的主要缺陷是:JavaScript 中没有类型安全。为什么要试图从类型安全语言中复制一个构造，而在 JavaScript 中实现时，类型安全语言却不能兑现它的承诺呢？答案是想出一个更好的解决方案。

因此，我向你展示第二种方法。使用字符串而不是类似枚举的对象。

但是使用字符串也会给你带来缺点。关于允许值甚至可能值的概述是什么？使用这种技术时，这可能会失败。您可能会考虑混合使用第一种和第二种方法来替代枚举。

我们用字符串而不是数字设置了枚举“itemCategory ”,现在可以用枚举表示重新分配“weapon.category”。

我们保留了 enum 的已定义可能性的概述。当使用字符串时，这是不可能的。

# 对一个 enum 人来说很漂亮

我在第一章谈到了双重标记枚举值的可能性。要做到这一点，你需要一个技巧。将枚举值表示的数字定义为 2 的幂的基础。这样做可以将二进制组合器带到桌面上。

我建议你为你的用法设计一个默认值。通常使用的值是 0。也许我们现在可以双重赋值。例如，治疗药剂也可以是治疗物品，也可以用于制作(酿造)。为此，我们必须组合相应的值。

如果你坚持我在第一章中介绍的第二种选择，那么你就不能像我一样对你的属性进行双重标记。

但是你难道不认为这仍然是一个很好的解决方法吗？很聪明，但不如原来的 enum。我们现在来看看 Typescript。这是圣地，牛奶和蜂蜜流经的河流。

# TypeScript 中的枚举

TypeScript 将枚举作为类型，您可以通过关键字“enum”轻松地使用它们。

TypeScript 在这里的行为类似于 C#并从 0 开始计数。如果你不想这样，就用你最喜欢的数字分配第一个枚举。当数字不是有意义的表示时，你也可以依赖字符串。

*注意:该语言允许在同一个枚举中混合字符串和数字作为赋值。但是这款的实用性不是很好。每个枚举最好坚持一种类型的赋值。*

您也可以在 typescript 中实现枚举值的组合。确保有一个默认值(0)。

# 转换回 JavaScript？

尽管在使用 TypeScript 时，您不必再用 JavaScript 代码思考或编码，但浏览器已经做到了。因此，TypeScript 将被转换回 JavaScript，这是一个紧张的部分，看看它是如何做到的！

举这个例子:

将代码编译回 JavaScript，输出如下:

我们一步步深入了解这个庞然大物。第一步是在第 1 行创建一个名为`itemCategory`的对象。这将具有未定义的值。它将在下一步被传递给一个函数。
在这个函数体中，你会看到四个赋值，分别对应于我们已知的枚举表示法“`healing`”、“`crafting`”、“`armor`”、“`weapon`”。但是等号前面是什么？让我们详细看看第二行。

我们注意到，这是整个结构内部的属性的创造。`itemCategory[“crafting”]`向新创建的对象`itemCategory`(【X10 _ convertedtsenumintojs . js 的 *line1)追加属性`crafting`并将其与`itemCategory[“crafting”]=1`一起设置为 1。*

*注意:在 JavaScript 中，赋值将总是返回赋值后的值。这里，赋值 1 也是这个赋值的返回值。*

由于赋值的返回值，代码会立即创建另一个名为“1”的属性。该属性将获得值“crafting”。如果你仔细检查这一行，你会得出结论，这是一个双重任务。我们将属性 1 赋值为“crafting ”,并将属性“crafting”赋值为“1”。现在有两种方法来识别“手工制作”的相同表示:“1”和“1”:“手工制作”。编译后的代码会创建一个对象，如下所示:

这有助于显示枚举背后的值。

这个例子说明 Typescript 在这里没有做不同的事情。应该是怎样的？它必须被转换回 JavaScript。Typescript 所做的，让我们作为程序员的生活变得更好更安全。打字安全！它向我们介绍了一种不太容易出错的语言，在这种语言中，我们可以专注于生产和创建的任务，而不是穿过一个挑战，希望最好的，试图避免 JavaScript 为我们准备的每个陷阱。

这里要提到一点:const 也可以使用，然后进入 JavaScript 编译的严格模式。你失去了理解枚举表示的两种方法之一。您无法获得该值的名称。这个选项没了。当我们查看编译后的代码时，这一点变得很清楚。我们在这里得到了类型脚本中枚举。

然后编译回 JavaScript。

没有人期望:“使用严格”。枚举完全消失了。它去哪儿了？我们从枚举中进行赋值:

另一次检查编译:

就像我说的，以值取名和以名取值这两个方向都没有了。它只留下其中一个。号码。

# 还有其他方法吗？

是的，你可以使用所谓的“歧视联盟”。这只是一个“type”类型的变量，包含许多值中的一个。

因为不需要随时编写枚举类型，我们可以把它看作一个轻量级的替代方案。因为 JavaScript 与字符串一起工作，而且这种“有区别的联合”也是如此，所以您编写的代码更可能是 JavaScript。这就像处理字符串的经典方式，仅限于少数允许的字符串。您可以在没有 enum 构造的情况下做到这一点，而仅仅依靠“类型”，但是如果您想要使用数字，那么您必须手动转换回底层的常量表示。这个工作，一个 enum 提供给你的已经完成了。

# 结论

无论你想拿什么，在整个项目中保持一致。在一个项目中混合使用只会导致混乱和不断的反思。只采用一种方法也可以减少错误。为了找到适合你需求的东西，你必须权衡所介绍的方法的利弊，就像生活中经常发生的那样！

[***节省自己大量的时间，专注于重要的主题。***](https://arnoldcodeacademy.ck.page/26-web-dev-cheat-sheets)

# 继续读

[](https://medium.com/next-level-source-code) [## 下一级源代码

### 无论你想得到提高你的源代码的技巧和诀窍，还是对编程相关话题的第二种看法，这…

medium.com](https://medium.com/next-level-source-code) [](https://medium.com/next-level-source-code/do-you-follow-these-10-principles-for-good-programmers-1445727af447) [## 你遵循了优秀程序员的这 10 条原则吗？

### 从接吻和干燥到足球和 YAGNI 和聪明屁股代码的 10 个原则！

medium.com](https://medium.com/next-level-source-code/do-you-follow-these-10-principles-for-good-programmers-1445727af447) [](https://medium.com/next-level-source-code/javascript-es6-var-let-or-const-88da65f3a0df) [## JavaScript ES6 Var，Let 或 Const

### 掌握 JavaScript 中的变量，避免用 var，let，const 提升

medium.com](https://medium.com/next-level-source-code/javascript-es6-var-let-or-const-88da65f3a0df) 

# 参考

[1]文件清单[https://www.typescriptlang.org/docs/handbook/enums.html](https://www.typescriptlang.org/docs/handbook/enums.html)

[2] LogRocket — **使用类型脚本枚举编写可读代码**[https://blog . log rocket . com/Writing-readable-code-with-TypeScript-enums-a 84864 f 340 e 9/](https://blog.logrocket.com/writing-readable-code-with-typescript-enums-a84864f340e9/)

[3]git book—enum[https://basarat.gitbook.io/typescript/type-system/enums](https://basarat.gitbook.io/typescript/type-system/enums)