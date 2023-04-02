# JavaScript 最佳实践

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-with-my-experiences-6d16e371f669?source=collection_archive---------1----------------------->

## 深度 JavaScript

## JavaScript 编程的注意事项(根据我的经验)

![](img/1f7013560844ca30fb32d222feff2676.png)

Photo by [NESA by Makers](https://unsplash.com/@nesabymakers?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

这些是我认为可以提高代码质量和清晰度的 JavaScript 最佳实践。

# 句法

## **使用 2 个空格**进行缩进

最近几年，缩进已经从你的空格变成了两个空格。例如，两个空格，不长，没有制表符——由 Google、npm、Node.js、Idiomatic、Felix 使用。

请记住，不要使用制表符缩进，因为应用程序和操作系统之间显示的制表符是如此不同。

## 除了避免转义，我对字符串使用单引号

您已经看到了用于在 JavaScript 中编写字符串的`'single quotes'`和`"double quotes"`。

```
'abc' === "abc"
```

但是我更喜欢用单引号来避免转义。

## 我在关键字之后和左括号之前添加了一个空格

在书面英语中，左括号后和右括号前没有空格。我也将这种方法应用于 JavaScript。

关键字`function`后面有一个空格。

对于匿名函数，我也加一个空格如下。

## 我在逗号后面加空格

逗号没有意义，但是它们帮助我们理解句子的结构，从而理解句子的意思。在逗号后面加一个空格。

## 我将 else 语句和它们的花括号放在同一行

## 我把条件运算符放在括号里

这有助于阅读。括号对于执行是无用的。但是对执行没用并不意味着对阅读代码没用。如果读起来更有意义，你应该把它们留下。

# 变量

## 我每行声明一个变量

不要用一个声明来声明多个变量。

我在每行声明一个变量，因为删除、插入和重新排列行很简单，而且行会自动正确缩进。

## 不要在两个不同的块中两次声明同一个变量

我不这样做。

应该这样写。

## 我用一个`/* global */`注释声明浏览器是全局的

例外情况有:`window`、`document`和`navigator`。

```
/* global
ADSAFE, report, jslint
*/
```

# 使用对象的规则

## 构造函数名必须以大写字母开头

构造函数名应该以大写字母开头，如下所示。

## 创建实例时，我总是使用构造函数和关键字`‘new'`

使用构造函数是很重要的，因为它可以防止你忘记在`strict mode`中实例化`new`操作符。

这也有助于我的代码更好地融入 JavaScript 主流，并且更有可能在框架之间移植。

如果构造函数没有参数，我还是会写括号。

## 我在私有属性的名字前加了一个下划线

如果你希望一个对象的私有数据是完全安全的，你必须使用闭包，但是这使得代码变得更加复杂。=和 slowers，所以我更喜欢为私有属性添加下划线。

# 其他规则

## 我总是用`===`代替`==`

`===`操作符不会进行转换，所以如果两个值不相同，type `===`将简单地返回 false。

## 我总是使用类型强制

通过 Boolean、Number、String()和 Object()将值强制转换为类型。

## 停止使用关键字`this`作为隐式参数

我不使用关键字`this`作为隐式参数来保持面向对象和功能机制的分离。

## 我通过关键字 in 和 hasOwnProperty()函数检查属性的存在

这比用`undefined`比较一个属性或检查真实性更安全。

## 我总是允许快速失败

最后，如果可以的话，最好快速失败，不要无声无息地失败

有用吧？