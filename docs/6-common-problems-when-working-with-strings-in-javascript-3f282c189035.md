# 在 JavaScript 中使用字符串时的 6 个常见问题

> 原文：<https://javascript.plainenglish.io/6-common-problems-when-working-with-strings-in-javascript-3f282c189035?source=collection_archive---------3----------------------->

## 以及如何解决它们

![](img/06b54c6c3dc2afc5032ae4d6a1a90633.png)

Photo by [Shahadat Rahman](https://unsplash.com/@hishahadat?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在任何编程语言中，处理字符串都是最常见的任务之一。

在 JavaScript 中，有一些内置函数，我想你会用到很多次。

那么它们是什么呢？让我们找出答案。

# 1.搜索和替换

假设你需要构建一个编辑器，它有一个特性就是找到 ***字符串 X*** 并用 ***字符串 Y*** 替换它，那么 ***replace()*** 就是你需要使用的函数来完成这个特性。

## 替换()

如何使用:***string . replace(strToFind，strToReplace)***

***strToFind*** 是您要查找的字符串， ***strToReplace*** 将是替换 ***strToFind*** 的字符串。

点击此处查看演示。

# 2.按索引获取子串

以下三种方法可以帮助您从给定的字符串中获取子字符串。

## 切片()

如何使用: ***string.slice(start，end)***

它将返回给定字符串从 ***开始*** 位置到 ***结束*** 位置的子串。

[点击这里观看演示。](https://codepen.io/amyjandrews/pen/QWjKJEe)

## 子字符串()

如何使用: ***string.substring(开始，结束)***

除了 ***开始*** 和 ***结束*** 必须始终为正之外，其工作原理与 ***slice()*** 相同。

点击此处查看演示。

## substr()

如何使用: ***string.substr(start，length)***

***substr()*** 函数要求您提供两个参数，起始位置和长度字符数。

点击此处查看演示。

# 3.将字符串转换为数组

给你一句话，*“我是 Amy，我是程序员。你想成为一名程序员吗？”*。要求是统计那句话的字数。要解决这个问题，你可能需要 ***split()*** 。

## 拆分()

如何使用: ***string.split(分隔符，限制)***

***分隔符*** 是用于拆分字符串的字符或正则表达式。

***极限*** 是拆分的次数(我一般不使用)

[点击此处查看演示。](https://codepen.io/amyjandrews/pen/xxwEQBW)

# 4.转换案例

可以使用***to lower case()***将字符串转换成小写，使用***toupper case()***将字符串转换成大写。

## toLowerCase()

如何使用:***string . tolowercase()***

点击此处查看演示。

## toUpperCase()

如何使用:***string . toupper case()***

点击此处查看演示。

# 5.连接字符串

这也是使用字符串时的常见任务之一。在 JavaScript 中，可以简单地使用' ***+'*** 运算符或者使用 ***concat()*** 函数。

## concat()

如何使用: ***string.concat(str1，str2，…，strN)***

str1，str2，…，strN 是要连接的字符串。

[点击这里观看演示。](https://codepen.io/amyjandrews/pen/qBOaQYx)

# 6.查找子字符串位置

我在工作中很满足这个要求。具体来说，从一段文本中删除一些不必要的字符，或者将一个句子的部分文本装饰为突出显示或使其红色。

您可以使用以下三个函数来查找子字符串:

## 索引 Of()

如何使用:***string . index of(subStr)***

您提供 ***subStr*** 作为想要查找的子串，该函数返回第一个出现的 ***subStr*** 。如果没有找到 ***subStr*** ，则返回 ***-1*** 。

[查看这里的演示](https://codepen.io/amyjandrews/pen/ExVgdzy)。

## lastIndexOf()

如何使用:***string . lastindexof(subStr)***

如果 ***indexOf()*** 返回第一个出现的 ***subStr*** ，那么***lastIndexOf()***将返回最后一个出现的 ***subStr*** 。

点击此处观看演示。

## 搜索()

如何使用:***string . search(subStr)***

这个和 ***indexOf()*** 一样。

[点击此处查看演示。](https://codepen.io/amyjandrews/pen/jObMQNj)

当然，JavaScript 中还有许多其他的字符串处理函数，但以上是最常见的。

使用字符串时，您会遇到哪些常见问题？请在下面的评论中告诉我。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！为我们的新出版物献上一点爱心吧，请跟随他们:[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至:[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****