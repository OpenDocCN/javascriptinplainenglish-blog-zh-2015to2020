# 如何在 JavaScript 中检查布尔值

> 原文：<https://javascript.plainenglish.io/how-to-check-for-a-boolean-in-javascript-98fdc8aec2a7?source=collection_archive---------1----------------------->

## 布尔值`true`或`false`，是 JavaScript 中最容易检查的原语之一——只需使用`typeof`操作符。

![](img/9ec9e3b95e70ae25261b9df72f6e14b4.png)

Photo by [Wouter Meijering](https://unsplash.com/@woetah?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 布尔人是`true`或`false`

C 检查[布尔值](https://en.wikipedia.org/wiki/Boolean_data_type)在 JavaScript 中很容易——它们要么是`true`要么是`false`，而`typeof` a [布尔值](https://developer.mozilla.org/en-US/docs/Glossary/Boolean)总是`"boolean"`。

> “布尔类型—布尔表示逻辑实体，可以有两个值:`true`和`false`。详见[布尔](https://developer.mozilla.org/en-US/docs/Glossary/Boolean)和`[Boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)`。”— [MDN 文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures)

一般来说，我们使用关键字`true`和`false`来表示[布尔值](https://www.typescriptlang.org/docs/handbook/basic-types.html#boolean)，尽管有时我们会依赖真值和假值，稍后我会对此进行解释。

以下代码显示了如何在 JavaScript 中检查布尔值:

# 如何在 JavaScript 中产生布尔值

到访问原始数据类型，布尔型，我们通常使用关键字`true`和`false`，这两个关键字是[留给布尔型](https://www.tutorialrepublic.com/javascript-reference/javascript-reserved-keywords.php)的。

然而，我们也可以将其他变量评估为含义`true`或`false`。

> “除了`*boolean*`原始类型，JavaScript 还为您提供了全局`*Boolean()*`函数，用大写字母`*B*`将另一种类型的值转换为`*boolean*`。”— [JavaScript 教程](https://www.javascripttutorial.net/javascript-boolean/)

我们可以通过以下三种方式之一将 JavaScript 中的任何表达式强制转换为布尔值:

1.  使用`[Boolean()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)`的[包装函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)。
2.  将[两个感叹号](https://medium.com/better-programming/javascript-bang-bang-i-shot-you-down-use-of-double-bangs-in-javascript-7c9d94446054)`[!!](https://medium.com/better-programming/javascript-bang-bang-i-shot-you-down-use-of-double-bangs-in-javascript-7c9d94446054)`([逻辑非](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_Operators#Description) `[!](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_Operators#Description)` 运算符，两次)放在任意表达式前面，调用`Boolean()`包装函数。
3.  将任何表达式放入[条件](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/conditionals)中，如`[if](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else)` [语句](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else)、[问号](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean) `[?](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)` [运算符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)或[循环](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Loops_and_iteration) —有或没有`[&&](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_Operators)`或[或](https://mariusschulz.com/blog/the-and-and-or-operators-in-javascript) `[||](https://mariusschulz.com/blog/the-and-and-or-operators-in-javascript)`。

以下是在 JavaScript 中将表达式强制转换为布尔值的示例:

# JavaScript 中的 Truthy 和 falsy 布尔值

在条件句中，计算结果为`false`的值称为[***falsy****values*，](https://medium.com/coding-at-dawn/what-are-falsy-values-in-javascript-ca0faa34feb4)以及 JavaScript 中的其他所有内容都是计算结果为`true`的[***truthy****values*](https://developer.mozilla.org/en-US/docs/Glossary/Boolean)。

JavaScript 中的 falsy 值有`false`、`0`、`[-0](https://medium.com/coding-at-dawn/is-negative-zero-0-a-number-in-javascript-c62739f80114)`、`0n`、`[null](https://medium.com/javascript-in-plain-english/how-to-check-for-null-in-javascript-dffab64d8ed5)`、`[undefined](https://medium.com/coding-at-dawn/how-to-check-for-undefined-in-javascript-bcedd62c8ad)`、`[NaN](https://medium.com/coding-in-simple-english/how-to-check-for-nan-in-javascript-4294e555b447)`、[空字符串](https://medium.com/javascript-in-plain-english/how-to-check-for-a-string-in-javascript-a16b196915ff)`[""](https://medium.com/javascript-in-plain-english/how-to-check-for-a-string-in-javascript-a16b196915ff)`；其他一切都是真理价值。

所有对象都是真的，包括空对象`{}`和数组`[]`。

以下代码给出了 truthy 和 falsy 值的一些示例:

# 结论:JavaScript 布尔值的类型检查

如果你需要确定你有一个布尔原始值，而不仅仅是一个伪值，[使用](https://medium.com/better-programming/how-to-check-data-types-in-javascript-using-typeof-424d0520a329) `[typeof](https://medium.com/better-programming/how-to-check-data-types-in-javascript-using-typeof-424d0520a329)`检查 JavaScript 变量的类型。

只有`true`和`false`的`typeof`等于 `"boolean"`。

您可以使用`Boolean()`包装函数将任何 JavaScript 表达式强制转换为布尔原语类型，该函数在全局对象上可用。

也可以使用`!!` ( *Bang！砰！*)。

这就是检查布尔值的全部内容！😊😂❤️😍👌

# 你知道布尔这个术语是从哪里来的吗？

> 布尔值以英国数学家[乔治·布尔](https://en.wikipedia.org/wiki/George Boole)命名，他是数学逻辑领域的先驱。— [MDN 文档](https://developer.mozilla.org/en-US/docs/Glossary/Boolean)

# 进一步阅读

*   [Patrick Divine](https://medium.com/u/7397926645f9?source=post_page-----98fdc8aec2a7--------------------------------) 在他的中型博客上分解双刘海(`!!` ) [:](https://medium.com/better-programming/javascript-bang-bang-i-shot-you-down-use-of-double-bangs-in-javascript-7c9d94446054)

[](https://medium.com/better-programming/javascript-bang-bang-i-shot-you-down-use-of-double-bangs-in-javascript-7c9d94446054) [## Javascript“砰，砰。我把你打趴下了”——使用双刘海(！！)在 Javascript 中。

### 如何在使用双刘海时不被击落

medium.com](https://medium.com/better-programming/javascript-bang-bang-i-shot-you-down-use-of-double-bangs-in-javascript-7c9d94446054) 

*   [马里乌斯·舒尔茨](https://medium.com/u/b69f343626a4?source=post_page-----98fdc8aec2a7--------------------------------)在他的博客中解释了 Javascript 的`&&` (和)`||`(或)[:](https://mariusschulz.com/blog/the-and-and-or-operators-in-javascript)

[](https://mariusschulz.com/blog/the-and-and-or-operators-in-javascript) [## JavaScript 中的&&和||运算符

### 与其他类似 C 的编程语言类似，JavaScript 定义了两个运算符&&和||来表示…

mariusschulz.com](https://mariusschulz.com/blog/the-and-and-or-operators-in-javascript) 

*   [布兰登·莫雷利](https://medium.com/u/e9031892baf5?source=post_page-----98fdc8aec2a7--------------------------------)解释`&&, ||`，Codeburst.io 中`!`(不是)[:](https://codeburst.io/learn-javascript-logical-and-or-not-ee687d08b1ad)

[](https://codeburst.io/learn-javascript-logical-and-or-not-ee687d08b1ad) [## 学习 JavaScript:逻辑 AND / OR / NOT

### 学习并理解 JavaScript 逻辑运算符:AND (&&)、OR (||)和 NOT(！)

codeburst.io](https://codeburst.io/learn-javascript-logical-and-or-not-ee687d08b1ad) 

*   [克里斯·洛夫](https://medium.com/u/514b97322c72?source=post_page-----98fdc8aec2a7--------------------------------)在他的博客上讨论了`!!`运营商[的“巫术”:](https://twitter.com/ChrisLove/)

[](https://love2dev.com/blog/javascript-not-operator/) [## 这是什么！！JavaScript 中的(不是 not)运算符？

### 前几天，我翻阅了一些 JavaScript 代码，想弄清楚第三方库是如何运行的。当我扫描…

love2dev.com](https://love2dev.com/blog/javascript-not-operator/) 

*   网站[freeCodeCamp.org](https://www.freecodecamp.org/forum/t/javascript-booleans-explained-how-to-use-booleans-in-javascript/14311)对 JavaScript 布尔值进行了概述:

[](https://www.freecodecamp.org/forum/t/javascript-booleans-explained-how-to-use-booleans-in-javascript/14311) [## JavaScript 布尔解释了如何在 JavaScript 中使用布尔

### 布尔布尔是计算机编程语言中常用的一种原始数据类型。根据定义，布尔值有…

www.freecodecamp.org](https://www.freecodecamp.org/forum/t/javascript-booleans-explained-how-to-use-booleans-in-javascript/14311) 

*   [JavaScript 教程](https://www.javascripttutorial.net/javascript-boolean/)关于为什么`Boolean()`好，而`new Boolean()`不好:

 [## JavaScript 布尔型与布尔型:举例说明

### 摘要:在本教程中，您将了解 JavaScript 布尔对象和布尔对象之间的区别

www.javascripttutorial.net](https://www.javascripttutorial.net/javascript-boolean/) ![](img/dd3cd53da24ee140c8ebe260054fc889.png)

Photo by [Wouter Meijering](https://unsplash.com/@woetah?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

[Derek Austin](https://www.linkedin.com/in/derek-austin/)博士是《职业编程:如何在 6 个月内成为一名成功的 6 位数程序员 》一书的作者，该书现已在亚马逊上架。