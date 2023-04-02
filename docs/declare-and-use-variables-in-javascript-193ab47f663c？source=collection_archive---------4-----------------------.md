# 在 JavaScript 中声明和使用变量

> 原文：<https://javascript.plainenglish.io/declare-and-use-variables-in-javascript-193ab47f663c?source=collection_archive---------4----------------------->

变量是 JavaScript 程序的基本构件。它们是代表存储在内存中的实体的名称。在 JavaScript 中，变量可以包含几乎任何内容。它们可以包含数字、对象、布尔值、字符串、函数、数组以及这些东西的组合。当你声明一个变量时，它是一个引用内存中这些东西的名字。换句话说，你得到实体并用变量操纵它。

它们允许您组合数据，并使用各种运算符进行更改。我们可以加减乘除数字。此外，我们可以连接字符串，调用函数，它们一起可以组合对程序员和用户有用的结果。

JavaScript 中的变量有数据类型。数字、对象、布尔和字符串都是类型。还有 JavaScript 中特有的`undefined`类型，它表示没有设置值或者还没有定义的变量。因为变量可以有不同的类型，所以必要时可以转换成不同的数据类型。例如，只有数字内容的字符串可以转换成数字，反之亦然。我们可以用各种操作符和函数来实现，比如用`+`将字符串转换成数字，用`toString`函数将数字转换成字符串。

![](img/387559a764843b7b883b51012d95abd7.png)

