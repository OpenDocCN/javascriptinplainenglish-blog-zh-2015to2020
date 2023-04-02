# 如何在 JavaScript 中检查字符串

> 原文：<https://javascript.plainenglish.io/how-to-check-for-a-string-in-javascript-a16b196915ff?source=collection_archive---------0----------------------->

## 当检查字符串时，`typeof`工作得很好，除非有人使用原始对象包装器，这被认为是糟糕的代码。

![](img/b2b414dee9a88c326ab92665c8b73403.png)

Photo by [David Clode](https://unsplash.com/@davidclode?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 检查原始字符串

在 JavaScript 中检查有些事情有点棘手，比如检查 NaN 的[，但谢天谢地，处理字符串要容易得多。](https://medium.com/coding-in-simple-english/how-to-check-for-nan-in-javascript-4294e555b447)

在 JavaScript 中检查值是否为字符串时，`[typeof](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof)`非常有用:

嗯，`typeof`在用`new String()`创建的字符串的情况下不能像预期的那样工作，这就是所谓的[原始对象包装器](http://adripofjavascript.com/blog/drips/javascripts-primitive-wrapper-objects.html)。

像这样创建“对象包装”的字符串很少使用，通常也不推荐使用，我将在下一节中探讨。

![](img/3ede56dd2653811ba388e64c3ac8acb0.png)

Photo by [Jeremy Bishop](https://unsplash.com/@jeremybishop?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 为什么原始对象包装器不好？

因为网上几乎每个人都说 JavaScript 对象包装不好，令人困惑，应该被淘汰。例如:

1.  谷歌 JavaScript 风格指南[说永远不要使用原始对象包装器，你们这些淘气的开发者](https://google.github.io/styleguide/jsguide.html#disallowed-features-wrapper-objects)。
2.  道格拉斯·克洛克福特[公开建议弃用基本对象包装器](http://www.crockford.com/javascript/recommend.html)——官方不鼓励使用它们。

其实我也没见过有人推荐他们用。原始对象包装器似乎受到了普遍的指责。

在下一节中，我将回顾人们需要了解的关于它们的知识。

![](img/54c7b78fb1e948ee0042ceab43bdcb5f.png)

Photo by [John Salzarulo](https://unsplash.com/@johnsalzarulo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 如何不使用对象包装器

实际上，包装器出现的唯一时间是在“Gotcha！”风格面试问题，通常关于使用`typeof`的类型检查。

事实上，在下面三种调用字符串的方法中，我认为几乎所有的 JavaScript 程序员都只知道第一种。

另外两种语法通常应该避免混淆:

![](img/1002bf8caa6140b4c8222e9441e86262.png)

Photo by [Kara Eads](https://unsplash.com/@karaeads?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 对象包装令人困惑

> “使用 new 运算符显式地创建包装器被认为是一种不好的做法。另一方面，在没有 new 运算符的情况下调用函数是完全有效的，因为它只是试图将输入转换为相应的基元类型并返回一个基元值。
> 
> ——[růžička](https://medium.com/u/a9dbdbb52335?source=post_page-----a16b196915ff--------------------------------)在[他的博客](https://www.vojtechruzicka.com/javascript-primitives/)

在中，除了改变`typeof`返回的内容，还有一件关于原始对象包装器的事情令人困惑:[类型强制](https://www.valentinog.com/blog/coercion/)。

同样的函数可以在没有关键字`new`的情况下使用，以便将一个值强制转换为原始类型。看起来是这样的:

因此，使用不带`new`关键字的包装函数是一种将值强制转换为原始类型的有用方法。

另一方面，使用`new`关键字创建了一个对象包装的原语，这被认为是**坏运气**。

![](img/410f05dd667de3d5205030d46a2d70ca.png)

Photo by [Kyle Glenn](https://unsplash.com/@kylejglenn?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 如果包装是坏的，为什么包装还会存在？

简单地说，这些原语对象包装器只存在于 JavaScript 中，以允许原语类型像对象一样工作。

当代码引用一个原型函数或属性时，JavaScript 自动在后台调用原语对象包装器。

例如，当使用[string . prototype . length](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/length)属性检查字符串的长度或使用[string . prototype()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace)函数替换字符串中的所有子字符串时，会出现这种情况:

在这两种情况下，JavaScript 调用对象包装器，以便能够引用对象的属性和方法。

![](img/702cfc5a6e24a6bfc5c121b8bf33ffb2.png)

Photo by [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 根据对象包装进行检验

如果你的代码碰巧被大量的原始对象包装器弄得千疮百孔，该怎么办？在这种情况下，如何检查字符串呢？

技巧是了解两个额外的 JavaScript 特性——`[instanceof](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/instanceof)`关键字和`[Object.prototype.toString.call()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/toString)`函数。

以下是如何使用这些来改进`isString()`功能:

![](img/ba88bd70e02c937ed84cce5e00fe063f.png)

Photo by [Andrew Seaman](https://unsplash.com/@amseaman?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

一般来说，在日常代码中检查字符串时，`typeof`会让你走得很远。

通常，被反对的包装字符串只作为 JavaScript 测试问题存在，但是如果你需要检查它们，它们很容易处理。

对于对象包装的原语，`instanceof`关键字和`Object.prototype.toString.call()`函数将揭示一个值的类型。

![](img/23f6c67a90ba2ee7af58f7af24e8a837.png)

Photo by [Corey Grover](https://unsplash.com/@suburbanmillennial?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 额外资源

*   [Nikhil Swain](https://medium.com/u/7f142403bda?source=post_page-----a16b196915ff--------------------------------) 讨论了在初创公司中使用 JavaScript 字符串:

[](https://medium.com/swlh/working-with-strings-in-javascript-34060a1c17a9) [## 在 JavaScript 中使用字符串

### HTML 是基于文本的，所以如果你在网页上读或写数据，不可避免的是你必须…

medium.com](https://medium.com/swlh/working-with-strings-in-javascript-34060a1c17a9) 

*   [Samantha Ming](https://medium.com/u/829a804ea5da?source=post_page-----a16b196915ff--------------------------------) 在 [Daily JS](https://medium.com/dailyjs/5-ways-to-convert-a-value-to-string-in-javascript-6b334b2fc778) 中谈到了将值转换为字符串:

[](https://medium.com/dailyjs/5-ways-to-convert-a-value-to-string-in-javascript-6b334b2fc778) [## 在 JavaScript 中将值转换为字符串的 5 种方法

### 让我们看看在 JavaScript 中将值转换成字符串的不同方法。从 Airbnb 的首选方式…

medium.com](https://medium.com/dailyjs/5-ways-to-convert-a-value-to-string-in-javascript-6b334b2fc778) 

*   [Trey Huffine](https://medium.com/u/47e700e59e44?source=post_page-----a16b196915ff--------------------------------) 在 [gitconnected](https://levelup.gitconnected.com/essential-javascript-string-methods-f1841dad1961) 中写了许多字符串方法:

[](https://levelup.gitconnected.com/essential-javascript-string-methods-f1841dad1961) [## 基本的 JavaScript 字符串方法

### 处理字符串的 13 个最重要的 JavaScript 函数。索引，切片，分裂，修剪你的方式通过 JS…

levelup.gitconnected.com](https://levelup.gitconnected.com/essential-javascript-string-methods-f1841dad1961) 

*   [CodeDraken](https://medium.com/u/79e2592263f8?source=post_page-----a16b196915ff--------------------------------) 覆盖了 [codeburst](https://codeburst.io/javascript-essentials-types-data-structures-3ac039f9877b) 的类型和数据结构；

[](https://codeburst.io/javascript-essentials-types-data-structures-3ac039f9877b) [## JavaScript 基础:类型和数据结构

### Essentials 是一个系列，涵盖了 X topic 最常用和最重要的方法。这是一个面向开发人员的系列…

codeburst.io](https://codeburst.io/javascript-essentials-types-data-structures-3ac039f9877b) 

*   [Sonya Moisset](https://medium.com/u/dbdd9894be12?source=post_page-----a16b196915ff--------------------------------) 演示了如何在 [freeCodeCamp](https://www.freecodecamp.org/news/how-to-reverse-a-string-in-javascript-in-3-different-ways-75e4763c68cb/) 中反转字符串:

[](https://www.freecodecamp.org/news/how-to-reverse-a-string-in-javascript-in-3-different-ways-75e4763c68cb/) [## JavaScript 中反转字符串的三种方法

### 这篇文章是基于自由代码营的基本算法脚本“反转一个字符串…

www.freecodecamp.org](https://www.freecodecamp.org/news/how-to-reverse-a-string-in-javascript-in-3-different-ways-75e4763c68cb/) 

德里克·奥斯汀博士是《职业规划:如何在 6 个月内成为一名成功的 6 位数程序员》一书的作者，该书现已在亚马逊上出售。