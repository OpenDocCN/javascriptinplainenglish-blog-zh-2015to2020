# 请停止在 JavaScript 中使用类，成为一个更好的开发人员

> 原文：<https://javascript.plainenglish.io/please-stop-using-classes-in-javascript-and-become-a-better-developer-a185c9fbede1?source=collection_archive---------0----------------------->

![](img/66dfbf7dfa9e0ca59725115e963f7cc2.png)

Image by Pexels from Pixabay

多年来，面向对象编程是软件工程中事实上的标准。类、多态性、继承和封装的概念主导并革新了开发过程。但是所有的东西都有有效期，包括编程范例。在本文中，我将讨论为什么首先引入类，为什么在 JavaScript 中使用类是一个坏主意，以及有哪些替代方法。

我不打算谈论为什么面向对象程序正在逐渐消失，但是你可以查看[这篇伟大的文章](https://medium.com/better-programming/object-oriented-programming-the-trillion-dollar-disaster-92a4b666c7c7)了解更多信息。

# ES6 之前的课程

尽管 class 关键字是从 ES6 (ECMAScript 2015)开始被添加到 JavaScript 中的，但是人们在更早的时候就使用了 class。实现这一点的方法是构造函数和原型委托。为了向您确切地展示我的意思，我将在 ES5 和 ES6 环境中实现相同的类。想想继承了`Car`的类`Car`和`SportsCar`。两者都有`make`、`model`属性和`start`方法，但`SportsCar`也有`turbocharged`属性并超越了`start`方法:

正如你可能猜到的，函数`Car`(第 2 行)和`SportsCar`(第 18 行)是构造函数。使用`this`关键字定义属性，使用`new`创建对象本身。如果您不熟悉`prototype`，这是一个特殊的属性，每个 JS 对象都必须委托通用行为。例如，数组对象的原型具有您可能很熟悉的功能:`map`、`forEach`、`find`等。字符串原型具有`replace`、`substr`等功能。

在第 33 行创建 Car 对象后，您可以访问它的属性和方法。从第 34 行开始的呼叫会导致以下操作:

1.  JS 引擎用键`start`向`car`对象请求一个值。
2.  对象回答说它没有这样的值
3.  JS 引擎向`car.prototype`对象请求一个带有键`start`的值。
4.  `car.prototype`返回`start`函数，JS 引擎立即执行该函数。

访问 make 和 model 属性的方式类似，只是它们是直接在 car 对象上定义的，而不是在原型上定义的。

继承有点棘手。它在第 24–25 行处理。这里最重要的函数是`Object.create`函数。它接受一个对象并返回一个全新的对象，其原型被设置为作为参数传递的值。现在，如果 JS 引擎没有在`sportsCar`对象或`sportsCar.prototype`上找到值，它将查询`sportsCar.prototype.prototype`，这是`Car`对象的原型。

# ES6 类别关键字

随着 2015 年 ES6 的发布，期待已久的`class`关键词在 JavaScript 中到来了。这是按照社区的许多要求做的，因为人们对使用面向对象语言感到不舒服。但是他们忽略了重要的一点。

> JavaScript 不知道什么是类

JavaScript 不是面向对象的语言，它不是被设计成面向对象的语言，类的概念绝对不适用于它。虽然 JS 中的所有东西都是对象，但这些对象不同于 Java 或 C#中的对象。在 JS 中，一个对象只是一个地图数据结构，带有一个稍微复杂的查找过程。就这样，真的。当我说一切都是对象时，我是认真的:甚至函数也是对象。您可以使用下面的代码片段来检查它:

好吧，这些都很好，但是`class`关键字是如何工作的呢？很高兴你问了。你还记得之前的`Car`和`SportsCar`的例子吗？嗯，class 关键字仅仅是语法上的糖衣。换句话说，类在概念上产生相同的代码，并且只用于美观和可读性的目的。正如我之前承诺的，这里有一个 ES6 中相同类的例子:

这些示例是相同的，并且产生相同的结果。有趣的是，它们在幕后产生(几乎)相同的代码。我不会把它写在这里，但是如果你好奇，可以去[在线巴别塔翻译器](https://babeljs.io/en/repl#?browsers=&build=&builtIns=false&spec=false&loose=false&code_lz=MYGwhgzhAEDCYCdoG8BQ1rAPYDsIBcEBXYfLBACgFswBrAUwBpoqsATekAShXQ2nwALAJYQAdDQbQAvCzr0A3HwxDRE9pxksNIJRgC-fPgUT4KPNP0y4IWEPTEgsAcwoByAG4IsWKm6560IYYfGQAyoTCOK4WytZ4dg5OrgAG8EgAtNAAJMiq4pL0-tBZufnqHCD6KQF8hoaooJAwYQAO5PgQ6dD0AB749DhsMN2W8QTEpOTU8syslcz4RAgARljAgojO9GyxVhBErfSUhXM6tVblS6vrmwjbbFrXaxtbO4HB0Mb4pua8VtgEvZHC53AA1ABKAHkYVCALL-D6oBqoAD0qOgAEFSEQwCBoEQIGBttAEPQaFEYEJ6NAiVR6KgPIhMMzZDh6AB3OCIdwAOVERJwbmYbjCRBwOAAnojGogxCYEGYAtB0dAvD4qI0bIkQa5gHLCsrVfyoGAcKhGcyIO1FV1WdB2Vy2h07ZRRUQVogiMLoG4AEIQgBaPsIRHotWtLvS8p-ivMChVGMhsPhWqBSVB-oQYmetzeuwTqtD9CAA&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=true&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=es2015&prettier=true&targets=&version=7.8.0&externalPlugins=)看看输出。

# 为什么不呢？

现在你应该了解 JS 中的*类*是什么，以及它们是如何工作的。现在，有了这些知识，我可以解释为什么在 JS 中使用类是一个坏主意。

1.  **装订问题**。由于类构造函数与关键字`this`密切相关，这可能会引入潜在的绑定问题，尤其是当您试图将类方法作为回调传递给外部例程时(hello，React devs👋)
2.  **性能问题**。由于类的实现，它们在运行时很难优化。虽然我们目前享受着高性能的机器，但摩尔定律正在消失的事实可能会改变这一切。
3.  **私有变量**。首先，私有变量是类的最大优势和主要原因之一，但在 JS 中是不存在的。
4.  **严格的等级制度**。类引入了直接的自顶向下的顺序，使得更改更难实现，这在大多数 JS 应用程序中是不可接受的。
5.  [**因为 React 团队告诉你不要**](https://reactjs.org/docs/hooks-intro.html) 。虽然他们还没有明确反对基于类的组件，但他们很可能会在不久的将来这样做。

所有这些问题都可以通过 JS 对象和原型委托来缓解。JS 提供了比类更多的功能，但是大多数开发人员对此视而不见。如果你想真正掌握 JS，你需要拥抱它的哲学，远离教条式的阶级思维。