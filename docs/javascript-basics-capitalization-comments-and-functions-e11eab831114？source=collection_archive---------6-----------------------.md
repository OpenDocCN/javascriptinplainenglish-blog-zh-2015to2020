# JavaScript 基础——大写、注释和函数

> 原文：<https://javascript.plainenglish.io/javascript-basics-capitalization-comments-and-functions-e11eab831114?source=collection_archive---------6----------------------->

![](img/d28efdc8027afe5f892f737d5ca527bd.png)

Photo by [Nick Fewings](https://unsplash.com/@jannerboy62?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是世界上最流行的编程语言之一。为了有效地使用它，我们必须了解它的基础知识。

在本文中，我们将查看代码和函数的大写和注释。

# 资本化

变量名不能有空格。然而，我们可能想用多个词。

因此，我们必须把它们粘在一起。

为此，我们可以写道:

*   `fooBar`
*   `foo_bar`
*   `foobar`
*   `FooBar`

这些都是最可能的选择。

所有小写字母都很难读，所以第三种方式不好。

下划线有点难打，因为我们必须找到钥匙。

因此，最常见的款式是 1 号和 4 号。

第一个用于大多数变量，而最后一个用于构造函数和类。

# 评论

原始代码可能不会传达我们想传达给其他人的所有信息。

这就是为什么注释存在于代码中。

它让我们能够解释代码中没有解释的东西。

例如，我们可以写:

```
// create tax report from database
let report = new Report();
```

我们可能不知道报告数据在数据库中，所以我们会传达这些信息。

`//`字符表示为单行注释。

我们可以通过编写以下内容来编写多行注释:

```
/*
 * create tax report from database
 */
```

现在我们可以写一行以上的东西。

`/*`和`*/`是分别表示多行注释的开始和结束的字符。

# 功能

函数是 JavaScript 程序的构建块。

我们将一堆代码打包成一个组。

例如，我们可以通过编写以下内容来定义一个函数:

```
const cube = num => {
  return num ** 3;
}
```

`cube`函数取一个参数`num`，然后返回它的立方。

函数有一组参数，可以是 0 或更多，还有一个主体，它有一堆在调用函数时运行的语句。

身体被大括号包围着。

它必须总是用大括号包装，除非我们有一个单语句函数的隐式返回。

例如，我们可以写:

```
const cube = num => num ** 3;
```

因为该函数是单语句函数，并且它返回相同的值。

不是所有的功能都必须产生价值，但是如果它们不产生价值，它们就会产生`undefined`。

# 领域

作用域是程序中变量可见的部分。

JavaScript 变量可以是函数、块或全局范围。

使用`let`和`const`关键字创建块作用域变量。

函数作用域变量是用`var`关键字创建的。

并且全局变量是在没有关键字或作为全局对象的属性的情况下创建的。

为函数参数或函数内部创建的变量只能在函数中引用。

它们被称为本地绑定。

例如，我们可以写:

```
if (true) {
  let y = 20;
  const z = 30;
  console.log(y + z);
}
```

那么`y`和`z`仅在`if`区块内部可用。

如果在块之前定义了一个变量，并且该变量处于更高的级别，则还可以从块内部引用该变量。

例如，我们可以写:

```
const z = 30;
if (true) {
  let y = 20;
  console.log(y + z);
}
```

而`z`仍然可以从`if`区块进入。

如果我们有一个`var`变量:

```
var x = 1;
```

那么如果它在顶层，整个脚本都可以使用它。

我们甚至可以在它们被定义之前引用它们，但这不会非常有用，因为它会在被赋予值之前`undefined`。

全局变量是指在整个应用程序中可用的变量。

例如，如果我们有:

```
x = 1;
```

然后我们声明了一个全局变量。

我们不能在 JavaScript 严格模式下这样做，因为我们不想意外地声明全局变量。

![](img/e022b7ee7fa88b7e6577f86d48ea84cc.png)

Photo by [Annie Spratt](https://unsplash.com/@anniespratt?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

大写对可读性很重要。我们可能应该坚持 JavaScript 的命名约定，也就是 camelCase 或 PascalCase。

函数是 JavaScript 程序的重要组成部分。变量的可用性取决于它们的声明方式。

注释可以帮助我们解释代码中没有解释的东西。

## **JavaScript 简单明了**

你知道我们有四份出版物吗？请通过 [**用通俗易懂的英语找到它们。io**](https://plainenglish.io/)——请关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **，彰显您的爱！**