# 这是什么？JavaScript 代码中问号的意思？

> 原文：<https://javascript.plainenglish.io/what-does-the-question-mark-mean-in-javascript-code-353cfadcf760?source=collection_archive---------1----------------------->

## 问号`?`是`if`语句的替代语句，最适用于基于条件语句将两个值之一赋给变量的情况。

![](img/a1e70a383262a3f10e918e966ba73a73.png)

Photo by [Jens Lelie](https://unsplash.com/@leliejens?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

> 问号运算符`?`的目的是根据条件返回一个或另一个值。— [JavaScript.info](https://javascript.info/) 于[2019 年 8 月](https://javascript.info/ifelse)

" A`?`" " A`?`在我的 JavaScript 代码中做什么？"当字符`?`出现在我的 JavaScript 编程中时，它跳出了屏幕。

幸运的是，问号(或问号-冒号)语法很容易理解，但是过度使用`? :`会使代码难以阅读。

问号在 JavaScript 中用作`if`语句的替代，尤其是在基于条件赋值的情况下。

![](img/e09dcecb2399dff6af911929bfdf67cc.png)

Photo by [Murat Düzyurt](https://unsplash.com/@muratid?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 另一个名字的问号

> JavaScript 中的“问号”或“条件”运算符是一个三元运算符，有三个操作数。— [GeeksForGeeks](https://www.geeksforgeeks.org/ternary-operator-question-mark-and-colon-in-javascript/)

它有很多名字:问号、问号—冒号、三元运算符。虽然语法非常简单，但是由于术语的原因，大声描述 JavaScript 问号`?`可能很困难。

问号操作符`?`接受三个操作数:某个条件，如果该条件为真，则接受一个值，如果该条件为假，则接受一个值。

它在 JavaScript 中用于将一条`if else`语句缩短为一行代码。

![](img/2f86482241c103a35cc16aea9d0bd75e.png)

Photo by [Saketh Garuda](https://unsplash.com/@sakethgaruda?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 三个一类——三元运算符

> **条件(三元)操作符**是唯一一个接受三个操作数的 JavaScript 操作符。该运算符常用作`[if](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else)`语句的快捷方式。— [MDN 网络文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)

作为一个带三个操作数的运算符，问号也被称为三元运算符——三元含义由三部分组成:

1.  条件在括号中
2.  如果为真，则先给出值
3.  第二个是 false 值

三个操作数看起来是这样的:`(1+1==2) ? "Pass" : "Fail"`。问号和冒号运算符合起来就是三元运算符。

![](img/3bd7e08287eec6876a481f89c7befe9a.png)

Photo by [Veronica Reverse](https://unsplash.com/@vereverse?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 看看我的`?`

问在 JavaScript 中，问号非常有用，可以根据条件快速给变量赋值，这是一项非常常见的任务。

奖金的确定只需要一行代码，所以我可以利用节省下来的时间编写`if`、`else`和`{}`程序块。

从技术上讲，括号是可选的，但是它们可以提高可读性:

![](img/135c7c37cdfa8ec427d5f882942daab6.png)

Photo by [Cosmic Timetraveler](https://unsplash.com/@cosmictimetraveler?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 小心`?`

ost 开发人员认为这段代码很难阅读，尽管问号运算符让我只用一行代码就能写出来:

将此与用`if`语句编写的替代方案进行比较:

典型的最佳实践是，在使用问号 JavaScript 中的冒号——有意义时，将问号保留在分配变量的情况下。

在其他情况下，如果某事为真，我会采取一些行动，然后我会尽量坚持使用 if 语句，以便使我的代码更清晰。

![](img/44c0644f55d93aac40cd68fd62721495.png)

Photo by [Michal Mrozek](https://unsplash.com/@miqul?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 额外资源

*   [Tania Rascia](https://medium.com/u/d2cf0a29e5ba?source=post_page-----353cfadcf760--------------------------------) 解释 [DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-write-conditional-statements-in-javascript) 上的条件语句:

[](https://www.digitalocean.com/community/tutorials/how-to-write-conditional-statements-in-javascript) [## If 语句、If Else 语句、嵌套 If、三元运算符|数字海洋

### 在编程中，有很多情况下你会希望不同的代码块根据用户而运行…

www.digitalocean.com](https://www.digitalocean.com/community/tutorials/how-to-write-conditional-statements-in-javascript) 

*   斯蒂芬·查普曼在 [ThoughtCo](https://www.thoughtco.com/javascript-by-example-use-of-the-ternary-operator-2037394) 上解释了他喜欢`?`的地方:

[](https://www.thoughtco.com/javascript-by-example-use-of-the-ternary-operator-2037394) [## JavaScript 中使用“三元”运算符的有效快捷方式

### JavaScript 中的条件三元运算符根据某种条件给变量赋值，并且是唯一的…

www.thoughtco.com](https://www.thoughtco.com/javascript-by-example-use-of-the-ternary-operator-2037394) 

*   [布兰登·莫雷利](https://medium.com/u/e9031892baf5?source=post_page-----353cfadcf760--------------------------------)喜欢三进制运算符在 [codeburst.io](https://codeburst.io/javascript-the-conditional-ternary-operator-explained-cac7218beeff) 上的简洁:

[](https://codeburst.io/javascript-the-conditional-ternary-operator-explained-cac7218beeff) [## JavaScript——解释了条件(三元)运算符

### 用条件运算符将 if 语句缩短为一行代码

codeburst.io](https://codeburst.io/javascript-the-conditional-ternary-operator-explained-cac7218beeff) 

[Derek Austin](https://www.linkedin.com/in/derek-austin/)博士是《职业编程:如何在 6 个月内成为一名成功的 6 位数程序员 》一书的作者，该书现已在亚马逊上架。