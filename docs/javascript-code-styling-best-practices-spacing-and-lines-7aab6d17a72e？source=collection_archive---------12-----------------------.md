# JavaScript 代码样式最佳实践—间距和线条

> 原文：<https://javascript.plainenglish.io/javascript-code-styling-best-practices-spacing-and-lines-7aab6d17a72e?source=collection_archive---------12----------------------->

![](img/2f09db9757b84680431c30a3bbee99aa.png)

Photo by [NASA](https://unsplash.com/@nasa?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。编写可以运行但有错误的代码很容易。

在本文中，我们将研究关键字之间的间距和换行符的一致性。

# 关键字前后间距一致

像`try`、`if`、`else`、`class`等关键词。在 JavaScript 语言中有特殊的含义。

它们的间距也不同于其他代码段。通常，在关键字之后，有一个空格，然后是代码的其余部分。

例如，我们写`if`语句如下:

```
if (foo) {} else {}
```

在上面的代码中，我们在`if`后有一个空格，在`else`前后各有一个空格。

同样，对于`try...catch`块，我们有以下代码:

```
try {} catch (ex) {} finally {}
```

`try`和`finally`看起来和`if`和`else`一样，但`catch`略有不同。

在`catch`关键字之后，在`catch`关键字和`ex`的左括号之间有一个空格。

这使它区别于函数调用。

一个类声明如下:

```
class Foo {}
```

我们在类名后面有一个空格字符。箭头函数声明如下:

```
const foo = () => {}
```

正如我们所见，粗箭头前后都有空格。

对于像`instanceof`和`in`这样的特殊运算符，我们在运算符前后都有空格。

例如，我们编写以下代码来使用它们:

```
arr instanceof Array
a in foo
```

在`instanceof`和`in`的前后我们有一个空格字符。

对于函数，左括号紧接在匿名函数的关键字`function`之后。例如，我们编写以下代码:

```
const foo = function() {}
```

对于命名函数，在`function`关键字和函数名之间有一个空格。

我们这样写道:

```
function foo() {}
```

在类中，`constructor`和`super`关键字就像常规函数。左括号紧跟在没有空格字符的关键字之后。

例如，我们写道:

```
class Foo {
  constructor() {
    super();
  }
}
```

对于变量声明，在`var`、`let`或`const`和标识符名称之间总是有一个空格。

所以我们写道:

```
var a;
let b;
const c = 1;
```

对于循环，我们通常在关键字`for`和`while`后面有一个空格。例如，我们编写以下代码:

```
while (foo) {}for (const a of arr) {}
```

# 行注释的位置

行注释的位置应该一致。要么他们总是在自己的线上，要么他们在线外。

所以要么我们写下这样的话:

```
// comment
let foo = 1;
```

贯穿我们的准则或:

```
let foo = 1; // comment
```

把长的注释放在它们自己的行中是有意义的，所以可能把它们放在它们自己的行中更有意义，除非我们所有的注释都是短的。

# 一致的分行样式

换行符样式应该在我们的代码中保持一致。尽管我们不能直接看出区别，但 Windows 和类 UNIX 系统之间的换行符风格是有区别的。

UNIX 换行符样式对于 LF 是`\n`，而 Windows 对于 CRLF 是`\r\n`。

我们应该调整它们，使它们一致。如果我们更多地在 Windows 上开发，那么就使用 Windows 风格的行尾。否则，我们使用 UNIX 风格的行尾。

# 注释周围的空行

注释周围的空行对于可读性来说可能是个好主意。

然而，这并不是一个必要条件，也不会产生太大的差别。

![](img/237fcf6e44cc76cb51fbcb29e67e6018.png)

Photo by [Anika Huizinga](https://unsplash.com/@iam_anih?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 类成员之间的空行

有些类成员之间应该有一个类成员，有些不应该。

如果我们的类成员是实例变量，那么它们之间不需要空行。

然而，我们不把实例变量放在方法之外，除非我们使用 TypeScript。

对于类方法，我们应该在每个类之间加一个换行符。例如，为了可读性，我们应该这样写。

例如，我们编写以下代码，用空行分隔方法:

```
class Foo {
  bar() {} baz() {}
}
```

我们在`bar`和`baz`之间有一条线。

# 结论

换行符应该在类方法之间。

此外，文件之间的换行符应该一致。对于 UNIX，它是`\n`，而 Windows 是`\r\n`。我们只坚持一种风格。

评论可以自成一行。此外，注释和代码之间可以有一条线。

最后，关键字前后的间距也要一致。

## **简明英语团队的笔记**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**我们还推出了一个 YouTube，希望你能通过 [**订阅我们的简明英语频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) 来支持我们**

**一如既往,“简明英语”希望帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****