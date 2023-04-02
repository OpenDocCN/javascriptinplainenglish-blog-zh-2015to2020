# 什么时候可以在 JavaScript 中对私有变量和函数进行单元测试？

> 原文：<https://javascript.plainenglish.io/unit-testing-private-variables-and-functions-in-javascript-when-is-it-ok-6090bd43c7b5?source=collection_archive---------11----------------------->

## 问这个问题可能是潜在设计缺陷的标志。解决这些问题的第一步。

## 为什么你通常不应该测试它们

在她的书[*Ruby*](https://www.poodr.com/)中的实用面向对象设计中，Sandi Metz 列举了以下理由**而不是**在单元测试中包含私有方法:

*   **冗余**:私有方法隐藏在*已经*测试过的公共组件中；现有的测试总是会暴露其中的失败。
*   **不稳定性**:私有方法更容易改变，当它们改变时，测试也会改变。这需要时间。
*   **暴露**:测试私有方法会将它们暴露给更多的受众，这些受众可能会被误导而使用它们，因此通过依赖它们来破坏封装。

我只能说我完全同意第一点:裁员。尽管这些观点很有道理，但它们更适合于实际的面向对象编程，这是 JavaScript 还不具备的一个方面。

关于不测试私有变量或方法，我发现书中的另一段摘录更有说服力:

**【单元测试】揭露底层代码中的设计缺陷。如果一个测试需要痛苦的设置，代码会期望太多的上下文。如果测试一个对象会将一堆其他对象拖进来，那么代码就有太多的依赖项。如果测试难以编写，其他对象会发现代码难以重用。**

这一点让我重新考虑我在发布的上一个 JavaScript 包中进行单元测试的方式。我们现在会研究这个问题。

## …以及如何测试它们

![](img/e7545bad4f574eb45cba40a74e279763.png)

Photo by [Austrian National Library](https://unsplash.com/@austriannationallibrary?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

考虑来自我的 JavaScript 模块[pending-promise-recycler](https://www.npmjs.com/package/pending-promise-recycler)的以下简化片段:

```
const registry = new ***Map***();module.exports = function recycle(func, options = {}) {
    // Here, we use the variable **registry** to cache promises.
    // The **key** of the cached function comes from the **options** and the
    // developer can either use a custom function or rely on a       
    // default builder that uses function name and arguments.
    // If the cache is a **hit**, return the cached function
    // If it's a **miss**, cache the function and return it
});
```

对于许多单元测试的实现，我认为最简单的方法是访问`registry`变量，它不是由模块公开的。这可以通过使用 [**模块**](https://www.npmjs.com/package/rewire) 重新布线轻松实现:

```
const rewire = require('rewire');const recycle = rewire('./index');const registry = **recycle.__get__('registry')**;
```

有关更多细节，请查看[完整单元测试套件](https://github.com/Dunkelheit/pending-promise-recycler/blob/main/index.test.js)。

很好！现在，我可以测试以下三种情况:

1.  缓存在注册表中的函数有键，这些键可以由自定义函数生成，也可以由函数名及其参数决定。
2.  在(promise)函数因为已被解析而不再属于注册表后，注册表将被清空，以防止潜在的内存泄漏。
3.  缓存实际上是工作的，有命中和未命中，这取决于某个函数是否仍在缓存注册表中。

但是，等等……我真的需要为此访问私有变量`registry`吗？该回到我最喜欢的节选了。很明显，在为了容易设置和执行而越过检查之后，我没有从正确的角度来看待单元测试。**一个失败的私人变量** `**registry**` **应该是可以通过肤浅的、公共的行为功能**检测出来的。

*   **对于测试 1 和测试 3** ，在不详细讨论实现细节的情况下，我可以很容易地创建一个单元测试，通过使用 [sinon 间谍](https://sinonjs.org/releases/latest/spies/)断言缓存的函数没有被调用两次，而是被调用一次，来验证缓存中的函数不会被执行两次。这应该适用于自定义或默认的密钥构建功能，如案例 1 中所述。
*   **对于情况 2** ，我可以验证函数最终被从缓存中移除，因为一旦它们被满足，对缓存函数的任何下一次调用将不是命中，而是未命中。使用 sinon 间谍也可以很容易地测试这一点。

对私有变量进行单元测试真的很有吸引力，在 JavaScript 中，由于像重新布线这样的模块，这非常容易。**如果你发现自己不得不对私有变量进行断言，这可能表明你的实现或者你的单元测试方法应该得到更多的时间和关注**。

最后，让常识来决定吧！

您是否面临过类似的情况？我很想听听进展如何！在下面给我留言！