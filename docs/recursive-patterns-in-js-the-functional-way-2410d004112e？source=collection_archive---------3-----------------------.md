# JS 中的递归模式，函数方式

> 原文：<https://javascript.plainenglish.io/recursive-patterns-in-js-the-functional-way-2410d004112e?source=collection_archive---------3----------------------->

![](img/0625dce63787732754fcf2edbe95518d.png)

这是一篇关于数组数据结构的函数递归的文章。

这篇文章的灵感来自于文章[“使用 ES6 的函数式 JS—递归模式](https://medium.com/dailyjs/functional-js-with-es6-recursive-patterns-b7d0813ef9e3)”但是这里我们要走一条更深刻的路，并且必须这样做，列表的代数定义是`List = 1+a*List`

> 这表示该列表或者是空列表 **[]** 或者是元素 **a** 和一个较小列表的串联。

# 模式匹配

首先，我们可以编写下面的 patternMatch 方法，它允许我们分离列表的不同部分

模式对象是这样的:

1.  其中在 `pattern.concat(head, tail)`方法中，`head`是数组的第一个值，如果数组不为空，`tail`是数组的其余部分。
2.  如果其为空，则调用`pattern.empty()` 。

定义模式匹配后，我们可以立即定义一些概念，例如数组的第一个元素:

或者类似地，数组的尾部:

___________________________________

# 功能褶皱

现在我要定义一个简单但容易混淆的方法。下面的方法在数组上定义了一个 catamorphism，或者更常见的名称 fold(我将保留名称 cata，但是您可以将其命名为 Fold)。

[参数评估通常称为代数。]

这里的关键点是:

```
concat: (head, tail) => evaluation.concat(head, **tail.cata(evaluation)**)
```

我们递归地通过评估来评估数组`**tail.cata(evaluation)**`的**尾部**

*注意:如果对你有帮助的话，你可以将方法重命名为 evaluation，这对面向对象的程序员来说可能更有意义:*

[**Catamorhism/Fold**](https://en.wikipedia.org/wiki/Catamorphism)**是将一个数组分解成某种其他类型的最通用方式**。`evaluation` 参数与我们在模式匹配中使用的`pattern` 具有(几乎)相同的方法:

```
evaluation= { 
  empty: () => {...},
  concat: (headEval, tailEval) => {...}
}
```

让我们使用`cata`函数来定义数组的长度。

# 长度

为什么会这样？不幸的是，JavaScript 没有类型，所以您看不到类型是什么。但是在这个长度代数中，你可能认为一切都是整数:

```
{                               
  empty: () => 0,                  
 concat: (v, r) => 1 + r          
}
```

> **我们希望空数组为 0，如果它不为空，则加上头部的长度 1 和其余部分的长度** `**r**`

我们当然可以不用`cata()`而简单地通过模式匹配和递归来实现:

> **在某种程度上，我们排除了** `**cata()**` **方法中的递归。我们可以不用递归来编写定义**

让我们看更多的例子。用于反转阵列

# 反面的

这里我们使用了一个代数来交换第一个值(v)的一部分和数组的其余部分(…r)。

# 过滤器

我们在这里使用的代数检查列表的头部。如果值不满足过滤器，我们只保留数组的其余部分。

# 地图

该地图非常默认:

# 减少

最后，Reduce 是大多数人使用的最接近 catamorhism 定义的词。(这和`cata`一样，只是做了一些重命名和重新安排，我在这里就不做了)。

**结论**:变形是将一个数组转换成其他类型的最通用的方法。因为潜在的想法可能会让开发人员感到困惑，所以我们使用等价的 reduce 数组方法。我们可以在像树这样的其他数据结构上使用分类。

___________________________________

如果你想搜索更多你可以定义 cata 上的树结构在这篇文章中[如何用 JavaScript 反转一棵树的函数方式](https://medium.com/javascript-in-plain-english/how-to-reverse-a-tree-in-javascript-the-functional-way-440d7106b277)