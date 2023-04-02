# 你需要知道的 5 个 JavaScript 技巧

> 原文：<https://javascript.plainenglish.io/5-javascript-tips-you-need-to-know-right-now-eca68691057?source=collection_archive---------7----------------------->

![](img/96a26923c272034c952b61886c70f0ec.png)

Photo by [Aziz Acharki](https://unsplash.com/@acharki95?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

更简单、简洁的代码总是比复杂、古怪的解决方案要好。在简短和易读的解决方案之间找到恰当的平衡可能是一个挑战。这里有一些技巧可以帮助你更高效地编写 JavaScript。

# 1.可选链接

**可选链接**操作符(`**?.**`)允许我们在一个对象中进行嵌套属性查找，而不必执行多次空值检查。

# 2.无效聚结

受 C#的启发，Nullish 聚结成功应用于 ES2020。这是一个非常方便的二元运算符，它返回第一个*定义的*值。

您必须熟悉使用`**||**`操作符来执行默认值赋值，如下所示:

但是问题是，`||`和‘假’值一起使用是不安全的。

重要的区别||还有？？运营商是:

*   `||`返回第一个*真值*值
*   `??`返回第一个*定义的*值

# 3.使用集合过滤数组中的唯一值

`Set`类型是包含唯一值的 iterable。由于`Set`类型是可迭代的，我们可以将它与'…' spread 操作符结合起来，在一行中查找数组中的唯一值。

需要记住的一点是，惟一性是通过对象引用相等性检查来确定的。所以这种技术最适用于具有原始值的数组，如 *number、string、null、undefined。*

如果你想学习如何在 JavaScript checkout [这篇文章](https://medium.com/bytelimes/iterables-in-javascript-custom-iterables-97a1f15852eb)中制作自己的可迭代数据结构。

# 4.在函数中使用析构的对象参数

根据一般经验，我们应该避免使用多参数函数。这是因为它可以很快变得不可读。

在函数中使用析构对象参数:

**好处**:

*   可读性更强的函数调用签名
*   您可以跳过具有默认值的属性

# 5.从数组中过滤“false”值

通过使用布尔构造函数作为`[predicate](https://en.wikipedia.org/wiki/Predicate_(mathematical_logic))`函数来过滤数组，我们可以很容易地过滤掉数组中的“false”值。

双 bang `!!`将值合并为布尔表示，filter 方法过滤掉假值。

# 结论

利用需要更少代码来编写更多代码的语言特性是非常有用的。我们学习了一些快速技巧来更高效地编写 JavaScript。

如果你想学习反应式编程，[看看这本书](https://leanpub.com/reactiveprogramming/)。

感谢你的阅读，我希望这能帮助你，就像它帮助了我一样。

你最喜欢的高效 JavaScript 技术是什么？我很想在下面的评论中了解他们

你可以在 [GitHub](https://github.com/nivrith) 或 [Twitter](https://twitter.com/_Nivrith_) 上关注我

快乐工程！