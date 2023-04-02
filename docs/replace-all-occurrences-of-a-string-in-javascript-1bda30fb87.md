# 替换 JavaScript 中出现的所有字符串

> 原文：<https://javascript.plainenglish.io/replace-all-occurrences-of-a-string-in-javascript-1bda30fb87?source=collection_archive---------6----------------------->

![](img/bbbab6fc99628470add15052bb51d73c.png)

在 JavaScript 中处理字符串时，我们可能会遇到用另一个字符串替换所有出现的字符串的任务。

让我们创建一个非常简单的测试用例来说明这个问题。首先，让我们定义一个`testString`，我们的目标是用`bar`替换*所有的* `foo`字符串:

```
const testString = 'abc foo def foo xyz foo';
```

简单而天真的方法是尝试类似于`replace()`函数的东西:

```
const result = testString.replace('foo', 'bar')
```

一旦我们运行它，我们只替换第一次出现的内容，结果将是:

```
abc bar def foo xyz foo
```

绝对不是我们想要的！

如果你有 Java(而不是 JavaScript！)体验你可以快速尝试类似于:`replaceAll()`的东西。不幸的是，JavaScript 中没有这样的字符串函数。

然而，有两种简单的方法来替换 JS 中出现的所有字符串。让我们使用`split()`和`join()`功能尝试第一种解决方案:

```
const result = testString.split('foo').join('bar'); 
```

瞧。它确实起作用了，结果正如我们所料:

```
abc bar def bar xyz bar
```

我们所做的是，首先通过使用`foo`作为分隔符将原始的`testString`转换成字符串的*数组*，然后我们用`bar`字符串将它连接在一起。

第二种解决方案是基于… `replace()`函数。这一次，我们将使用正则表达式，而不是搜索模式。这就像添加两个斜线`/`一样简单，重要的是，`g`修饰符(其中`g`代表`global`匹配，即替换将从给定字符串的开始到结束，而不是匹配第一个出现的字符串):

```
const result = testString.replace(/foo/g, 'bar');
```

答对了。这也很有效。

事实上， [replace 实际上比 split-join 方法更快](https://jsperf.com/replace-all-vs-split-join)。因此，只需使用它，或者如果您在代码的其他地方看到拆分-连接方法，请用更快的代码替换它。