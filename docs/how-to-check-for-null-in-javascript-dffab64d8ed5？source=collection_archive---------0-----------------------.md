# 如何在 JavaScript 中检查 null

> 原文：<https://javascript.plainenglish.io/how-to-check-for-null-in-javascript-dffab64d8ed5?source=collection_archive---------0----------------------->

## 由于一个历史错误，JavaScript 中的`typeof null`返回`"object"`——那么如何检查`null`？

![](img/ff4138959099afc3b2258bbca3782ee7.png)

Photo by [Ben Hershey](https://unsplash.com/@benhershey?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 什么是`null`？

> “值`null`代表任何对象值的有意缺失。这是 JavaScript 的基本价值观之一。”— MDN 文档

JavaScript 原始类型表示有意缺少值——它通常是有意设置的，表示变量已经声明但还没有赋值。

这与类似的原始值`undefined`形成对比`null`，原始值是任何对象值的无意缺失。

这是因为一个已经声明但没有赋值的变量是`undefined`，而不是`null`。

不幸的是，`typeof`在调用`null`值时会返回`"object"`，因为[JavaScript](https://alexanderell.is/posts/typeof-null/)中的一个历史错误将永远无法修复。

这意味着不能使用`typeof`进行空值检查。

![](img/36e5a5961b9e13db40ea3880ab5dae63.png)

Photo by [Eugene Triguba](https://unsplash.com/@eugenetriguba?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# `null`是福尔西

> "`null`是一个 falsy 值(即，如果强制为布尔值，则计算结果为`false`)——[乔希·克兰顿](http://adripofjavascript.com/about/default.htm) at [一滴 JavaScript](http://adripofjavascript.com/blog/drips/equals-equals-null-in-javascript.html)

T 检查`null`最简单的方法是知道`null`在条件中的计算结果为`false`或者是否被强制为`[boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)`值:

当然，这并不能区分`null`和其他虚假值。

接下来，我探索使用`==`或`===`相等操作符来[检查空值](https://medium.com/javascript-in-plain-english/how-to-check-for-null-in-javascript-dffab64d8ed5)。

![](img/8cbb1bb52d76325677634b91b59534bf.png)

Photo by [Nicholas Ruggeri](https://unsplash.com/@nicholasruggeri?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 使用`==`的福尔西等式

> “尽管事实上`null`是[falsy],但它并不等同于 JavaScript 中的任何其他 falsy 值。事实上，`null`唯一大致相等的值是`undefined`和它自己— [乔希·克兰顿](http://adripofjavascript.com/about/default.htm)在[一点点 JavaScript 代码](http://adripofjavascript.com/blog/drips/equals-equals-null-in-javascript.html)

在 JavaScript 中检查`null`的一种方法是使用[双等式](https://medium.com/better-programming/making-sense-of-vs-in-javascript-f9dbbc6352e3) `[==](https://medium.com/better-programming/making-sense-of-vs-in-javascript-f9dbbc6352e3)` [运算符](https://medium.com/better-programming/making-sense-of-vs-in-javascript-f9dbbc6352e3)检查一个值是否宽松地等于`null`:

如上所示，`null`只是松散地等于自身和`undefined`，而不是所示的其他 falsy 值。

这对于检查值的缺失很有用— `null`和`undefined`都表示值的缺失，因此它们大致相等(尽管它们是不同的类型，但它们具有相同的值)。

因此，在试图处理变量之前，当编程检查变量是否有任何值时，可以使用`== null`来检查`null`或`undefined`。

![](img/f4ee0867502631765b79053078b24696.png)

Photo by [David Becker](https://unsplash.com/@beckerworks?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 使用`===`的严格等式

到确保我们有一个确切的`null`值，排除任何`undefined`值，使用[三重等式](https://medium.com/better-programming/making-sense-of-vs-in-javascript-f9dbbc6352e3) `[===](https://medium.com/better-programming/making-sense-of-vs-in-javascript-f9dbbc6352e3)` [运算符](https://medium.com/better-programming/making-sense-of-vs-in-javascript-f9dbbc6352e3)就可以了:

通常，捕捉`null`和`undefined`值是一个好主意，因为这两个值都表示缺少值。

这意味着检查`null`是 JavaScript 中少数几个推荐使用`==`的时候之一，而[否则](https://medium.com/better-programming/making-sense-of-vs-in-javascript-f9dbbc6352e3) `[===](https://medium.com/better-programming/making-sense-of-vs-in-javascript-f9dbbc6352e3)` [一般推荐](https://medium.com/better-programming/making-sense-of-vs-in-javascript-f9dbbc6352e3)。

![](img/848a1df10e369b0ed54209894f4f49ec.png)

Photo by [Dan Meyers](https://unsplash.com/@dmey503?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 检查空值时比较== vs ===

为了清晰起见，一些 JavaScript 程序员更喜欢一切都是显式的，这没有错。

事实上，代码 linter JSLint [明确禁止](https://jslint.com/help.html) `[==](https://jslint.com/help.html)`来防止类型强制导致的意外错误。

另一个流行的代码 linter，ESLint，在使用`==`和`===`时有[相似但更可配置的行为](https://eslint.org/docs/rules/eqeqeq)。

这意味着，如果您(或您的 linter)习惯于总是使用严格的等式运算符`===`，那么您可以检查一个值是否严格等于 null 或者(`||`)是否严格等于 undefined，而不是使用`==`:

它比`==`操作符更冗长，但是每个阅读您代码的人都会清楚地知道`null`和`undefined`都被检查。

![](img/821212f3aec3a1d16d3608b8395deeb2.png)

Photo by [Cassie Boca](https://unsplash.com/@cassieboca?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 何时检查空值的真实示例

> “在现实世界的例子中，如果您试图在加载元素之前在 JavaScript 中使用 DOM 元素，可能会出现这种错误['null 不是对象']。这是因为 DOM API 为空白的对象引用返回`null`。— [十大 JavaScript 错误滚动条](https://rollbar.com/blog/top-10-javascript-errors/)

T 如果在加载脚本之前没有创建 DOM 元素，例如，如果脚本高于页面上的 HTML，从上到下进行解释，则会发生此[type error](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypeError)(“`null`不是对象”)。

解决方案是使用一个事件监听器，它会在页面准备好的时候通知我们，然后运行脚本。

但是，在试图访问 DOM 元素之前检查它是否是`null`可能是谨慎的。

![](img/2a45b543de0f8382453bc93b8b2faf43.png)

Photo by [Denys Nevozhai](https://unsplash.com/@dnevozhai?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 使用具有虚假力量的类型

> “谢天谢地，因为`null`不是一个真正的对象，它是唯一一个假值的‘对象’，所以空对象是真的。”— [凯西·莫里斯](https://medium.com/u/c194ff39a976?source=post_page-----dffab64d8ed5--------------------------------)在[日报](https://medium.com/dailyjs/rant-js-undefined-vs-null-7f90f203063b)

另一种检查`null`的方法是基于这样一个事实，即`null`是[假](https://medium.com/coding-at-dawn/what-are-falsy-values-in-javascript-ca0faa34feb4?)，但是空对象是[真](https://medium.com/coding-in-simple-english/what-are-truthy-values-in-javascript-e037bdfa76f8)，所以`null`是唯一的假对象。

这可以使用[逻辑非](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_Operators#Description) `[!](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_Operators#Description)` [运算符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_Operators#Description)方便地进行检查:

使用`typeof`可能是一个有用的技巧，因为如果一个变量未声明，那么试图引用它将抛出一个`[ReferenceError](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ReferenceError)`。

但是，`typeof`一个未声明的值是`undefined`，所以使用`typeof`可以是检查`null`、`undefined`和未声明变量的好方法。

![](img/82bb7a26728bad709297f1608c8b8f76.png)

Photo by [Dylan Freedom](https://unsplash.com/@d4rock?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 使用`Object.is()`

T 他 [ES6 函数](https://medium.com/coding-at-dawn/es6-object-is-vs-in-javascript-7ce873064719) `[Object.is()](https://medium.com/coding-at-dawn/es6-object-is-vs-in-javascript-7ce873064719)`不同于[严格的](https://medium.com/better-programming/making-sense-of-vs-in-javascript-f9dbbc6352e3) `[===](https://medium.com/better-programming/making-sense-of-vs-in-javascript-f9dbbc6352e3)` [和宽松的](https://medium.com/better-programming/making-sense-of-vs-in-javascript-f9dbbc6352e3) `[==](https://medium.com/better-programming/making-sense-of-vs-in-javascript-f9dbbc6352e3)` [等式运算符](https://medium.com/better-programming/making-sense-of-vs-in-javascript-f9dbbc6352e3)在于它如何[检查为](https://medium.com/coding-in-simple-english/how-to-check-for-nan-in-javascript-4294e555b447) `[NaN](https://medium.com/coding-in-simple-english/how-to-check-for-nan-in-javascript-4294e555b447)`和[【负零】](https://medium.com/coding-at-dawn/is-negative-zero-0-a-number-in-javascript-c62739f80114) `[-0](https://medium.com/coding-at-dawn/is-negative-zero-0-a-number-in-javascript-c62739f80114)`。

对于空值，`Object.is()`的行为与`===`相同:

这意味着如果您使用`Object.is()`，您将需要显式地检查`null`和`undefined`，这是在 React 中检查状态[变化的辅助方法。](https://medium.com/coding-at-dawn/es6-object-is-vs-in-javascript-7ce873064719)

![](img/63494535c8fc38a6031f89556aa5492c.png)

Photo by [Patrick Hendry](https://unsplash.com/@worldsbetweenlines?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

检查空值是每个 JavaScript 开发人员在某个时候都必须执行的一项常见任务。

`typeof`关键字为`null`返回`"object"`，所以这意味着需要多一点努力。

可以进行比较:`null === null`严格检查空值，或者`null == undefined`宽松地检查空值或未定义值。

值`null`是 falsy，但是空对象是 true，所以`typeof maybeNull === "object" && !maybeNull`是检查值不是`null`的一个简单方法。

最后，要检查一个值是否已经被声明并被分配了一个既不是`null`也不是`undefined`的值，使用`typeof`:

现在走出去，满怀信心地检查`null`！

![](img/4f57abd9c6722e3333ec8bfba674df46.png)

Photo by [Puk Patrick](https://unsplash.com/@macpukpro?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 额外资源

*   康斯坦丁·布洛欣在 [freeCodeCamp](https://www.freecodecamp.org/news/avoiding-null-check-pollution-in-javascript-4ed8e2702ce3/) 清理“零检查污染”:

[](https://www.freecodecamp.org/news/avoiding-null-check-pollution-in-javascript-4ed8e2702ce3/) [## 如何避免 JavaScript 中的空检查污染:使用选项

### 如何避免 JavaScript 中的空检查污染:使用选项清洗你的代码…

www.freecodecamp.org](https://www.freecodecamp.org/news/avoiding-null-check-pollution-in-javascript-4ed8e2702ce3/) 

*   Casey Morris 在 [DailyJS](https://medium.com/dailyjs/rant-js-undefined-vs-null-7f90f203063b) 上对 null 和 undefined 有一些看法:

[](https://medium.com/dailyjs/rant-js-undefined-vs-null-7f90f203063b) [## Rant.js —未定义 vs 空

### 有关系吗？它们都是行为相似的虚假价值观，这不是有点傻吗？我不这么认为。

medium.com](https://medium.com/dailyjs/rant-js-undefined-vs-null-7f90f203063b) 

*   [阿比纳夫·西兰](https://medium.com/u/89342c85a990?source=post_page-----dffab64d8ed5--------------------------------)解释了为什么`null >= 0`在[香草营地](https://blog.campvanilla.com/javascript-the-curious-case-of-null-0-7b131644e274)结束了`true`:

[](https://blog.campvanilla.com/javascript-the-curious-case-of-null-0-7b131644e274) [## JavaScript:Null > = 0 的奇怪情况

### 为什么阅读 Javascript 规范很重要

blog.campvanilla.com](https://blog.campvanilla.com/javascript-the-curious-case-of-null-0-7b131644e274) 

*   [Kiro Risk](https://medium.com/u/6c3b1647d032?source=post_page-----dffab64d8ed5--------------------------------) 在 [JavaScript Refined](https://javascriptrefined.io/null-and-typeof-9330e475d272) 有一篇关于 null 和 typeof 的文章:

[](https://javascriptrefined.io/null-and-typeof-9330e475d272) [## Null 和 typeof

### 一劳永逸地揭开 null 类型的神秘面纱

javascriptrefined.io](https://javascriptrefined.io/null-and-typeof-9330e475d272) 

*   [Hackernoon](https://medium.com/u/4a8a924edf41?source=post_page-----dffab64d8ed5--------------------------------) 作者 [Yuri Ramos](https://medium.com/u/c3b3dca692ab?source=post_page-----dffab64d8ed5--------------------------------) 的[第一个提示](https://hackernoon.com/12-amazing-javascript-shorthand-techniques-fef16cdbc7fe)展示了如何使用`let variable2 = variable1 || ''`在一行中检查 null 或未定义:

[](https://hackernoon.com/12-amazing-javascript-shorthand-techniques-fef16cdbc7fe) [## 12 种优秀的 JavaScript 速记技巧

### 更新 1:由于很多两极分化的评论(如喜欢或讨厌这篇文章)，我只是想明确表示速记…

hackernoon.com](https://hackernoon.com/12-amazing-javascript-shorthand-techniques-fef16cdbc7fe) 

*   Alex Ellis 在他的博客上深入探究了`typeof null`被窃听的原因[:](https://alexanderell.is/posts/typeof-null/)

 [## null 的类型:调查一个经典的 JavaScript 错误

### 在我的上一篇帖子中，我研究了一些 JavaScript 选角，并探究了为什么 0<= null evaluates as true. For this post, I’d…

alexanderell.is](https://alexanderell.is/posts/typeof-null/) ![](img/52a40799db68d4e9ffc56600ce993cbc.png)

Photo by [Toby Christopher](https://unsplash.com/@tobychristopher?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

[Derek Austin](https://www.linkedin.com/in/derek-austin/)博士是《职业编程:如何在 6 个月内成为 6 位数成功的程序员 》一书的作者，该书现已在亚马逊上出售。