# 用生成器+ Yield 处理异步 JavaScript

> 原文：<https://javascript.plainenglish.io/make-yield-great-again-javascript-ec6ced93f86?source=collection_archive---------4----------------------->

## JavaScript 和 yield 让异步代码和数据流变得简单！

![](img/0bbf815a5853ca64fa8fdb9dbc8b8fc1.png)

Photo by [Dodgeball Time](https://www.freeimages.com/photographer/clowdomega-32111) from [FreeImages](https://freeimages.com/)

***注:** *如果想只读 JavaScript 特定部分，跳转到 JavaScript* *章节中的* ***。***

*Yield* 自 2.0 版本发布以来就已经是 C#语言的一部分。几乎每个开发人员都读过一些关于它的东西，至少知道它“与 LINQ 有关”——但是具体是什么，很少有人能回答。具有讽刺意味的是，LINQ 是在 C# 3.0 中引入的，所以 yield 关键字在连接建立之前就存在了。因此，它至少还有一个用途。

事实上，yield-related 构造在 C#中使用相对频繁，具有 *IEnumerable* 和 *IEnumerator* 接口以及它们的通用对应物*IEnumerable<T>和 *IEnumerator < T >。**

接口 *IEnumerable* 基本上是用可枚举元素来标识一个类型。因此，中的每个可枚举类型。NET 实现了这个接口:从简单的数组到通用的特殊类型，如 *List < T >* 。IEnumerable 本身提供了一个名为 *GetEnumerator* 的方法，该方法返回一个 *IEnumerator* 的实例。

通用对应物 *IEnumerable < T >* 实现 *IEnumerable* ，但在其他方面的行为完全类似，并相应地返回 *IEnumerator < T >* 的实例。

# 从一数到 IEnumerator

*IEnumerator* 和 *IEnumerator < T >* 是允许类型实际迭代的接口。为了使这成为可能，他们为你提供了属性 C*current*方法 *MoveNext()* 。[1][2]每次使用 forEach 循环迭代集合时，C#都会创建一个枚举数，并使用它来逐步迭代集合。为了正常工作，你要迭代的对象必须实现非接口或者通用接口*。*

只有当一个方法的返回类型是*IEnumerable<T>时，C#才允许你访问关键字 *yield* 。您可以将 yield 视为一个链接到 forEach 循环迭代中的小家伙:当在 forEach 循环中调用一个方法时，它首先照常执行，直到 C#遇到 yield return 语句。*

在那里指定的值被返回给调用方，方法的执行结束。在 forEach 循环的下一次迭代中，将再次调用该方法。然而，这一次执行不是从头开始，而是在 yield 返回后，再次执行，直到下一个 yield 返回。

这意味着 C#保存被调用方法的状态，并在再次调用该方法时恢复它。一旦方法到达其结尾，枚举完成，调用 forEach 循环终止。如果你想提前终止循环，这可以通过使用*屈服* *中断*来完成。

这是一个必要的调用，因为传统的返回是不可能的:在这种情况下，您必须根据方法的签名返回一个实现 IEnumerable <t>的对象。</t>

利用收益率的优势再明显不过了。

1.  Yield 逐渐创建结果列表，因此我们可以更好地利用计算能力。如果 forEach 循环没有完全运行，则只生成实际需要的结果。
2.  我们使用更少的内存，因为之前不必建立大的列表，这可能并不完全需要。

正是因为这些原因， *yield* 关键字也被用于 C#中的 LINQ。*延期执行*基于*收益率。*【3】*这样做的话，并不是所有的查询结果都必须一次加载并保存在内存中。*

***提示:** Unity 提供协程。它们还返回一个类型为 *IEnumerable* 的对象，并且必须使用带有 *yield* 语句的返回。*

*[](https://medium.com/unity-hub) [## 统一中心

### 在篝火旁取暖，在团结中心与长者和智者聊天，以确保你的…

medium.com](https://medium.com/unity-hub) 

# JavaScript 中的产量

JavaScript 也知道关键字 *yield。从 ECMAScript 2015 开始，接口 *IEnumerator* 的对应物也实现了。JavaScript 将其标记为*生成器函数*。*

*Yield* 只有在包含函数被定义为生成器函数的情况下，才能在 JavaScript 中使用。这是通过*函数** 关键字完成的。第一眼看到的唯一区别是，不用 *yield* return，你只需要写 yield。[4]

之后，您可以调用这个函数，首先获得一个生成器，它是一个 *IEnumerator* 实例的 JavaScript 副本:

这允许我们接下来调用函数，该函数实际上执行生成器函数。发生*时中断产生*，并返回一个对象，该对象带有当前值和是否到达发生器结尾的信息:

通过使用 C#的 forEach 循环的 JavaScript 对应物:for … of 循环的*，生成器函数也可以更简单地运行。在底层，它负责调用下一个函数，并在到达生成器末尾时结束迭代:*

如果想提前终止一个生成器函数，只需用 return 关键字退出该函数。不匹配返回类型的问题是不必要的，因为函数调用没有静态定义的类型。所以处理起来比 C#简单一点。

# 异步发电机

自 ECMAScript 2018 以来，JavaScript 还知道生成器的异步变体，这些变体是用 *async* 关键字定义的，并允许在生成器中使用 *await* 调用异步代码。以前，这些函数只是为同步代码设计的，不可否认这是很不切实际的。

异步生成器的定义如下:

为了运行这样的生成器，JavaScript 还知道一种新形式的循环，因为经典的 *for … of-* 循环在这里并不合适——毕竟，它也是为同步代码设计的。

解决方法是- 循环中 *await …的*:**

我们应该注意到- 循环的 *for* *await …只能在异步函数中使用，因为它使用了 *await* 关键字。*

# 流动

总的来说，这实现了令人兴奋的新场景，尤其是在数据的异步处理中。这种数据通常以流的形式提供，必须将数据和结束附加到流的事件上:

然而，这只是最小的实现，因为您实际上还必须监听错误事件。我还建议删除流后面附加的事件处理程序，以防止内存泄漏。这同样适用于错误发生后。您用以下示例中的结构替换了以前紧凑且易于管理的代码。

很容易想象当不止一个流在运行时，这样的代码退化得有多快。然而，ECMAScript 2018 引入的异步文献器以最优雅的方式解决了这个问题，并且在内部基于已经描述的异步发电机的基本概念。

从版本 10 开始，Node.js 至少包括对此的实验性支持，流行的 web 浏览器甚至包括最终支持。[5]

*X _ 3 _ 1 _ stream processing . js*中显示的代码可以简化为以下几行:

*for await … of-* 循环处理正确连接和分离事件处理程序的逻辑。即使调用 break 手动退出循环，构造也会执行所有必要的清理。这极大地简化了基于流(或其他异步数据源)编写 JavaScript 代码。

# 结论

C#和 yield 的开发者并不是那么好的朋友。随着 LINQ 的引入，更多的开发人员定期使用 *yield* ，但大多是间接使用。

当我们谈到 JavaScript 时，它的行为是一样的。*收益率*不是很受欢迎。最近，由于异步数据处理通过生成器函数&变得更加方便，ECMAScript 2018 结合用于 await 的*为我们带来了异步生成器和迭代器..的*-环*产量*得到了比以前更多的关注。

通过在浏览器或服务器上使用 Node 开发代码，您可以轻松地使用 yield 更快、更轻松地达到目标。JS，并抱怨在处理流时不方便地开发策略。

为你自己节省大量的时间，专注于重要的主题。

**延伸阅读:**

[](https://medium.com/next-level-source-code/game-engines-guide-to-pick-the-right-16b843905ba6) [## 游戏引擎:选择正确的指南！

### 如何开发游戏第一步:为你的开发选择合适的引擎

medium.com](https://medium.com/next-level-source-code/game-engines-guide-to-pick-the-right-16b843905ba6) [](https://pjdarnold.medium.com/enums-typescript-4-0-and-javascript-guide-all-you-need-to-know-5e090355bff6) [## Enums TypeScript 4.0 和 JavaScript 指南—您需要知道的一切

### 你将读到的关于 enums 的最后一个指南！

pjdarnold.medium.com](https://pjdarnold.medium.com/enums-typescript-4-0-and-javascript-guide-all-you-need-to-know-5e090355bff6) [](https://medium.com/javascript-in-plain-english/the-dark-side-of-typescript-try-catch-deeded18ba0d) [## TypeScript 中 Try/Catch 的黑暗面

### 用“unknown”转义 TypeScript 4.0 中的任何错误异常

medium.com](https://medium.com/javascript-in-plain-english/the-dark-side-of-typescript-try-catch-deeded18ba0d) 

# 参考

[1]当前 c# API[https://docs . Microsoft . com/en-us/dot net/API/system . collections . ienumerator . Current？view = net-5.0](https://docs.microsoft.com/en-us/dotnet/api/system.collections.ienumerator.current?view=net-5.0)
【2】MoveNext()C # API[https://docs . Microsoft . com/de-de/dot net/API/system . collections . ienumerator . MoveNext？view = net-5.0](https://docs.microsoft.com/en-us/dotnet/api/system.collections.ienumerator.movenext?view=net-5.0)
【3】延迟执行微软示例
[https://docs . Microsoft . com/en-us/dot net/standard/linq/Deferred-Execution-example](https://docs.microsoft.com/en-us/dotnet/standard/linq/deferred-execution-example)
【4】Yield JavaScript[https://developer . Mozilla . org/de/docs/Web/JavaScript/Reference/Operators/Yield](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/yield)
【5】节点。JS 第十版[https://nodejs.org/docs/latest-v10.x/api/index.html](https://nodejs.org/docs/latest-v10.x/api/index.html)*