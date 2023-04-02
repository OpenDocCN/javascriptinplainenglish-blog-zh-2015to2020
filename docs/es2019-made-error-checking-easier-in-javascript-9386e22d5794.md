# ES2019 简化了 JavaScript 中的错误检查

> 原文：<https://javascript.plainenglish.io/es2019-made-error-checking-easier-in-javascript-9386e22d5794?source=collection_archive---------3----------------------->

## ECMAScript2019 规范意味着现在可以选择在 try…catch 语句的 catch 块中使用 error 参数。

![](img/98c10cd78ffcba4b8adfbce08c95c4f1.png)

Error handling is easier than ever in ES2019 (Photo by [Nathan Dumlao](https://unsplash.com/@nate_dumlao?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral))

在 JavaScript 语言最近发生变化之前，try…catch 语句总是需要在 catch 块中引用被捕获的错误。

在现代 JavaScript 中，引用被捕获的错误现在是可选的。

让我们看一个处理 JSON 对象的演示，看看它是如何工作的。

*不确定 JSON.parse()是如何工作的？* [*阅读我的文章解析& stringify*](https://medium.com/javascript-in-plain-english/how-to-use-stringify-and-parse-in-javascript-6b637b571a32) *。*

![](img/29f189ce7dbacec66a02ed0063129244.png)

Try…catch statements are commonly used with promises and asynchronous functions. (Photo by [Max Chen](https://unsplash.com/@maxchen2k?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral))

## 外卖

引用被捕获的错误不总是有用的吗？不，当然不是。有时候，您确实想显示捕获的错误本身。

但是，在有些情况下，该错误并不特别有用，例如来自 JSON.parse()的 SyntaxError。

在这些情况下，不按名称引用被捕获的错误会很有用。

德里克·奥斯汀博士是《职业规划:如何在 6 个月内成为一名成功的 6 位数程序员》一书的作者，该书现已在亚马逊上出售。