# JavaScript 代码样式最佳实践—语句、注释、条件和构造函数

> 原文：<https://javascript.plainenglish.io/javascript-code-styling-best-practices-statements-comments-conditionals-and-constructors-cd1f8ea17163?source=collection_archive---------10----------------------->

![](img/a9b7d65e52fd4554e293c9abb39d2246.png)

Photo by [Leslie Jones](https://unsplash.com/@les_elizabethj?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。编写可以运行但有错误的代码很容易。

在本文中，我们将研究函数中应该包含的语句数量、多行注释、三元表达式之间的换行符，以及命名和调用构造函数的问题。

# 函数中语句的数量

函数中语句的数量应该与函数中的行数相似。这两者都应该最小化。

因此，语句越少，我们的函数就越容易理解。

# 每行的最大语句数

每行的最大语句数应该最小化，这样大多数人不用水平滚动就可以阅读一行中的所有内容。

这意味着一行中的所有内容应该在 100 个字符左右。

太多的语句很难阅读，即使它们用分号分隔。

例如，下面的代码是不好的:

```
const foo = () => { let x = 1; let y = 2; let z = 3; let a = 4; }
```

即使用分号分隔语句，也很长，很难读懂。

但是，下面的要好得多:

```
const foo = () => {
  let x = 1;
  let y = 2;
  let z = 3;
  let a = 4;
}
```

每个赋值语句都在一行中，以便于阅读。

# 多行注释

编写多行注释的更好方法是使用星号块。带星号的代码块类似于:

```
/* comment */
```

这适用于单行和多行注释，因此更加灵活。

如果我们想用它写一个多行注释，那么可以这样写:

```
/*
 * multiline
 * comment 
 */
```

它很简洁，适用于单行和多行注释。

# 三元表达式的操作数之间的换行符

大多数三元表达式应该是一行。如果它不止一行，那么我们可能要写一个`if`语句来代替。

例如，一个短的三元表达式可以写成如下形式:

```
const foo = condition ? 'foo' : 'bar';
```

然而，如果三元表达式的任何部分很长，那么我们可以将它重写为一个`if`语句，如下所示:

```
let foo;
if (condition) {
  foo = [1, 2, 3, 4, 5, 6];
} else {
  foo = [1, 2, 3, 4, 5, 6.7 .8];
}
```

因为我们在上面的代码中有长表达式，所以`if`语句是一个更好的选择，因为如果将两个数组放在一行中，它们可能会溢出页面。

此外，我们不应该有嵌套的三元表达式。它们总是很难阅读，特别是如果表达式没有括号来分隔三元表达式的开始和结束。

例如，我们不应该这样写:

```
const foo = a ? b ? c ? 'foo' : 'bar' : 'baz' : 'qux';
```

像上面这样的嵌套三元表达式很难阅读和理解。

# 构造函数名称应该以大写字母开头

构造函数和类应该以大写字母开头。这是标准的 JavaScript 约定，以区别于其他类型的标识符，如变量和非构造函数。

构造函数和类是等价的，所以它们应该遵循相同的约定。

例如，我们应该让它们如下所示:

```
class Foo {}function Bar() {}
```

然而，一些 JavaScript 内置函数如`Boolean`和`Symbol`打破了这一规则。

他们不是构造函数，但是他们的名字的第一个字母是大写的。

![](img/be637bb2b38a62e4b342d4960bc852cf.png)

Photo by [Christian Fregnan](https://unsplash.com/@christianfregnan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 调用不带参数的构造函数时添加括号

当我们在 JavaScript 中调用不带参数的构造函数时，我们可以不用写出括号。

例如，我们可以如下调用`Foo`类:

```
class Foo {}const foo = new Foo;
```

然而，我们不应该这样做，因为许多人不知道这个 JavaScript 特性。他们可能认为我们在用构造函数或类做别的事情。

因此，为了清楚起见，即使没有参数，我们也应该加上括号。

# 结论

构造函数和类的名字应该以大写字母开头。

如果我们不传入任何参数，即使我们不需要它们，我们也应该用括号调用它们。

三元表达式应该很短。这意味着我们不应该嵌套它们，因为它们很难阅读。

如果很长，那么就用一个`if`语句来代替。

函数中不应该有太多的语句来保持简短。他们应该只做一件事。

带星号的注释块既可以作为单行注释也可以作为多行注释，所以我们只是用它们来注释我们的代码。

# **简明英语团队的笔记**

你知道我们有四种出版物吗？给他们一个关注来表达爱意吧:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)，[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**我们还推出了一个 YouTube，希望你能通过 [**订阅我们的英语频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) 来支持我们**

**一如既往,“简明英语”希望帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****