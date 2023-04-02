# 未来 JavaScript 版本中值得关注的 5 个特性

> 原文：<https://javascript.plainenglish.io/5-features-to-look-out-for-in-future-javascript-versions-5c21e1f0b2c2?source=collection_archive---------22----------------------->

![](img/650a4755a98b6073bec998191de9c36e.png)

Photo by [Joan Gamell](https://unsplash.com/@gamell?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/javascript?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

JavaScript 是一种不断发展的语言，每年 TC39 社区都会发布一个新版本。TC39 有一整套增加新功能的系统。开发人员提出的每个新功能都要经过 5 个阶段。先来了解一下 [TC39](https://tc39.es/) :

# TC39

负责维护语言和添加新功能的官方组织。根据官方网站的介绍，Ecma International 的 TC39 是一个由 JavaScript 开发人员、实现人员、学者等组成的团体，与社区合作维护和发展 JavaScript 的定义。

# 提案制度

1.  阶段 0——对规范进行补充的初始计划
2.  阶段 1 —描述问题和解决方案的正式提案
3.  第 2 阶段—使用官方规范语言的语法和示意图更新规范的初始草案
4.  第 3 阶段——完成草稿，并由指定的审阅者和 EcmaScript 编辑进行审阅，还从实现和用户那里获得反馈
5.  第 4 阶段—一切都已完成，所有的更改都已完成，新的规范将在下一版本中推出

> 查看 TC39 关于[提案阶段](https://tc39.es/process-document/)的官方文件，了解更多详情

# 类别字段

类字段包括私有方法和访问器(如`get` & `set`)、公共和私有实例字段、静态类字段和私有静态方法。让我们来看看它们:

## 私有方法和字段

我们所要做的就是在名字前加一个`#`。假设字段是`name`，那么它的私有版本将是`#name`。方法也是如此。以下是两者的一个例子:

## 静态字段和方法

就像我们对`instance`字段和方法所做的一样，我们也可以对静态字段和方法做同样的事情:

# 顶级等待

我敢打赌，我们大多数人都使用过`async` IIFEs(立即调用的函数表达式),也称为 IIFEs，以这种方式在模块的顶层使用`async/await`:

使用这个很酷的新特性，我们可以将这段代码简化为:

# Array.prototype.at()

多年来，程序员希望对 javascript 数组进行“负索引”，就像你对 python 所做的那样。“负索引”意味着能够执行`array[-1]`而不是`array[array.length-1]`，负数从最后一个元素开始向后计数。

然而，`[]`并不特定于数组和字符串，它适用于所有对象。当按索引取一个元素时，像`array[1]`，基本上就是一个对象的属性带值`1`。从技术上来说，`arr[-1]`在今天的代码中“起作用”，但是它返回对象的`-1`属性的值，而不是返回一个从末尾向后计数的索引。

`.at()`方法特定于`Array` s、`String` s 和`TypedArray` s。该方法采用一个整数值并返回该索引处的项目，同时向后计数。它可以这样使用—

# Object.hasOwn()

`Object.hasOwn()`让`Object.prototype.hasOwnProperty()`更容易上手。今天，像这样使用`Object.prototype.hasOwnProperty()`很常见:

这个方法简化了代码，如下所示:

> 点击了解更多关于`Object.hasOwn`和[的信息](https://github.com/tc39/proposal-accessible-object-hasownproperty)

# 新的`Set`方法

`Set.prototype`增加了基于集合论的新方法。如果你在高中时从来没有注意过(我也没有)，不要担心，我也会解释这些方法。让我们来看看它们:

1.  `Set.prototype.intersection(iterable)` —通过将当前集合与另一个集合相交创建一个新的`Set`，即与任一集合中的公共元素相交的集合。
2.  `Set.prototype.union(iterable)` —通过并集创建一个新的`Set`，它基本上合并了任一集合中的元素，并删除了任何重复的元素。
3.  `Set.prototype.difference(iterable)` —创建一个新的`Set`，排除作为参数的集合中的元素。
4.  `Set.prototype.symmetricDifference(iterable)` —返回只在`this`或`iterable`中找到的元素的`Set`
5.  `Set.prototype.isSubsetOf(iterable)` —检查`Set A`(调用方法的那个)的元素是否出现在`Set B`(参数)中
6.  `Set.prototype.isSupersetOf(iterable)` —检查`Set B`(参数)的元素是否出现在`Set A`(调用方法的那个)中

这个帖子到此为止！我希望你喜欢它。查看我的 Twitter，在那里我为开发者发布了技巧、窍门和迷因。再见🤘

# 资源

1.  私有类字段和方法—[https://github.com/tc39/proposal-private-methods](https://github.com/tc39/proposal-private-methods)、[https://github.com/tc39/proposal-class-fields](https://github.com/tc39/proposal-class-fields)和[https://github.com/tc39/proposal-static-class-features](https://github.com/tc39/proposal-static-class-features)
2.  顶级`await`——[https://github.com/tc39/proposal-top-level-await](https://github.com/tc39/proposal-top-level-await)
3.  https://github.com/tc39/proposal-relative-indexing-method 的`Array.at()`
4.  `Object.hasOwn()`—[https://github . com/tc39/proposal-accessible-object-hasownproperty](https://github.com/tc39/proposal-accessible-object-hasownproperty)
5.  设定方法—[https://github.com/tc39/proposal-set-methods](https://github.com/tc39/proposal-set-methods)

*更多内容请看*[***plain English . io***](http://plainenglish.io/)