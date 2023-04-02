# 如何在 JavaScript 中使用 JSON.stringify()和 JSON.parse()

> 原文：<https://javascript.plainenglish.io/how-to-use-stringify-and-parse-in-javascript-6b637b571a32?source=collection_archive---------0----------------------->

## `JSON.stringify()`和`JSON.parse()`是处理 JSON 格式内容的有用工具

![](img/7b054548743f2b417cb4df137b511d1d.png)

Stringify turns objects like { foo: ‘bar’ } into JSON strings like {‘foo’: ‘bar’} (Photo by [davide ragusa](https://unsplash.com/@davideragusa?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral))

JavaScript 有两个有用的方法处理 [JSON](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON) 格式的内容:`[JSON.stringify()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)` & `[JSON.parse()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse)`，两个一起学很有用。

*   `JSON.stringify()`获取一个 JavaScript 对象，然后将其转换成 JSON 字符串。
*   `JSON.parse()`获取一个 JSON 字符串，然后将其转换成一个 JavaScript 对象。

让我们来看一个代码示例:

`JSON.stringify()`可以接受附加参数、一个 replacer 函数和一个字符串或数字，用作返回字符串中的“空格”。

这意味着如果返回 undefined，replacer 参数可用于筛选出值，如下例所示:

# JSON.parse 和 JSON.stringify 可以用来深度复制对象吗？

在 JavaScript 深度克隆的讨论中，一个常见的代码示例是将 JSON.parse 包装在 JSON.stringify 周围，以制作数组或对象的深度副本——这意味着深度嵌套的数组或对象将被复制。

但是，要小心使用这种方法，因为它不适用于许多数据类型，包括 undefined、 [Date 对象、RegExp 对象](https://medium.com/coding-in-simple-english/how-to-parse-a-date-using-a-regular-expression-in-javascript-f4e5b1d02935)和 [Infinity](https://medium.com/swlh/what-is-infinity-in-javascript-%EF%B8%8F-1faf82f100bc) :

如果你确定你的深度嵌套数据只包括[字符串](https://medium.com/javascript-in-plain-english/how-to-check-for-a-string-in-javascript-a16b196915ff)、[数字](https://medium.com/javascript-in-plain-english/how-to-check-for-a-number-in-javascript-8d9024708153)、布尔和空，那么你可以使用 parse & stringify 进行深度复制。

但是在 JavaScript 中深度复制对象和数组的一种更可靠的方法是使用自定义函数或助手库，比如`[rfdc](https://github.com/davidmarkclements/rfdc)`，正如我在这里讨论的:

[](https://medium.com/javascript-in-plain-english/how-to-deep-copy-objects-and-arrays-in-javascript-7c911359b089) [## 如何在 JavaScript 中深度复制对象和数组

### 复制对象或数组的常用方法只能进行浅层复制，所以深度嵌套的引用是个问题…

medium.com](https://medium.com/javascript-in-plain-english/how-to-deep-copy-objects-and-arrays-in-javascript-7c911359b089) ![](img/aef58175efccea1c0889265d179c94b5.png)

How do you spell stringify backwards? JSON.parse() (Photo by [clement fusil](https://unsplash.com/@clementfusil?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral))

[德里克·奥斯丁](https://www.linkedin.com/in/derek-austin/)博士是《职业编程:如何在 6 个月内成为 6 位数的成功程序员 一书的作者，该书现已在亚马逊上出售。