# 可维护的 JavaScript —注释

> 原文：<https://javascript.plainenglish.io/maintainable-javascript-comments-62b0c002dbf6?source=collection_archive---------14----------------------->

![](img/8426e6f2a207042d593f87b97766a9dc.png)

Photo by [George Pagan III](https://unsplash.com/@gpthree?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果想继续使用代码，创建可维护的 JavaScript 代码很重要。

在本文中，我们将通过创建更好的单行注释来了解创建可维护 JavaScript 代码的基础。

# 评论

很多人不喜欢评论。

添加和更新它们很麻烦。

它们感觉像是文件。

幸运的是，代码大部分时间都作为我们的文档。

我们可以写一些注释来涵盖代码中没有涵盖的内容。

JavaScript 支持两种注释。

注释可以是单行或多行。

# 单行注释

单行注释以 2 个斜杠开始，在行尾结束。

例如，我们可以写:

```
// a single-line comment
```

添加单行注释。

我们通常在两个斜线后面加一个空格来抵消注释文本。

单行注释有几种用法。

它们可以在自己的行中用来解释注释后面的内容。

该行前面应该总是有一个空行。

注释应该与下一行在同一缩进级别，

我们也可以在一行代码的末尾使用它们作为尾随注释。

因此在代码和注释之间应该至少有一个缩进。

并且注释不应该超过最大行长度。

这避免了任何水平滚动来阅读评论。

如果注释需要放到下一行，那么我们应该把注释放在代码上面的单独一行。

我们也可以通过注释来暂时停止一段代码的运行。

这可以通过许多文本编辑器自动完成。

单行注释不应该在连续的行中使用，除非我们临时用它来注释掉代码。

例如，我们可以写:

```
if (condition) { // all permission checks are successful
  allowed();
}
```

我们在注释上方有一个空行来提高可读性。

这比:

```
if (condition) {
  // all permission checks are successful
  allowed();
}
```

因为缺少空行所以更难阅读。

缩进也很重要。

它应该与我们正在评论的代码处于同一级别。

所以我们不应该写这样的评论:

```
if (condition) {
// all permission checks are successful
  allowed();
}
```

此外，如果一段代码和注释在同一行，我们应该在它们之间添加一些空格。

例如，我们写道:

```
var result = count + anotherCount; // both are always numbers
```

这样，我们在代码和注释之间有了一个空格。

我们不应该这样写:

```
var result = count + anotherCount;//both are always numbers
```

因为代码和注释之间没有空格。

另外，我们不应该写这样的评论:

```
// a condition that checks for
// all permissions.
// If all permissions are present, 
// then we do something
if (condition) {
  allowed();
}
```

我们有多个单行注释，应该是多行注释。

![](img/452420fd58915e0aadd4c2284cd759d1.png)

Photo by [Alex wong](https://unsplash.com/@killerfvith?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

有比其他方法更好的方法来编写 JavaScript 注释。

使用空格和正确的缩进可以更好地格式化单行注释。

如果单行注释太长，那么它应该是多行注释。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**