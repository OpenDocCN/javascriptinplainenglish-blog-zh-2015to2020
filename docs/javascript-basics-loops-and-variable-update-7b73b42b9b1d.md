# JavaScript 基础—循环和变量更新

> 原文：<https://javascript.plainenglish.io/javascript-basics-loops-and-variable-update-7b73b42b9b1d?source=collection_archive---------4----------------------->

![](img/38b6e7bf708fd1a599ab637ad10c44a0.png)

Photo by [Maarten Deckers](https://unsplash.com/@maartendeckers?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是世界上最流行的编程语言之一。为了有效地使用它，我们必须了解它的基本知识。

在这篇文章中，我们将看看如何使用循环和以简单的方式更新变量。

# While 和 Do 循环

如果我们需要重复运行同一段代码，那么我们必须让它为我们所用。

所以与其写:

```
console.log(0); 
console.log(1); 
console.log(2); 
console.log(3); 
console.log(4);
```

我们必须写一些更短的东西，这样我们就不必复制粘贴`console.log`那么多次。

我们可能要重复做一件事数百次、数千次或数百万次，所以我们不能一直这样做。

为了让我们的生活更轻松，我们可以使用循环来重复运行代码。

例如，我们可以写:

```
let num = 0;
while (num < 5) {
  console.log(num);
  num++;
}
```

在上面的代码中，我们将`num`设置为 0 作为初始条件。

然后我们运行循环体，直到`num`为 5。

`console.log`是在循环体中运行的另外一条语句，用来更新`num`。

我们确保`num`是递增的，这样我们就不会得到一个无限循环。

循环使我们在程序中重复做一些事情的工作变得更加容易。

do-while 循环类似，只是第一次迭代总是运行。

例如，我们可以将前面的例子改写为:

```
let num = 0;
do {
  console.log(num);
  num++;
} while (num < 5);
```

我们仍然有初始条件，但是现在循环体在条件检查之前运行。

现在我们检查`num`在结尾小于 5，而不是在开头。

# 刻痕

如果语句在一个块中，它们前面应该有空格。

JavaScript 并不要求这样做，但是这使得人们阅读程序变得更加容易。

如果块中的语句前没有空格，我们将很难阅读嵌套的代码。

2 个空格是一个很好的缩进规则。

口味可能不同。有些人用 4 空格，有些人用 tab。

但是 2 空间是一个受欢迎的选择。

大多数文本编辑器可以自动为我们添加缩进。

# 对于循环

For 循环是另一种循环。它让我们可以在一行中完成循环计数器的初始化、更新和值检查。

这样，很难忘记我们必须检查这些值。

例如，我们可以写:

```
for (let i = 0; i < 5; i++) {
  console.log(i);
}
```

我们有 for 循环将`i`索引变量设置为 0，确保仅在`i`小于 5 且`i < 5`和增量`i`都在第一行时运行。

在循环体内，我们像在 while 循环中一样运行`console.log`。

# 打破循环

我们不必运行一个循环的所有迭代。

为了打破循环，我们可以使用`break`语句。

例如，我们可以写:

```
for (let i = 1; i < 5; i++) {
  if (i % 3 === 0) {
    break;
  }
  console.log(i);
}
```

然后，当`i`是一个能被 3 整除的数时，我们停止循环。

这意味着我们不会看到任何超过 2 的日志。

![](img/372a1f6da815f220a7a741bf6f43c90f.png)

Photo by [mark james](https://unsplash.com/@rabbitman22?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 更新变量

我们可以通过使用`+=`、`*=`、`-=`或`/=`操作符以更短的方式更新我们的变量。

这样，当我们给变量赋值时，就不必重复变量名了。

例如，我们可以写:

```
i += 2
```

而不是:

```
i = i + 2
```

然后我们将`i`的值增加 2。

如果我们只增加或减少 1，那么我们可以分别使用`++`或`--`。

例如，我们可以写:

```
i++
```

而不是:

```
i = i + 1
```

或者:

```
i += 1;
```

注意`++`和`--`创建表达式，所以我们可以把它赋给一个变量，但是我们不应该这样做以减少混淆。

# 结论

我们可以使用循环来运行重复的代码，而不用在代码中重复它们。

此外，我们可以通过使用特殊操作符来更新变量，通过更新变量的现有值来更新变量。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**