# 让我们在 JavaScript 中分解数字

> 原文：<https://javascript.plainenglish.io/lets-break-numbers-in-javascript-632f12de6218?source=collection_archive---------7----------------------->

## 你擅长算术吗？JavaScript 呢

![](img/0193681d399f2b0f9467edcbe4ec741f.png)

Photo by [Michał Parzuchowski](https://unsplash.com/@mparzuchowski?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

任何了解 JavaScript 的人都会知道为什么会有`{} !== {}`和`{} != {}`。这很好，因为 JavaScript 在这里与使用引用来比较对象是一致的。

但是让我们来看看 JavaScript 的一些真正奇怪的地方。让我们试试这些。如果你的电脑上安装了 node.js，你只需打开一个终端并输入`node`就可以轻松地完成。

```
typeof []
// 'object'typeof {}
// 'object'typeof new Date()
// 'object'typeof new String()
// 'object'typeof null
// 'object'
```

🤔没什么意义，是吗？我们继续。`NaN`定义为[非数字](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/NaN)。所以试图像对待 T5 一样对待一个不是 T4 的值会产生这样的结果。例如

```
Number('blabla');
// NaN"hello" - 5
// NaN
```

这一切都有道理。当然，一个字符串不是一个数字。但是让我们试试这个:

```
typeof NaN
// 'number'
```

所以“不是数字”的类型是“数字”🙃。让我们探索一些其他的“数字”。在 REPL，尝试以下命令:

```
01
// 1
02
// 2
03
// 3
04
// 4
05
// 5
06
// 6
07
// 7
08
// 8
```

相当标准。01 是 1，02 是 2，03 是 3 等等。但是等我们到了 10 号。在 JavaScript 领域`09`是`9`但是`010`是`8`。但是等等，我觉得这有点奇怪……似乎有一种模式:

```
010
// 8
011
// 9
012
// 10
013
// 11
014
// 12
015
// 13
016
// 14
017
// 15
```

那好吧。结论:JavaScript 很奇怪，但仍然是符合逻辑的。这似乎有一定的逻辑……我们继续，试试`018`。等等。`018`的输出是… `18`。所以…我不知道该说什么。这些怪癖使得 javascript 很难理解。有一个规则，但这是一个奇怪的规则。老实说，我不知道 JavaScript 的这个规则或“特性”有什么用。但如果你想了解它，[这篇 stackoverflow 帖子](https://stackoverflow.com/questions/30386993/why-in-javascript-does-10-010-result-in-false/30387040)描述得很好。这和 JavaScript 中的`010 === 10`是`false`是一个道理。

在我们的 JavaScript 控制台或节点 REPL 中，让我们现在尝试一个大的数字。输入`9999999999999999`。您看到下一行返回了什么？`10000000000000000`。等等，这两个数字怎么会一样。这一定是由于某种舍入机制，对吗？因为这些数字肯定是不一样的…或者是吗？

`9999999999999999 === 10000000000000000`返回`true`。酷毙了。如果我给`9999999999999999`加 1 呢。`9999999999999999 + 1 === 10000000000000000`也会还真吗？是的。好吧，这个有道理。那么如果我们做了:

```
9999999999999999 + 1 === 9999999999999999
// true
```

哼。一个数加一怎么能等于它自己？从中减去 1 怎么样？

```
9999999999999999 - 1 === 9999999999999999
// true
```

也是真的。太好了。让我们发现更多的疯狂。我们看到了非常大的数字。这次我们来看一些小的。让我们试试`0.1 + 0.2`。很简单，对吗？你可能会这么想……但不是。结果是`0.30000000000000004`。我们甚至不需要去看那些非常小的数字就能发现一些奇怪的东西。不如我们试试小一个数量级:

```
0.01 + 0.02
// 0.03
```

好的。所以现在我们恢复了理智。但是为什么这些操作都可以，上面的不行呢？嘘 JavaScript 教会不欢迎这种问题。

如果你用过 JavaScript，你会知道`0`就是 [falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy) 。所以`0 == false`给了我们`true`。酷有道理。而且`null`也是假的，所以`null == false`也给了我们`true`。酷毙了。那`0 == null`呢。这是错误的。但是因为我们很奇怪，我们用 JavaScript 编码，我们知道`==`不是一个严格的“等于”操作符，我们仍然可以理解这个奇怪的逻辑。但是如果我们做`0 <= null`呢。这给了我们真实…那么自然地，既然`0 != null`和`0 <= null`，那么它一定意味着`0 < null`。不😁。我们忘记了在 JavaScript 领域，事情并不总是有意义的。

这只是对我们最喜欢的语言如何处理数字的一个快速浏览。更多这样的乐趣，跟随我这里，或更多的随机性；在推特上。