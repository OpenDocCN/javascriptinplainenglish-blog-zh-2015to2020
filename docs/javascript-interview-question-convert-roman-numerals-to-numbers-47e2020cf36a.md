# JavaScript 面试问题:将罗马数字转换成数字

> 原文：<https://javascript.plainenglish.io/javascript-interview-question-convert-roman-numerals-to-numbers-47e2020cf36a?source=collection_archive---------10----------------------->

## 用于面试准备和日常编码的罗马数字 JavaScript 指南

![](img/2655d6a006fca4b689a56160cd2c4378.png)

Image credit: Author

[罗马数字](https://en.wikipedia.org/wiki/Roman_numerals)是一种起源于古罗马的数字体系。今天，它仍然在一些场合使用。罗马数字的现代用法使用七个符号，每个符号都被赋予一个整数值:

*   一:1
*   五:5
*   十点十分
*   L: 50
*   列车员:100 元
*   D: 500
*   男:1000

罗马数字由 MDCLXVI 的大写字母组成。最大的符号写在左边，最小的符号写在右边，与阿拉伯数字一致。比如 MDCLXVI 代表 1666 `(1000 + 500 + 100 + 50 + 10 + 5 + 1)`。一个符号可以重复，比如 III (3)。一般来说，所有的符号都是令人上瘾的符号。然而，当一个较小的符号在左边时，它定义了一个减法符号。这些都是减法符号:

*   四(`5 — 1 = 4`)
*   九世(`10 – 1 = 9`)，
*   XL ( `50 — 10 = 40`)
*   XC ( `100 - 10 = 90`
*   CD ( `500 — 100 = 400`)
*   CM ( `1000 — 100 = 900`)

有时候，罗马数字会出现在真实的面试中。掌握原理，通过面试题很重要。

# **将罗马数字转换成数字**

我们被要求写一个将罗马数字转换成数字的算法。

如果没有减法符号，那就很简单了。只需将每个罗马数字转换成一个数字，然后将它们相加。

如果有减法符号，我们可能会不小心把左边的符号加错了。另外，左边的符号表示减法。因此，解决方法是在检测到左侧符号时将其减去两次。

以下是将罗马数字转换为数字的算法:

检查第 18 行到第 40 行的代码。看起来像不像 JavaScript reducer？

是的，确实如此。我们可以采用[数组 API](https://medium.com/better-programming/the-technical-interview-guide-to-dynamic-programming-3ce755d99849)，比如`from`、`map`、`reduce`来让代码真正凝练起来。

这些是验证测试，适用于两个 gists:

# 将数字转换成罗马数字

写了一个把罗马数字转换成数字的算法之后，我们要做相反的事情:把数字转换成罗马数字。

创建一个新的转换器，从最大的符号到最小的符号，包括 9 和 4。

*   男:1000
*   厘米:900
*   D: 500
*   CD: 400
*   列车员:100 元
*   XC: 90
*   L: 50
*   XL: 40
*   十点十分
*   九:9
*   五:5
*   四:4
*   一:1

如果一个数大于或等于当前符号，则取该符号并减去该数。重复该过程，直到剩余值为 0。

我们以 8 为例:

*   8 比 5 之前的任何符号都小。
*   8 大于 5。结果加上`V`，余数为 3。
*   3 小于 5，小于 4。
*   3 大于 1。结果加上`I`成为`VI`，余数为 2。
*   2 大于 1。结果加上`I`成为`VII`，余数为 1。
*   2 等于 1。结果加上`I`成为`VIII`，余数为 0。

算法是这样的:

看看上面的代码。你觉得有什么需要改进的吗？

对于第 31 - 38 行的循环，每次减去一个数字。对于 5000 这个数，需要 5 次减去 1000。当数量很大时，这可能会很慢。由于 5000 / 1000 是 5，所以可以预测需要 5 个`M`。当数不能被整除时，可以取一个楼层。同时，余数是剩余的数。

下面是改进的算法:

在第 36 行，我们采用字符串 API，`repeat`，这在[字符串文章](https://medium.com/better-programming/the-technical-interview-guide-to-string-manipulation-92f4c4649cd)中有所提及。

这些是验证测试，适用于两个 gists:

# 罗马数字计算器

我们把罗马数字转换成 a 数字，反之亦然。罗马数字对我们来说不再陌生。它们可以应用于各种数字问题。

让我们用罗马数字建造一个计算器。此计算器接受罗马数字、空格、括号、加号和减号。例如，键入`"I + II "`将产生数字`III`。

首先，我们需要构建一个使用正常数字的计算器，它可以计算`" 1 + 2 "`得到数字 3。

这些是验证测试:

要构建一个使用罗马数字的计算器，我们需要包含前面的函数来将罗马数字转换成数字(下面代码中的第 25 - 40 行)。然后用罗马数字相关块替换正常数字相关块(第 71 - 79 行)。

我们还需要包含前面的函数，将数字转换成罗马数字(第 42 - 57 行)，并在第 106 行返回值之前调用它。

总共有 4 行差异(第 72、75、76 和 106 行)。

在下面的代码中，我们为两种转换共享复杂的转换器。

这些是验证测试:

有趣的是，第 3 行的输出为 0，它被转换为罗马数字作为一个空字符串。数字 0 最初并没有自己的罗马数字，但中世纪的学者用单词 *nulla* (一个拉丁语单词，意思是“无”)来表示 0。

# 结论

脸书、微软和其他公司已经给出了关于罗马数字的面试问题。熟能生巧。享受编码吧！

感谢阅读。我希望这有所帮助。你可以在这里看到我的其他媒体出版物。