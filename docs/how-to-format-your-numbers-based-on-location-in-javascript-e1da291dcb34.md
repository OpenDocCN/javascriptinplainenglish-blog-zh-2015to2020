# 如何在 JavaScript 中根据位置格式化数字

> 原文：<https://javascript.plainenglish.io/how-to-format-your-numbers-based-on-location-in-javascript-e1da291dcb34?source=collection_archive---------5----------------------->

![](img/68bbfe80de686f0c53cc9ccdd511505b.png)

Photo by [Jason Leung](https://unsplash.com/@ninjason?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/currency?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

我在做一个试图兑换货币的项目时遇到了一个问题。这不是进行转换的问题，但如果我想用逗号或句点分隔数字，该怎么办呢？在美国，对于 5000 美元的价格，通常写为$5，000.00，但在另一个国家，它可能写为€5.000，00。我将从两个方面向您展示如何做到这一点。

# number . prototype . tolocalestring()

`.toLocaleString()`方法很简单，因为它接受一个带选项的参数。

1.  **区域设置[，选项]**

MDN 注释:

> 当格式化大量数字时，最好创建一个`[NumberFormat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/NumberFormat)`对象，并使用其`[NumberFormat.format](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/NumberFormat/format)`属性提供的函数。

让我们举个例子:

通过放置参数`"it-it"`,我能够根据特定的区域设置格式化我的`num`。*如果你对自己或他人的地区代码很好奇，这个* [*网站*](https://www.science.co.il/language/Locale-codes.php#definitions) *很有帮助。没有争论地离开它，我能够回到我目前所在的地方。*

**现在你可能会问有哪些选择？**

选项部分实际上非常酷，因为你可以添加你想要指定的格式(**我们可以使用一大堆选项，如百分比和货币**)。我提到过我在一个基于货币的项目中使用这个选项，我会让用户从下拉列表中选择他们的货币，这将被实现到选项参数中。我将向您展示一种使用选项的方法。

您将选项作为对象添加。现在，它将告诉方法，我们希望货币符号和货币数字根据带有货币符号的地区进行正确格式化。这正是我们得到的！

虽然这是可以添加到函数中的简单快捷的方法，但是您可能会发现自己需要使用`[Intl.NumberFormat()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/NumberFormat/NumberFormat)` [构造函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/NumberFormat/NumberFormat)

## `Intl.NumberFormat()`建造师

与`.toLocaleString()`不同，`Intl.NumberFormat()`可以接受许多参数，包括地区和选项。*查看文档以满足您的需求。*

我们使用`new`关键字创建`Intl.NumberFormat()`构造函数，然后添加选项对象。同样，我使用的是“货币”风格。现在，为了显示我们的结果，我们使用了 `.format()`并在括号内添加了`curr`变量。如果您正在处理较大的数字，这是 MDN 推荐的方法，因此如果您不知道数字有多大，使用`Intl.NumberFormat()`可能会更容易。

其他选项也是如此:

这里我使用了“百分比”样式选项，它将我的数字转换成带有百分比符号的百分比。

# 结论

我希望这有助于处理数字和格式。查看 MDN → `[.toLocaleString()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/toLocaleString)`和`[Intl.NumberFormat()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/NumberFormat/NumberFormat)`上的文档