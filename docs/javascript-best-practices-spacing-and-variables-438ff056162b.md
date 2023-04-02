# JavaScript 最佳实践—间距和变量

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-spacing-and-variables-438ff056162b?source=collection_archive---------4----------------------->

![](img/863e99aea040ee9f6c0f10b4da240329.png)

Photo by [Alexander Mils](https://unsplash.com/@alexandermils?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种简单易学的编程语言。编写运行并执行某些操作的程序很容易。然而，很难考虑所有的用例并编写健壮的 JavaScript 代码。

在本文中，我们将研究如何格式化文件源代码以提高可读性。

我们还会看看变量应该如何声明。

# 延续行的缩进

如果我们的行从上一行继续，那么我们至少缩进 4 个空格，这样我们就可以区分这一行和上一行。

# 间隔

当我们写代码时，我们应该考虑间距。

有些空格是必需的，有些不是。即使它们不是必需的，我们也可能想要拥有它们。

# 垂直空白

在一个类或对象文本中，连续的方法之间应该有一个空行。

方法体应该在逻辑组之间换行。

然而，我们不应该在函数的开头或结尾有空行。

在类或对象的第一个和最后一个方法之前，我们可能会有一个空行。

# 水平空白

水平空格的使用取决于位置。

我们不应该有尾随空白。

我们应该用空格来分隔保留字，如`if`、`for`或`catch`。

`function`和`super`在它们和左括号之间不需要空格。

`else`或`catch`用于将保留字与其前面的右花括号分开。

我们应该在左花括号前加一个空格。

此外，我们不应该在模板字符串表达式的开头有任何空格。

所以我们不应该:

```
`a $ {y} b`
```

相反，我们应该写:

```
`a ${y} b`
```

任何二元或三元表达式的两边在操作数之间也应该有一个空格。

还应该添加逗号和分号。

对象文本中的冒号后面也应该有一个空格。

开始行尾注释的双斜线两边也应该有空格。

开放块注释字符，这些字符的两边也有空格。

# 没有水平对齐

我们的代码中不应该有水平对齐。

它们很难维护。

例如，我们可以这样写:

```
{
  foo: 1, 
  longer: 2,
};
```

而不是:

```
{
  foo:    1, 
  longer: 2,
};
```

# 函数参数

我们应该将函数参数换行，以便每行的宽度不超过 100 个字符。

例如，我们可以写:

```
doSomething(
  fooArgumentOne,
  fooArgumentTwo,
  fooArgumentThree
)
```

# 分组括号

我们可能会用括号将表达式分组，如果没有括号，这些表达式可能会被误解。

然而，它们对于跟随在`delete`、`typeof`、`void`、`return`、`throw`、`case`、`in`、`of`或`yield`之后的表达式来说是不必要的。

# 评论

我们可能希望在一些代码中加入注释。

# 阻止评论

对于多行注释，我们可以使用`/* ... */`将注释添加为块。

单行注释，我们从`//`开始。

# 参数名称注释

如果参数没有足够清楚地表达它们的意思，我们应该对参数的命名进行评论。

例如，我们可以写:

```
foo(obviousParam, /* greeting= */ 'hello');
```

# 语言特征

JavaScript 有许多可疑或危险的特性，我们应该避免。

我们应该尽可能避免不好的。

# 变量声明

有比其他方法更好的方法来声明变量。

我们应该尽可能多地使用它们。

![](img/fc3e53dbad2aa3a1b73ce171efc73139.png)

Photo by [Brooke Lark](https://unsplash.com/@brookelark?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 使用 let 或 const

我们应该使用`let`或`const`来声明变量。

这样，它们是块范围的，在块外不可用。

除非需要重新分配变量，否则应使用`const`。

`var`不得使用关键字。

# 每个声明一个变量

我们不应该在每个声明中有一个以上的变量。

所以，类似`let a = 3, b = 2;`的东西千万不要用。

# 需要时声明变量

我们应该在需要变量之前声明它们。

这样，我们就不必在太多的行中遵循一个变量的用法。

# 尽快初始化

我们应该尽快初始化变量。

这样，我们就不会忘记它们，也不会出现变量未初始化或`undefined`错误。

# 结论

我们应该用括号将 or 表达式分组，以明确它们是如何被调用的。

应在块上添加缩进。

垂直和水平的空白都很重要。

变量应该只在需要的时候声明。并且应该用`let`或`const`声明。

## 用简单的英语写的便条

你知道我们有四种出版物吗？通过[**plain English . io**](https://plainenglish.io/)找到它们——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**