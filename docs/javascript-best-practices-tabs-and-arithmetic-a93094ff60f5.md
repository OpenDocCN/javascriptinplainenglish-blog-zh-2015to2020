# JavaScript 最佳实践—制表符和算术

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-tabs-and-arithmetic-a93094ff60f5?source=collection_archive---------11----------------------->

![](img/9501817eac581767a446e66bccb3b343.png)

Photo by [Zdeněk Macháček](https://unsplash.com/@zmachacek?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。编写可以运行但有错误的代码很容易。

在本文中，我们将研究在 JavaScript 中使用`++`和`--`操作符以及用空格替换制表符。

# 一元运算符`++`和`--`

`++`和`--`操作符让我们将数值变量加 1。

例如，如果我们有以下代码:

```
let x = 1;
x++;
```

然后`x`先赋 1，然后因为`x++`加 1 就变成 2 了。

`++`运算符也可以用在变量之前。例如，我们可以编写以下代码:

```
let x = 1;
++x;
```

上面的代码也首先将变量赋值为 1。然后它将变量`x`增加到 2。

在变量之前使用它和在变量之后使用它的区别在于，当它被添加到变量之后时，它递增并返回递增前的值。

如果在变量之前使用它，那么它递增并返回递增后的值。

在我们的例子中，前后没有区别，因为我们没有把它赋给任何变量。

然而，如果我们把它赋给一个变量或常数，那就有关系了。例如，如果我们有以下代码:

```
let x = 1;
const y = ++x;
```

那么`x`和`y`都是 2，因为`x`的最新值在递增后返回，然后最新值被赋给`y`。

另一方面，如果我们有下面的代码:

```
let x = 1;
const y = x++;
```

那么`x`为 2，`y`为 1，因为`x`的旧值是在执行递增操作时返回的，所以旧值 1 被赋给`y`。

`--`类似于`++`，除了它将变量递减 1 而不是递增 1。

我们可以看到，如果在变量之前或之后应用`--`，结果与`++`相似。

因此，`++`或`--`在变量之前或之后出现很重要。

空格也很重要。无论运算符是加在变量之前还是之后，我们都不应该有任何空格。

例如，我们不应该编写如下代码:

```
let x = 1;
let y = 1;
x 
++ 
y;
```

在上面的代码中，`x`保持为 1，但`y`为 2，因为 JavaScript 解释器将其解释为如下:

```
let x = 1;
let y = 1;
x
++y;
```

因此，`y`递增，而`x`不递增。

为了使递增和递减操作更加清晰，我们可以改为编写以下代码:

```
let x = 1;
x += 1;
```

然后`x`增加 1，所以`x`变成 2。

这也适用于将`x`增加任意数量。所以我们可以这样写:

```
let x = 1;
x += 2;
```

将`x`增加 2。

减法、乘法或除法也有相应的运算符。

例如，我们可以写出下面的减法:

```
let x = 1;
x -= 2;
```

对于乘法，我们可以写成:

```
let x = 1;
x *= 2;
```

对于除法，我们可以这样写:

```
let x = 1;
x /= 2;
```

这些比`++`或`--`更清晰，所以除了给同一个变量赋值外，我们还应该考虑使用这些运算符进行算术运算。

![](img/f616a35f3bb377ad3d5a964918367368.png)

Photo by [Tomas Sobek](https://unsplash.com/@tomas_nz?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 没有制表符

制表符是一种痛苦。它们在所有操作系统或文本编辑器中并不一致。

因此，在代码中使用制表符时，我们应该三思。使用制表符可以节省输入，但是会产生操作系统和文本编辑器之间间距不一致的问题。

继续使用 tab 键来节省打字的一个好方法是将它们转换成两个空格。

空格几乎总是与大多数文本编辑器和操作系统保持一致。因此，当我们在不同的文本编辑器或操作系统工具中打开同一个文件时，不会有任何间距问题。

# 结论

`++`和`--`运算符可能会带来混淆。它们可以被添加到变量的前面或后面，当更新的值被返回时，它们的行为会有所不同。

如果在变量之前应用这些操作符，那么最新的值会立即返回，这样我们就可以将更新后的值赋给另一个变量。

另一方面，返回值仍然是旧值，所以如果我们试图给一个变量赋值，它将被赋旧值。

它们都递增，但是它们的返回值是不同的。

在我们的代码中不应该使用制表符。如果我们想节省打字，我们应该自动将制表符转换成空格。

# **简明英语笔记**

你知道我们推出了一个 YouTube 频道吗？我们制作的每个视频都旨在教给你一些新的东西。点击此处 查看我们，并确保订阅该频道😎