Photo by [Christopher Gower](https://unsplash.com/@cgower?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 声明变量

声明变量是 JavaScript 程序最基本的部分。我们需要通过使用不同的关键字来声明我们将要使用的任何变量。在现代 JavaScript 中，我们应该使用`let`关键字来声明变量。我们使用`let`,因为它允许我们声明块范围的变量，这意味着如果它是在函数或类中声明的，那么它不能在那里之外被改变。为了使用`let`来声明一个变量，我们把:

```
let a = 1;
```

等号是赋值运算符。右侧的值被设置为左侧的变量。所以在这种情况下，变量`a`的值为 1。这声明了基本类型，但是我们也可以将一个对象赋给一个变量，就像这样:

```
let a = { num: 1 };
```

你可能也听说过`var`关键字，但是我们不应该使用它，因为范围不是恒定的。与用`var`声明的变量在同一级别声明的任何函数或其他代码，或者嵌套在这些实体中的任何东西，都可以修改用`var`声明的变量，这不会发生在用`let`声明的变量上。

我们也可以将一个变量赋给另一个变量。例如，我们可以写:

```
let a = 1
let b = a // 1
```

上述示例将`a`设置为 1，然后将`b`分配给被赋值为 1 的`a`。`b`将从`a`获得值 1。

`const`是我们用来声明常量的关键字。JavaScript 常量在设置后不能被赋予新值。然而，我们可以用`const`修改对象集内部的值。所以我们不能写:

```
const a = { num: 1};
a = 2; // error
```

但是，我们可以写:

```
const a = { num: 1 };
a.num = 2;
```

`a`的值将会是`{num: 2}`。`const`非常有用，因为在这种情况下很难意外覆盖对象的值。

如果`'use strict'`没有被添加到我们的代码之上，我们也可以声明全局变量。我们通过编写`a = 1;`来声明全局变量，它在全局范围内声明`a`。这意味着在任何地方编写的代码都可以修改它。这与将`a`附加到`window`对象上是一样的，因此`a = 1`与`window.a = 1`是一样的。意外覆盖`a`的值太容易了，所以我们不应该声明全局变量，除非你在`window`对象中使用了什么。

![](img/09bbde56284b20da835f6517ba3ded82.png)

Photo by [Clément H](https://unsplash.com/@clemhlrdt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 变量命名

变量可以有多种命名方式。给变量命名时，有几条规则适用。它们可以以大写或小写字母、下划线或美元符号开头。JavaScript 中的变量是区分大小写的，所以`a`和`A`不一样。此外，它们不能包含空格。您可以根据自己的意愿为变量命名，但是命名太长可能会让其他开发人员在阅读您的代码时感到困惑。

变量名不能与下面列出的保留关键字相同:

*   摘要
*   其他
*   实例 of
*   转换
*   布尔型
*   列举型别
*   （同 Internationalorganizations）国际组织
*   同步的
*   破裂
*   出口
*   连接
*   这
*   字节
*   延伸
*   长的
*   扔
*   情况
*   错误的
*   当地的
*   投
*   捕捉
*   最后的
*   新的
*   短暂的
*   茶
*   最后
*   空
*   真实的
*   班级
*   漂浮物
*   包裹
*   尝试
*   常数
*   为
*   私人的
*   类型 of
*   继续
*   功能
*   保护
*   定义变量
*   调试器
*   转到
*   公众的
*   空的
*   系统默认值
*   如果
*   返回
*   不稳定的
*   删除
*   工具
*   短的
*   在…期间
*   做
*   进口
*   静电
*   随着
*   两倍
*   在
*   极好的

![](img/0dd9de8b1a621f0ccf701e4cc2978d53.png)

Photo by [Safar Safarov](https://unsplash.com/@codestorm?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 数据类型

JavaScript 变量有数据类型，它根据内容对存储的数据类型进行分类。他们区分一种数据和另一种数据。例如，单词和数字不一样。然而，JavaScript 是一种松散类型的语言，这意味着不同类型的数据都属于一个变量。这意味着我们在比较变量和处理变量的时候，必须小心，不要处理错误的数据。这也意味着当数据被比较时，如果可能的话，它们会被自动转换成相同的类型来进行比较。

JavaScript 已经给出了基本数据类型。它有数字、字符串、布尔、NaN 和未定义类型。

## 数字类型

顾名思义，number 类型的变量存储数字。JavaScript 数字的范围从 5e-324 (-5 后跟 324 个零)到 1.7976931348623157e+308(与 179769313486231570000000000000000000000000000000000000000000000000000000 相同

您可以通过编写以下代码来声明数字变量:

```
let x = 1;
```

我们可以将任何类型的变量转换成数字。如果你把一个数转换成一个数，你仍然得到一个数。`Number`函数允许我们将任何类型的变量转换成数字是可能的。如果不能，返回`NaN`，这意味着被转换的变量不是数字。

具有数字内容的字符串可以转换为数字。

`Number(“1”) // returns number 1`

无法转换为数字的文本字符串返回值 NaN:

`Number(“duck”) // returns NaN`

布尔值`true` 返回数字 1:

`Number(true) // returns 1`

布尔值`false` 返回数字 0:

`Number(false) // returns 0`

其他 falsy 值，这意味着如果它们被转换为布尔值，它们将返回`false`，也转换为 0。JavaScript 中的 Falsy 值包括空字符串(`“”`)、`0`、`false`、`null`和`undefined`。

举个例子，

```
Number(‘’) // returns 0
Number(null) // returns 0
Number(undefined) // returns 0
Number(0) // returns 0
```

变量前面的`+`符号与`Number`函数的作用相同，所以我们可以这样写:

```
+“1” // returns number 1
+“duck” // returns NaN
+‘’ // returns 0
+null // returns 0
+undefined // returns 0
+0 // returns 0
```

当你操作可能包含数值的变量时，在组合它们之前先把它们转换成数字。然而，当你对两个变量进行乘法或除法运算时，其中一个或两个变量都是字符串，那么在进行运算之前，JavaScript 会把它们都转换成数字。比如说:

```
'2'*'2' // 4
'2'/2 // 1
```

当然，如果一个或两个字符串都没有数字内容，这是行不通的。例如:`'duck'*'goose'`将返回`NaN`，因为它们都不是数字。

## 字符串类型

字符串类型是可以存储任何字符的变量。它可以存储字母、数字、标点符号和特殊转义字符来表示某些字符。转义字符包括:

*   \' —单引号
*   \" —双引号
*   \\ —反斜杠
*   \n —新行
*   \r —回车
*   \t —制表符
*   \b —退格
*   \f —换页

我们可以通过编写以下代码来声明字符串:

```
let a  = 'string';
```

单引号和双引号是一样的。但是，如果您想在字符串中使用引号，那么您必须使用不同于您用来包装字符串的类型，或者使用斜杠字符来转义引号，以便 JavaScript 解释器知道它是引号而不是字符串分隔符。例如，我们写道:

```
let a = "\"This is a string in quotes\"";
```

或者

```
let a = "'This is a string in quotes'";
```

## 模板字符串

从 2015 年开始，现代 JavaScript 也有模板字符串。这非常方便，因为我们可以将变量放在字符串中，这样我们的字符串就可以包含变量，而不必将它们连接起来。我们用反勾号字符声明模板字符串，反勾号字符是键盘上 Escape 键下面的字符。我们通过编写`${a}`将变量放入字符串中，其中`a`是一个变量。

例如，我们可以写:

```
let a = 1;
let b = `There is ${a} chicken.` // There is 1 chicken.
```

## 布尔型

布尔型变量只能包含值`true`或`false`。我们声明如下:

```
let a = true;
```

其他类型的变量可以转换为布尔类型。几乎所有内容都转换为`true`，除了以下内容:

*   `NaN`
*   `undefined`
*   `0`(数值零)
*   `-0`
*   `“”`(空字符串)
*   `false`

## NaN 类型

`NaN` 代表不是一个数字。任何不能转换或计算为数字的东西都将是`NaN`。例如，我们不能求负数的平方根，也不能将非数字字符串转换成数字。

## 未定义的类型

类型为`undefined`的变量意味着变量没有被设置值或者还没有被定义。

这些是 JavaScript 变量在 JavaScript 中声明和使用的基本方式。有了这些操作，随着我们对 JavaScript 其他部分的了解越来越多，我们可以做的不仅仅是声明和分配变量。

![](img/f6cba7bf5f10a9609e7995e096b422da.png)