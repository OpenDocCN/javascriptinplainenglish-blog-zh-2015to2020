# 对象变换的箭头函数和析构

> 原文：<https://javascript.plainenglish.io/javascript-destructuring-for-object-transformation-8ac8d088134e?source=collection_archive---------4----------------------->

![](img/e6e88cb85dd893599e78c847927b92db.png)

Image by [Free-Photos](https://pixabay.com/photos/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=498202) from [Pixabay](https://pixabay.com/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=498202)

箭头函数和析构是 JavaScript 的两个强大特性。当结合使用时，它们呈现出一种很酷的模式，我喜欢称之为对象转换。**要点是编写一个简单的函数来改变给定对象**的结构，同时保留(一些)属性值。

# 简单变压器

首先，让我们编写一个看起来非常对称的普通 transformer 函数，就像这样:

乍一看，这个函数似乎什么都不做。让我们针对一个对象运行它:

它返回与接收到的对象相同的对象。现在让我们用一个更复杂的对象来填充它:

现在我们看到这个 transformer 函数返回了一个新的对象，其中只有`fruit`属性。

那么它是如何工作的呢？这个`extract`是一个 lambda 函数，它接受一个具有`fruit`属性的对象(表达式的左边部分)。它返回具有相同`fruit`属性(表达式的右边部分)和相同值的对象，忽略所有其他属性。`=>`符号将参数与 lambda 函数体分开。

箭头右边的对象应该放在括号中，否则 JavaScript 解释器会将花括号解析为函数体，而不是对象:

`wrongExtract`函数的主体只包含一个表达式——`fruit`变量。这是一个有效的 JavaScript 表达式，但是它不从函数返回任何值，所以函数返回`undefined`。

现在让我们使用析构来提取一些深度嵌套的数据。我们想从一个盒子里得到一个水果，这个盒子本身就在一个容器里:

我们刚刚编写了一个函数，使用析构从对象中提取一个深度嵌套的结构。

# 复杂的案件

但是析构不仅对提取数据有用。让我们用它来交换一个对象的两个嵌套很深的属性的值。这里的对象有点复杂，所以我们不会尝试将函数写成一行——跨多行格式化应该可以提高可读性:

不过，这里还有很多东西需要消化。我们有一个复杂的对象，在不同的嵌套层次上有两个(布尔)属性。我们希望交换这些属性的值，所以我们将它们析构为`hanShotFirst`和`greedoShotFirst`变量。

使用这些变量，我们返回一个具有相同结构的新对象，但是交换了这些属性值。

最后，让我们使用数组析构从数组中提取一个元素:

我认为你可以使用箭头函数和析构赋值来进行对象转换的方式非常酷。当然，析构化还有更多的用例，但这是我目前最喜欢的用例。