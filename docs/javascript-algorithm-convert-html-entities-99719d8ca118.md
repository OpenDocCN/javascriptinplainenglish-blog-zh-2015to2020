# JavaScript 算法:转换 HTML 实体

> 原文：<https://javascript.plainenglish.io/javascript-algorithm-convert-html-entities-99719d8ca118?source=collection_archive---------3----------------------->

## 编写一个函数，将选定数量的字符转换成相应的 HTML 实体。

![](img/91772eb16ec6eb4b223ca9a2d04ec570.png)

今天我们将编写一个名为`convertHTML`的函数，它将接受一个字符串(`str`)作为参数。

您将得到一个包含以下字符之一的字符串:`&`、`<`、`>`、`"`(双引号)和`'`(撇号)。该函数的目标是返回包含这些字符的字符串，但是是在它们对应的 HTML 实体中。

你可以在网上找到与这些字符相对应的 HTML 实体的列表[,但是这里有一个我们将用于该功能的字符的快速列表:](https://dev.w3.org/html5/html-author/charref)

`&` → `&amp;`

`<` → `&lt;`

`>` → `&gt;`

`“` → `&quot;`

`‘` → `&apos;`

示例:

```
let str = "Dolce & Gabbana";// output: "Dolce &amp; Gabbana"let str = "Hamburgers < Pizza < Tacos"// output: "Hamburgers &lt; Pizza &lt; Tacos"
```

对于这个函数，我们将结合使用正则表达式和`replace()`方法。

首先，我们将创建一个正则表达式模式，它将匹配任何字符`&`、`<`、`>`、`"`和`'`。我们将该模式分配给一个名为`regex`的变量。

```
let regex = /[&|<|>|"|']/g;
```

接下来，我们将在字符串输入上使用`replace()`方法。我们将编写一个函数，而不是在参数中编写一个替换字符串。

该函数将获取与正则表达式模式匹配的每个字符，并根据该字符指定要返回的 HTML 实体。

```
let htmlString = str.replace(regex, function(match) {
    if (match === "&") {
        return "&amp;";
    } else if (match === "<") {
        return "&lt;"
    } else if (match === ">") {
        return "&gt;";
    } else if (match === '"') {
        return "&quot;";
    } else {
        return "&apos;";
    }
});
```

在我们的函数中，我们创建了一系列 if 语句来检查传递到函数中的每个字符，并返回其对应的 HTML 实体。

我们将新的字符串赋给`htmlString`并返回它。

```
return htmlString;
```

我们的代码到此结束。下面是该函数的其余部分:

如果您发现这个算法很有帮助，请查看我的其他 JavaScript 算法解决方案文章:

[](https://medium.com/@endubueze00/javascript-algorithm-hex-to-decimal-3400f3742d1e) [## JavaScript 算法:十六进制到十进制

### 写一个将十六进制数转换成十进制数的函数。

medium.com](https://medium.com/@endubueze00/javascript-algorithm-hex-to-decimal-3400f3742d1e) [](https://medium.com/swlh/javascript-algorithm-sorted-union-9e316654561f) [## JavaScript 算法:排序联合

### 我们编写一个函数，它将返回从两个或更多其他数组中获取的唯一值的数组。

medium.com](https://medium.com/swlh/javascript-algorithm-sorted-union-9e316654561f) [](https://levelup.gitconnected.com/javascript-algorithm-search-and-replace-6895e17ccfd7) [## JavaScript 算法:搜索和替换

### 我们编写了一个函数，可以对字符串执行简单的搜索和替换操作。

levelup.gitconnected.com](https://levelup.gitconnected.com/javascript-algorithm-search-and-replace-6895e17ccfd7) 

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****