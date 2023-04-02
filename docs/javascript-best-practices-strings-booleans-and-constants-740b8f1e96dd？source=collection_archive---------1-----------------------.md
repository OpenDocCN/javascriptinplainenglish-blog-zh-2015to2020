# JavaScript 最佳实践—字符串、布尔值和常量

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-strings-booleans-and-constants-740b8f1e96dd?source=collection_archive---------1----------------------->

![](img/159d540056a1c578e77520659a491297.png)

Photo by [Rajiv Perera](https://unsplash.com/@rajivperera?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。编写可以运行但存在问题的代码很容易。

在本文中，我们将了解如何在 JavaScript 中处理字符串、布尔值和常量。

# 避免魔法线

就像幻数一样，幻弦也一样不好。

如果我们在多个地方使用了同一个字符串，但没有明确说明其含义，那么我们应该将该值赋给一个已命名的常量。

例如，如果我们在任何地方都使用字符串`'employee'`，那么我们应该显式地将它赋给一个常量。

我们可以通过书写来分配它；

```
const WORKER_TYPE = 'employee';
```

所以我们知道它指的是公司里的员工类型。

此外，改变它会更容易，因为我们只需要改变它一次，而不是在许多地方。

我们也可能决定让我们的节目国际化。那么这个可以让我们很容易的翻译出来。

# 注意一个接一个的错误

我们应该注意字符串的索引，这样我们就不会试图访问长度超过字符串减 1 的索引。

# 检查我们的语言和环境如何支持 Unicode

如果我们使用 Unicode 字符，那么我们必须检查我们的语言或环境是否真正支持它。

JavaScript 程序在大多数情况下应该支持 Unicode，但是我们还是要检查一下。

# 在项目生命周期的早期决定国际化/本地化策略

国际化和本地化是主要问题。

如果我们必须翻译和本地化我们的程序，那么翻译和让所有支持的语言都正确运行将会是一项繁重的工作。

# 如果我们需要支持多种语言，使用 Unicode

Unicode 支持许多现成的语言。

# 使用布尔变量来记录我们的程序

我们可以将布尔表达式赋给一个变量，这样我们就知道表达式测试的是什么。

例如，不写:

```
navigator.userAgent.toLowerCase().indexOf('mac') !== -1
```

我们可以将它赋给一个变量，写为:

```
const isMac = navigator.userAgent.toLowerCase().indexOf('mac') !== -1;
```

然后我们知道我们正在我们的网站上检查 Mac 用户。

# 使用布尔变量简化复杂的测试

如果我们有复杂的测试，那么我们也可以将表达式赋给变量，这样我们就可以很容易地看到它们。

例如，如果我们有:

```
!error && MIN_COUNT < lineCount && lineCount < MAX_COUNT
```

我们可以定义一个变量来存储，如下所示:

```
const successfullyReadFile = !error && MIN_COUNT < lineCount && lineCount < MAX_COUNT
```

现在我们知道，这些表达式一起用于检查我们是否成功地读取了一个文件。

# 模拟枚举类型

枚举类型非常适合设置有几种可能选择的变量。

然而，JavaScript 没有枚举类型。

然而，我们可以用一个类的静态变量来模拟它，如下所示:

```
class Colors {}
Colors.RED = 'red';
Colors.GREEN = 'green';
Colors.BLUE = 'blue';
```

我们也可以把选择放在一个对象中:

```
const Colors = {
  RED: 'red',
  GREEN: 'green',
  BLUE: 'blue',
}
```

那么我们可以如下使用它们:

```
const color = Colors.RED;
```

# 在数据声明中使用命名常量

命名常量非常适合数据声明。

如果有些值我们在很多地方都不会改变和使用，那么我们应该把它们赋给一个指定的常量。

例如，我们可以写:

```
const MAX_DONUTS = 10;
```

设定一个人可以吃的甜甜圈的最大数量。

![](img/77b82d41c40fda1648a4e9c120fa8942.png)

Photo by [Drew Beamer](https://unsplash.com/@drew_beamer?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 避免文字

和上一节的建议差不多。如果我们有没有明显意义的文字，那么我们应该把它们赋给一个命名的常量。

例如，如果我们有:

```
for (let i = 0; i < 10; i++) {
  //...
}
```

我们有 10 个不知道是什么意思的词。

因此，我们写道:

```
const MAX_DONUTS = 10;
for (let i = 0; i < MAX_DONUTS; i++) {
  //...
}
```

所以我们知道 10 是甜甜圈的最大数量。

# 一致地使用命名常数

我们应该一致地使用名称常量，而不是在某些地方使用常量，在其他地方使用文字。

这样，如果需要的话，我们不会忘记更新某些地方的值。

# 结论

我们应该避免使用任何神奇的字符串，而是使用命名的常量。这样，如果常量值需要改变，我们只需要改变一个地方。

因为我们知道它们的意思，所以它们也使得阅读代码更容易。

如果我们有很长的布尔表达式，我们可能想把它们赋给一个变量，这样我们就知道它们在检查什么。

# **用简单英语写的便条**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页 [**plainenglish.io**](https://plainenglish.io/) 上找到所有这些——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**