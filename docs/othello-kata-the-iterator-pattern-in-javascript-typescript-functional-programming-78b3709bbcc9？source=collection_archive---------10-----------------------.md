# Othello Kata:JavaScript/TypeScript 函数式编程中的迭代器模式

> 原文：<https://javascript.plainenglish.io/othello-kata-the-iterator-pattern-in-javascript-typescript-functional-programming-78b3709bbcc9?source=collection_archive---------10----------------------->

![](img/189851d9e131130f4fd1c1fdfa2428ee.png)

[Othello Tutorial with 2x World Champion Ben Seeley on YouTube](https://www.youtube.com/watch?v=8VAq0C3oMBc&ab_channel=SpinMasterGames)

本文是在 TypeScript 中著名的 kata 上演示函数式编程概念的系列文章的一部分。参见 [*FizzBuzz 形:与函子的探索*](https://medium.com/javascript-in-plain-english/fizzbuzz-kata-an-exploration-with-functors-41d5217464fc) *！*

# 奥赛罗(又名黑白棋)

《奥赛罗》除了是威廉·莎士比亚的著名悲剧之外，还是一款两人在 8×8 棋盘上玩的策略棋盘游戏。据说需要“一分钟学习，一生掌握”。事实上，看到戏剧性的优势逆转直到比赛结束并不罕见。如果你对这个游戏不熟悉，想学，可以[在线](https://www.othelloonline.org/)试一试。

在这个形中，我们将尝试实现告诉玩家可以在哪里玩的功能:

现在，我们选择将棋盘建模为数组的数组:

在某些时候，这个形需要在棋盘上迭代，并确定每个格子放置一个圆盘是否允许捕捉对手颜色的圆盘。对于数组的数组，这意味着循环遍历行(1 到 8)以及每行循环遍历列(A 到 H)。

但这是实现细节，人们可能会在不知道这实际上是如何发生的情况下，对棋盘进行循环。迭代器模式就是在那里弹出来的！

# 迭代器模式

迭代一个数组似乎很明显，你只需循环遍历它的所有项。但是如何迭代一棵树呢？深度优先遍历和广度优先遍历是最有用和最常用的方法，尽管也可以设想其他方法。无论如何，相应的实现细节是*而不是*客户端代码所关心的。

[重构大师](https://refactoring.guru/design-patterns/iterator)指出:

> 迭代器是一种行为设计模式，允许您遍历集合的元素，而不暴露其底层表示。

在 JavaScript 中，[迭代器协议](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#The_iterator_protocol)指定了一个对象为了在`[for...of](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of)`语句中可迭代而必须实现的接口。具体地说，当一个对象实现一个返回具有以下两个属性的对象的`next()`方法时，它就是一个迭代器:

*   `done` : `false`如果迭代器能够产生集合中的下一个值；`true`如果迭代器完成了它的序列。
*   `value`:如果`done`是`false`，则是集合的下一个 JavaScript 值。否则，不定义该属性。

这非常方便，但是不太适合函数式编程风格，函数式编程风格更喜欢声明而不是命令式语句(参见维基百科上的[声明式编程](https://en.wikipedia.org/wiki/Declarative_programming))。实际上，这意味着人们更喜欢使用`map`或`reduce`函数而不是`for...of`循环来迭代数据结构。

因此，迭代器模式需要调整。

# 遍历数组

对于数组，迭代通常通过`[reduce](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)`函数实现，该函数具有以下签名:

```
Array.prototype.reduce(callback(accumulator, currentValue[, index[, array]]) {
  ...
}[, initialValue]);
```

`reduce`函数的一个常见用例是将所有数组的数字相加:

顺便说一下，`reduce`函数是基本的，因为`map`和`filter`函数是从它派生出来的，正如这些替代实现所暗示的:

现在，受数组的启发，我们将定义一个具有类似签名的`reduce`函数，以便在棋盘上迭代。

# 建议的实现

首先让我们定义一个集合的通用接口，我们可以用一个`reduce`函数对其进行迭代:

现在我们可以如下定义一个可迭代的棋盘:

并在棋盘上实现实际的循环。电路板的`reduce`功能依赖于底层数组的`reduce`功能:

由于上述原因，通过`reduce`函数迭代棋盘终于成为可能。简单地说，我们在棋盘上迭代，并在一个数组中收集可玩单元的坐标:

# 最后的想法

通过一个具体的例子，本文说明了这样一个事实，即许多面向对象的编程模式在函数式编程世界中仍然有效，假设做了一些调整。

将一种模式从一种编程风格转换到另一种编程风格需要引用该模式想要解决的原始问题，例如，在不知道具体细节的情况下迭代一个数据结构。

欢迎评论。感谢阅读！

# 资源

*   [https://github.com/mathieueveillard/othello](https://github.com/mathieueveillard/othello)
*   [维基百科上的奥赛罗](https://en.wikipedia.org/wiki/Reversi#Rules)
*   重构大师的迭代器模式
*   [MDN 上的 array . prototype . reduce](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)
*   作曲软件:埃里克·埃利奥特的书
*   凯尔·辛普森的[功能轻 JavaScript](https://github.com/getify/Functional-Light-JS)
*   [基本上足够的函数式编程指南](https://github.com/MostlyAdequate/mostly-adequate-guide)
*   [催化剂](https://katalyst.codurance.com/browse)，通过共保