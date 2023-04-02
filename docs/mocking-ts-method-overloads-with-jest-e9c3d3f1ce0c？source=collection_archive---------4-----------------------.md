# 用 Jest 嘲弄 TS 方法重载

> 原文：<https://javascript.plainenglish.io/mocking-ts-method-overloads-with-jest-e9c3d3f1ce0c?source=collection_archive---------4----------------------->

用 Jest 模仿重载的 TS 方法本身并不难，但是您必须知道如何去做。

![](img/bb00300f0859e1f8b5edfecd7a650557.png)

Picture courtesy of [Ammar Sabaa](https://unsplash.com/@ammar_sab3)

在本文中，我将解释这个“问题”以及如何解决它。

# “问题”

对于习惯于面向对象编程的开发人员来说，方法重载是一个非常熟悉的概念(顺便说一下，这是我将在我即将出版的书[中解释的众多概念之一)。](http://dev-concepts.dev/)

使用方法重载，您可以定义多个名称相同但签名不同的方法/函数。重载可以很好地使用，因为具体的实现是根据您传递的参数来调用的。

当然，方法重载是一个有用的概念，它本身根本不是一个问题。虽然在测试时，用 TypeScript 模拟重载方法可能有点困难。

问题是当我们要求 TypeScript 给我们一个重载方法的参数时，我们得到的是最后一个重载的参数。因此，我们可能会遇到编译器抱怨模拟实现不接受正确的参数或不返回预期类型的问题。是`ReturnType`和条件类型的限制之一。

一开始这可能会有点混乱。

问题当然是:如何告诉 TypeScript 我们想要模仿这个或那个版本的函数？

# 一个例子

这里有一个例子来说明这个问题。

在我当前的项目中，我使用 [nano](https://www.npmjs.com/package/nano) 库连接到一个 [CouchDB](https://couchdb.apache.org/) 数据库。特别是，我在所谓的存储库类中使用 nano，它负责与数据库进行交互。

在为一个新的“批量删除”操作编写单元测试时，我们需要确保批量删除正在做它需要做的事情。因为我们正在讨论单元测试，所以我们确实想要模拟 nano 数据库连接以及批量操作数据库调用。

使用 nano，当我们建立一个数据库连接时，我们得到一个类型为`DocumentScope<D>`的对象。在许多其他功能中，该接口包括一个`bulk`功能，可用于执行批量操作。

该功能实际上有两种变体:

正如你所看到的，根据它被调用的方式，我们得到一个`Promise<DocumentBulkResponse[]>`或者一个`Promise<DocumentInsertResponse[]>;`。

第二个变体对应于批量插入操作，而第一个(我们需要模拟的那个)对应于批量删除。

通常，当我们使用 Jest 模仿函数时，我们是这样做的:

这对于只有一个变体的情况很好，但是在我们的例子中，我们需要能够告诉 TypeScript 我们在模仿哪个变体；否则，我们会得到一个编译错误，一开始可能有点误导:

现在你知道了函数的两个变量，你就能理解这个错误来自哪里；TypeScript 希望我们尊重第二个变量的签名(以及返回类型)。

咩！

那么我们如何解决这个问题呢？

# 解决方案

这个测试问题的解决方案实际上非常简单:

*   定义一个自定义类型，匹配您想要模仿的变量的签名
*   在创建 mock 之前，将类型转换为使用您刚刚定义的类型

这里有一个例子:

有了这些，TypeScript 不再抱怨了。当然，这只是一种变通方法，有时可能会导致问题(例如，如果被模仿的函数的签名发生了变化)，但这是一种推进测试的有用技巧。

参考资料:

*   [https://stack overflow . com/questions/61752367/how-to-mock-overload-method-in-jest](https://stackoverflow.com/questions/61752367/how-to-mock-overload-method-in-jest)
*   https://github.com/marchaos/jest-mock-extended/issues/20
*   [https://stack overflow . com/questions/52760509/typescript-return-type-of-overloaded-function/52761156 # 52761156](https://stackoverflow.com/questions/52760509/typescript-returntype-of-overloaded-function/52761156#52761156)
*   [https://github . com/definitely typed/definitely typed/issues/34889](https://github.com/DefinitelyTyped/DefinitelyTyped/issues/34889)

# 结论

在本文中，我解释了如何在使用 Jest 和 TypeScript 模仿函数时处理重载。

还有其他方法可以解决这个问题，但是这种方法非常简单(尽管很“弱”)。TS 团队的官方建议是将更一般的重载放在最后定义，但有些库并不总是遵守这一点(我猜主要是因为它并不那么出名)。

今天到此为止！

PS:如果你想了解大量关于软件/Web 开发的其他很酷的事情，那么你可以预订[我即将出版的书](https://dev-concepts.dev/)，拿一本[我的打字本](https://www.amazon.com/gp/product/B081FB89BL)，和/或订阅[我的简讯](https://mailchi.mp/fb661753d54a/developassion-newsletter)！