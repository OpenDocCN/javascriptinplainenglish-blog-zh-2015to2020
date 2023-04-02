# JavaScript 最佳实践—传播语法和模板字符串

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-spread-syntax-and-template-strings-28b790d4b18a?source=collection_archive---------5----------------------->

![](img/8ed6565b75b411fdadf7643454cac8bf.png)

Photo by [Siora Photography](https://unsplash.com/@siora18?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。编写可以运行但有错误的代码很容易。

在本文中，我们将研究如何使用 spread 语法来调用函数，以及如何使用模板字符串来代替字符串连接。

# 使用扩展语法代替`.apply()`

spread 语法允许我们调用函数，将数组展开到函数中以逗号分隔的参数列表中。

这比旧的方法简单多了，在旧的方法中，我们必须调用带有一组参数的函数的`apply`方法来调用带有许多参数的函数。

例如，使用旧的`apply`方法，要用`Math.min`方法获得一个数字数组的最小值，我们必须编写以下代码:

```
const min = Math.min.apply(null, [1, 2, 3, 4, 5]);
```

在上面的代码中，`apply`方法调用的第一个参数是`this`的值，也就是`null`，因为`Math.min`是静态方法，所以我们不需要为`this`设置值。

第二个参数是我们要传递给方法的参数数组。

因此，我们得到`min`的值 1，这是数组中的最小值。

有了函数调用的 spread 语法，我们不需要做所有的事情来调用一个函数，将一个参数数组展开成一个函数的逗号分隔的参数列表。

我们可以使用`...`操作符将一个参数数组展开成一个逗号分隔的参数列表。

要用 spread 语法调用`Math.min`,我们只需编写以下代码:

```
const min = Math.min(...[1, 2, 3, 4, 5]);
```

正如我们所看到的，我们从方法调用中获得了相同的返回值 1，但是要做同样的事情，我们需要输入的内容要少得多。同样，我们不必使用`apply`方法用一系列参数来调用它。

# 使用模板字符串而不是字符串串联

模板字符串是 ES6 发布的另一个很棒的特性。它让我们将表达式插入到字符串中，从而得到一个动态字符串。

这比字符串连接要好得多，因为这很快会与所有那些连接字符串的`+`符号混淆。

例如，不用编写下面的代码:

```
const greeting = 'hi';
const firstName = 'jane';
const lastName = 'smith';
const greetingStr = greeting + ', ' + firstName + ' ' + lastName;
```

当我们用所有的`+`符号和引号阅读它时，这是令人困惑的。我们用模板字符串编写以下代码:

```
const greeting = 'hi';
const firstName = 'jane';
const lastName = 'smith';
const greetingStr = `${greeting}, ${firstName} ${lastName}`;
```

在上面的代码中，我们使用了模板字符串文字，这让我们可以用分隔符`${}`将表达式插入到字符串中。

我们将`greeting`、`firstName`和`lastName`变量直接插入到模板字符串中。

因为我们不必到处处理`+`操作符和引号，所以更加清晰。

此外，模板字符串由反斜杠分隔，因此我们可以使用单引号和双引号来引用文本，而不是作为字符串分隔符。

它对空格和换行符也很敏感，所以我们可以用它来创建多行字符串而不会有太多麻烦。

要创建多行字符串，我们只需编写以下代码:

```
const greeting = 'hi';
const firstName = 'jane';
const lastName = 'smith';
const greetingStr = `${greeting}, 
${firstName} ${lastName}`;
```

那么我们得到`greetingStr`的值是:

```
hi, 
jane smith
```

这很好，因为创建多行字符串的老方法是使用换行符和连接。

因此，我们必须编写类似下面的代码来创建一个多行字符串:

```
const greeting = 'hi';
const firstName = 'jane';
const lastName = 'smith';
const greetingStr = greeting + ',\n' + firstName + ' ' + lastName;
```

在上面的代码中，我们在`,`后面添加了一个额外的`\n`来创建一个新行。

如果我们有更多的换行符来创建换行符，那么代码会变得更加混乱。

![](img/df33109682c03a692569cc51f69cbd7e.png)

Photo by [Vitaly Sacred](https://unsplash.com/@vitalysacred?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

扩展语法和模板字符串都是现代 JavaScript 的重要特性。

spread 语法允许我们在 JavaScript 代码中将数组展开成逗号分隔的参数列表。

模板字符串允许我们在代码中插入 JavaScript 表达式，这比把它们连接在一起要好得多。它还允许我们创建多行字符串，而无需显式添加换行符。

# **简明英语团队的笔记**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页[**plain English . io**](https://plainenglish.io/)找到所有这些内容——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**