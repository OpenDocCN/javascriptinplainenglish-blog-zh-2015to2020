# JavaScript 最佳实践—变量声明

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-variable-declarations-72119e0228eb?source=collection_archive---------5----------------------->

![](img/4305a5a5244fa4443341d2ad107e04f8.png)

Photo by [Sid Balachandran](https://unsplash.com/@itookthose?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。编写可以运行但有错误的代码很容易。

在本文中，我们将看看如何正确地声明变量。

# 要求在变量声明中初始化

变量声明应该在声明的时候赋值，这样我们就不用担心以后没有声明的变量了。

例如，我们应该编写以下代码:

```
var x = 1;
```

确保`x`从一开始就有价值。

同样，用`const`声明的常量必须用值声明，所以我们必须写:

```
const x = 1;
```

然而，这更多的是一种风格偏好，因为它对我们的代码没有显著的影响。

# 永远不要对变量使用删除运算符

永远不要在变量上使用`delete`操作符。这可能会导致意想不到的行为。

例如，如果我们有以下没有严格模式的代码:

```
let x = 1;
delete x;
```

在对它使用了`delete`操作符后，我们得到`x`仍然是 1。它也禁止我们使用严格模式，所以下面的代码会抛出一个错误:

```
'use strict'
let x = 1;
delete x;
```

上面的代码会给出错误‘未捕获的语法错误:在严格模式下删除一个不合格的标识符。’

这是使用严格模式的一个很好的理由，以防止我们在变量上使用`delete`这样的事情。

# 不要用与变量名相同的名称命名标签

我们不应该用与变量名相同的名字来标记循环。

例如，我们不应该像下面的代码那样有标签:

```
let x = 1;const baz = () => {
  x: for (let y = 0; y <= 1; y++) {
    break x;
  }
}
```

与变量同名的标签很容易让人混淆。

# 没有未声明的变量

未声明的变量是尚未定义的变量。

如果没有严格模式，这些变量将作为全局变量被动态创建。这是不可取的，因为它污染了全球范围。我们不希望这样，因为多个同名的全局变量可能会相互冲突。

因此，我们不应该有如下代码:

```
a = 1;
```

在上面的代码中，如果严格模式没有打开，那么`a`被声明为一个全局变量。

我们不希望这样，所以我们应该打开严格模式，这样可以防止创建全局变量`a`，反而会抛出一个错误。

要声明变量，我们要么使用`let`、`const`，要么使用`var`。`let`和`const`更受青睐，因为它们是块范围的，在声明之前不能被引用。`const`也不能被重新赋值，必须在用`const`声明某个东西时设置一个值。

同样，我们应该使用带有`typeof`的未声明变量。例如，如果`a`未声明，我们应该编写如下表达式:

```
typeof a === 'undefined';
```

在没有严格模式的情况下，如果事先没有声明`a`，上面的表达式将返回`true`。

打开严格模式，如下所示:

```
'use strict';
typeof a === 'undefined';
```

它还会返回`true`，所以我们应该检查我们没有将这些未声明的变量与`typeof`操作符一起使用。

# 否初始化为未定义

在 JavaScript 中，已声明但未初始化的变量被自动设置为`undefined`。因此，我们不必显式地将其设置为`undefined`。

例如，我们不必编写类似下面的代码来将`bar`设置为`undefined`:

```
let bar = undefined;
```

相反，我们可以只写:

```
let bar;
```

![](img/4b573cdf882d319d2e4098cdbc7c5201.png)

Photo by [NOAA](https://unsplash.com/@noaa?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 不要使用 u `ndefined`作为变量名

我们不应该使用`undefined`作为变量名，因为它是保留关键字。

例如，我们不应该写类似下面这样的东西:

```
var undefined = "hi";
```

如果没有严格模式，这将什么也做不了。如果我们在这一行之后记录`undefined`，我们仍然会得到`undefined`。

但是，这是没有用的，也是混乱的。因此，在严格模式下是禁止的。

我们也不能将`var`替换为`let`或`const`，如下所示:

```
let undefined = "hi";
```

或者:

```
const undefined = "hi";
```

因为我们将得到错误“未捕获的语法错误:标识符“未定义”已经被声明”。

这是使用`let`和`const`的另一个好理由。

# 结论

我们在声明 JavaScript 变量时应该小心。我们不应该用像`undefined`这样的保留关键字来声明变量。

此外，我们不应该在代码中有未声明的变量。JavaScript 严格模式应该禁止这种情况发生。

运算符不应该用在变量上，因为它不做任何事情。

标签不应该和变量名重名，以免混淆。

# **用简单英语写的便条**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页 [**plainenglish.io**](https://plainenglish.io/) 上找到所有这些——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**