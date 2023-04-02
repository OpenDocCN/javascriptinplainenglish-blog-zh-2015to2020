# JavaScript 的下一步是什么？

> 原文：<https://javascript.plainenglish.io/whats-next-for-javascript-e1c2dbb7a403?source=collection_archive---------10----------------------->

![](img/019b6d4292f16bb23243b78b25dc6980.png)

如果你在 2015 年之前使用过 web 技术，你会知道 JavaScript 过去变化真的很慢。事实上，**直到 2015 年才发布了一个标准的 JavaScript 规范**，将我们一直使用的所有特性量化到一个文档中。

然而，在接下来的 5 年里，这一切都改变了，JavaScript 每年都发布新的规范，每个规范都有更好的特性。那么 JavaScript 的下一步是什么？对于 JavaScript 的新特性有一系列的提议，有些还处于定义的早期阶段。下面，我将探索 JavaScript 即将推出的一些最有用的特性。

# Array.prototype.at()

**JavaScript 状态:**需要添加

**停下来想一想，为了得到数组的最后一个元素，你写了多少次** `arr[arr.length-1]`。许多开发人员要求 JavaScript 将这种符号改为`arr[-1]`，其中-1 是我们所引用的数组末尾的数字。然而，由于数组的限制，这很难实现。

相反， [tc39](https://tc39.es/) 小组建议我们用`arr.at(-1)`来引用数组中的最后一个元素，或者用`arr.at(1)`来引用第一个元素。这种符号更直观，并且有望为我们节省一点理智(和几个字节)。

# 时态而不是日期

**JavaScript 状态:**需要添加

我们都知道 JavaScript 的日期函数一团糟，这也是有原因的。 [JavaScript 和日期函数，10 天就写好了。](https://maggiepint.com/2017/04/09/fixing-javascript-date-getting-started/) `Temporal`打算尝试并解决所有那些问题。temporal 中的大多数函数返回一个对象，您可以轻松地从中提取日、月、秒和年数据，而不必使用`substr()`。`Temporal`相当复杂，[和有据可查的](https://tc39.es/proposal-temporal/docs/)，所以它可能值得自己写一篇文章来探究为什么以及如何有用。

可以说，用`Temporal`管理和操作日期和时间将变得更加容易。它将解决`Date`的三个最大问题:

*   与`Date`不同，`Temporal`对象将是**不可变的**
*   默认情况下将支持更多的日历
*   **增加对其他时区**的支持，除了 UTC 和用户的本地时区(最终)。

# Array.prototype.unique()

**JavaScript 状态:**仍在定义中

这是一个非常早期的提议，但它本质上将把类似`[ 1, 1, 2, 3, 4, 4 ]` **的数组变成一个独特的** `[ 1, 2, 3, 4 ]`数组。这已经作为 uniq 存在于 *lodash* 中——并且是那里最受欢迎的函数之一，所以将它引入核心 JavaScript 是有意义的。这个简单明了，以至于你开始想为什么它还没有出现在 JavaScript 中。

# 元组和记录

**JavaScript 状态:**需要添加

没错，我们将得到两种新的基元类型。你们中的一些人可能已经知道元组，它存在于其他语言中，比如 Python 和 Swift，但是 records 是 JavaScript 中一种独特的对象元组。元组是一个既唯一又有特定顺序的数组。也是不可改变的或者说**不可改变的**。

## 为什么会有用？

元组和记录的主要好处之一**你可以使用深度相等来比较它们的值。也就是说，你可以比较它们的值，看一个多级记录是否等于另一个记录。特别是在对象中，这是很难的——您必须遍历对象的每一层来比较值是否相同。**

如果你不知道在 JavaScript 中检查两个对象的值是否相等有多难，[看看这个 Stackoverflow 问题](https://stackoverflow.com/questions/1068834/object-comparison-in-javascript)。

# **Map.prototype .炮位()**

**JavaScript 状态:**需要添加

通常，当处理对象或地图时，您希望**查找一个项目**，如果它不在中，则**插入它，如果它在**中，则**更新它。目前，您必须查找密钥，检查它是否存在，然后插入或更新它。`Map.prototype.emplace()`试图通过提供一种方法来解决地图的这个问题，如果你想更新，它可以做一件事，如果你想插入，它可以做另一件事。**

# 结论

我们不仅将获得 ES2021 的许多新特性，而且 JavaScript 的**甚至更多的**变化即将到来。其中一些功能在过去会节省我很多时间。有没有一个项目，这些功能会对你有所帮助？请在评论中或在[推特](https://twitter.com/smpnjn)上告诉我。

## 进一步阅读

*   [TC39 工作组](https://tc39.es/)
*   [修复 JavaScript 日期](https://maggiepint.com/2017/04/09/fixing-javascript-date-getting-started/)
*   [Brendan Eich 在 10 天内创建 JavaScript](https://thenewstack.io/brendan-eich-on-creating-javascript-in-10-days-and-what-hed-do-differently-today/)

[](https://medium.com/javascript-in-plain-english/new-javascript-features-that-will-make-your-life-easier-502f77551e9a) [## 新的 JavaScript 特性将使您的生活更加轻松

### JavaScript 变化很快。了解您可以在下一个项目中使用的这些新功能。

medium.com](https://medium.com/javascript-in-plain-english/new-javascript-features-that-will-make-your-life-easier-502f77551e9a)