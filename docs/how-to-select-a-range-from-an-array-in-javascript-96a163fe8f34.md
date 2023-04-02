# 如何在 JavaScript 中从数组中选择一个范围

> 原文：<https://javascript.plainenglish.io/how-to-select-a-range-from-an-array-in-javascript-96a163fe8f34?source=collection_archive---------2----------------------->

## 需要获取数组中的一系列项目？下面是如何使用`Array.prototype.slice()`，就像`String.prototype.substring()`对于数组一样。

![](img/763dc67c2697f1db6654c950edb45ec5.png)

Photo by [Kaizen Nguyễn](https://unsplash.com/@kaizennguyen?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果你读过我关于 [JavaScript 子字符串](https://medium.com/coding-at-dawn/how-to-select-a-range-from-a-string-a-substring-in-javascript-1ba611e7fc1)的文章，那么你就会知道我实际上更喜欢 Ruby 的语法而不是 JS，因为 Ruby 的`[..](http://rubylearning.com/satishtalim/ruby_ranges.html)` [范围操作符](http://rubylearning.com/satishtalim/ruby_ranges.html):

[](https://medium.com/coding-at-dawn/how-to-select-a-range-from-a-string-a-substring-in-javascript-1ba611e7fc1) [## 如何在 JavaScript 中从字符串(子串)中选择范围

### 这里没有捷径可走。要获得子串，使用内置方法 string . prototype . substring(startIndex…

medium.com](https://medium.com/coding-at-dawn/how-to-select-a-range-from-a-string-a-substring-in-javascript-1ba611e7fc1) 

因此，我希望能够使用 Ruby 的 range 语法在数组中选择一个范围，这一点也不奇怪——这太方便了！

在 Ruby 中，`["🏄","🏊","🌴","🍹","🌞"][2..3]`等于`["🌴","🍹"]`。

在 JavaScript 中，我们需要使用`Array.prototype.slice()`。以下是方法。

> "`**slice()**`方法将数组的一部分的浅拷贝返回到从`begin`到`end`(不包括`end`)中选择的新数组对象中，其中`begin`和`end`表示该数组中的项目的索引。原始数组不会被修改。— [MDN 文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)

在 JavaScript 中，`["🏄","🏊","🌴","🍹","🌞"].slice(2,4)`将产生我们的比奇表情符号对，`["🌴","🍹"]`。

注意，结束索引的项目没有包括在内，因此与 Ruby range 方法的工作方式相比，您需要在索引中添加一个项目。

如果不包含结束索引，则得到数组的剩余部分，所以`["🏄","🏊","🌴","🍹","🌞"].slice(2)`产生`["🌴","🍹","🌞"]`。

也可以使用负索引，从数组末尾开始倒数:`["🏄","🏊","🌴","🍹","🌞"].slice(-2)`返回`["🍹","🌞"]`。

提供给`slice()`的指标可以是正的，[零](https://medium.com/coding-at-dawn/is-negative-zero-0-a-number-in-javascript-c62739f80114)，也可以是负的:
`["🏄","🏊","🌴","🍹","🌞"].slice(-4,3)`使`["🏊","🌴"]`。

同样，`["🏄","🏊","🌴","🍹","🌞"].slice(2,-1)`就是`["🌴","🍹"]`。

有一个有趣的事实——`slice()`实际上可以以同样的方式用在弦上，在这种情况下，它的工作方式与`substring()`完全一样。

(一个区别是`slice()`取负指数；`substring()`不会。)

`slice()`的另一个常见用法是[浅复制数组](https://levelup.gitconnected.com/how-to-copy-an-array-in-javascript-with-array-from-298c7e66eebc)，在下面的文章中，我将它与[深复制数组](https://medium.com/javascript-in-plain-english/how-to-deep-copy-objects-and-arrays-in-javascript-7c911359b089)的方法进行了比较:

[](https://medium.com/javascript-in-plain-english/how-to-deep-copy-objects-and-arrays-in-javascript-7c911359b089) [## 如何在 JavaScript 中深度复制对象和数组

### 复制对象或数组的常用方法只能进行浅层复制，所以深度嵌套的引用是个问题…

medium.com](https://medium.com/javascript-in-plain-english/how-to-deep-copy-objects-and-arrays-in-javascript-7c911359b089) 

正如您可能猜到的，鉴于我与`.substring()`方法的比较，`slice()`不会改变原始数组。

(无论`substring()`还是`slice()`都不会修改原始字符串。)

内置方法`slice()`对于返回现有数组的一部分非常有用——原始数组中的子数组或子序列。

请记住，结束索引处的数组项不包括在数组的切片部分中，并且负索引是倒计数的。

编码快乐！💯😊🔥😍🖥️

德里克·奥斯汀博士是《职业规划:如何在 6 个月内成为成功的 6 位数程序员》一书的作者，该书现已在亚马逊上出售。