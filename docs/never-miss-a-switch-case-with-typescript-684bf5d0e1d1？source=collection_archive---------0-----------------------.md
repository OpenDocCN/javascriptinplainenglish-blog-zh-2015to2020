# 使用 TypeScript，不要错过任何一个开关

> 原文：<https://javascript.plainenglish.io/never-miss-a-switch-case-with-typescript-684bf5d0e1d1?source=collection_archive---------0----------------------->

Switch 语句有时被认为是一种代码味道，但是当它们确实有意义使用时，你最好确保你没有忘记一个*案例*。幸运的是，TypeScript 可以提供帮助。

![](img/f2dd906fdc38078545f3d28c5dc09933.png)

Image courtesy of [Kelly Sikkema](https://unsplash.com/@kellysikkema)

在本文中，我不会讨论为什么您应该在应用程序中避免 switch 语句(例如，它们可能很容易违反 DRY 和 Open/Closed 原则，如果您忘记了 break，可能会引入微妙的错误，等等)。众所周知，switch 语句经常导致代码不稳定。

我也不会解释编写 switch 语句的替代方法(例如，策略模式、命令模式、高阶函数等)。

相反，我会假设您知道自己在做什么，并且 switch 语句对于您的用例来说是一个很好的解决方案。

我将向您展示如何利用 TypeScript 的`never`类型来确保您确实涵盖了所有可能的情况，例如在针对枚举使用开关时。

# 出发点

让我们假设我们有下面的枚举作为起点:

上面的枚举描述了“某物”的状态。目前只有几种可能的状态。

接下来，我们有以下枚举:

这一部分描述了应用程序中不同的用户角色。

现在，假设我们需要创建一个基本的权限系统，根据一组标准确定谁可以做什么，一个是元素的状态，另一个是用户的当前角色。

随着应用程序的发展，枚举可能也会发展，需要我们随着时间的推移来调整规则。

# 不安全的解决方案

如果你的目标是定义一个函数来决定用户是否可以编辑一个元素，你可以这样开始:

这看起来*有点*干净，因为它确实涵盖了此时所有可能的情况**。**

你能找出上面代码中最大的问题吗？事实上有很多，但是我想集中讨论一个:如果一个新的 ElementState 被添加到`ElementState`枚举中会怎么样？

问题是，上面的代码将继续编译良好，因为有一个默认的情况下，将击中，如果你没有在任何以前的情况。这可能会引入错误，因为默认情况下总是返回`false`，这可能不是您想要的新状态。注意，对于另一个枚举来说也是如此，但是现在让我们忽略它。

那么我们能做些什么来避免这种情况呢？马上，您可能想用 throw 语句替换默认情况下的`retVal = false`，但是这在编译时没有任何帮助。

编译器真的能帮助我们吗？

# 打字稿从不打字来拯救

好消息是，TypeScript 可以帮助我们使编码人员更加安全。 [TypeScript 的基本类型](http://typescriptlang.org/docs/handbook/basic-types.html)之一叫做 [never](https://www.typescriptlang.org/docs/handbook/basic-types.html#never) 。`never`类型可以用来表示*永远不会*出现的值。

你可能想知道这有什么帮助？

感谢`never`类型，我们可以向编译器解释，我们永远不应该在 switch 语句的默认情况下结束。让我告诉你怎么做。

首先，让我们创建一个永远不应该用实际值调用的函数:

如您所见，上面的函数接受了一个类型为`never`的参数。实际上，这个函数永远不能用非`never`类型的参数(即实际值)调用。

让我们重温一下我们的函数，以利用我们超级有用的函数:

我们现在调用 switch 的默认子句中的`assertUnreachable`函数。目前，`ElementState`枚举的每个可能值都有一种情况。这段代码编译得很好，因为默认情况可能永远不会出现(因为我们通过这些情况使用了所有可能的值)。

现在，如果我们修改`ElementState`枚举并向其添加一个新元素，上面的代码将不再编译，直到我们为新的可能值添加一个 case。

现在我们有了一个更安全的 switch 语句版本。

# 结论

在本文中，我分享了一个`never`类型的 TypeScript 支持的小技巧。多亏了我们的`assertUnreachable(never)`函数，我们能够编写一个详尽的 switch 语句，我们确信编译器会在我们忘记修改代码的时候告诉我们，以防后备枚举发生变化。

这种模式通过确保涵盖所有可能的情况，极大地提高了编译时的安全性。

当然，小心使用 switch 语句的一般建议仍然存在；不要在你的代码中滥用它们。但是如果你确实需要使用它，请安全地使用它！:)

今天到此为止！

PS:如果你想学习大量关于软件/Web 开发、TypeScript、Angular、React、Vue、Kotlin、Java、Docker/Kubernetes 和其他很酷的主题的其他很酷的东西，那么不要犹豫[去拿一本我的书](https://www.amazon.com/Learn-TypeScript-Building-Applications-understanding-ebook/dp/B081FB89BL)并订阅[我的简讯](https://mailchi.mp/fb661753d54a/developassion-newsletter)！

## **用简单英语写的便条**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页 [**plainenglish.io**](https://plainenglish.io/) 找到所有这些内容——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**