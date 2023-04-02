# JavaScript 错误—无用的代码和无效的正则表达式

> 原文：<https://javascript.plainenglish.io/javascript-mistakes-useless-code-and-invalid-regex-8f3e48718105?source=collection_archive---------7----------------------->

![](img/7c4fdc502199b8093b3aa7c9efd83a1d.png)

Photo by [Chuan Xu](https://unsplash.com/@chuanxu?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。编写可以运行但有错误的代码很容易。

在本文中，我们将看看一些无用的代码，我们不需要把它们放到我们的 JavaScript 代码中。

# 没有多余的括号

除非我们想把优先级较低的东西组合在一起，否则我们不需要像算术表达式那样用括号括起来。

因此，我们在表达式中不需要括号，比如:

```
(a * b);
```

或者:

```
(a * b) + c;
```

此外，我们不需要在循环变量周围加上括号:

```
for (let a of (b)){
 //,,,
}
```

我们可以去掉循环中`b`两边的括号。

一些运算符表达式也不需要括号，如:

```
typeof (foo);
```

我们不需要用圆括号将`foo`括起来。

下面是另一个有额外括号的例子:

```
(function(){} ? foo() : bar());
```

我们可以删除行首和行尾的括号。

如果我们选择在一个条件中进行赋值，那么我们应该用括号把它们括起来，这样可以清楚地表明我们在给哪个变量赋值:

```
while ((foo = bar())) {}
```

例外是一个普通的`for`循环，我们写它时没有括号:

```
for (let i = 0; i < 10; i++) {
  //...
}
```

像循环中的赋值一样，为了清楚起见，我们也应该在`return`语句中的赋值表达式两边加上括号:

```
const foo =(b) => {
  return (b = 1);
}
```

# 删除多余的分号

JavaScript 允许我们在一行的末尾放置尽可能多的分号。它的作用和单个分号一样，所以多余的分号没有用。

我们也不需要在函数声明后使用分号。

例如，在下面一行中我们不应该有多个分号:

```
let x = 1;;;
```

相反，我们应该写:

```
let x = 1;
```

下面的代码是函数声明的一个示例:

```
function foo() {};
```

在上面的代码中，我们不需要额外的分号。相反，我们应该写:

```
function foo() {}
```

# 将函数声明重新分配给不同的值

因为 JavaScript 函数只是常规变量，所以它们也可以被重新分配给任何东西。

这通常是一个错误的迹象，因为我们可能想调用这个函数，而不是把它赋给别的函数。

例如，我们不想写:

```
function fn() {}
fn = foo;
```

相反，我们应该显式地创建一个变量，并为该变量分配一个函数，以表明它实际上是一个我们希望重新赋值的变量:

```
const fn = function() {}
fn = foo;
```

![](img/ebb485502284276c296a0ed2e25fc60f.png)

Photo by [Christopher Carson](https://unsplash.com/@bhris1017?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 将从模块导入的成员赋给另一个值

因为 JavaScript 模块的导入成员是只读的，所以我们不能给它们赋值。因此，我们应该有这样的代码:

```
import { x, y } from "./foo";
x = 1;
```

我们会得到一个错误，即`x`是只读的。

这也适用于默认导出的导入。

例如:

```
import x from "./foo";
x = 1;
```

也给出同样的错误。

下面没有给出一些构建工具的错误:

```
import * as foo from "./foo";
foo.x = 1;
```

# 嵌套块中的函数声明

在 ES6 之前，嵌套块中的函数声明应该在程序的顶层或另一个函数的主体中。

然而，一些 JavaScript 解释器允许将函数声明添加到嵌套块中。

例如:

```
function foo() {
  if (true) {
    function bar() {}
  }
};
```

不应该是有效的 JavaScript，因为我们在`foo`函数的`if`块中声明了`bar`函数。

为了使我们的代码语法正确，我们应该从`if`块中删除`bar`函数声明。如果我们想在代码块中定义一个函数，我们也可以在代码块中使用函数表达式。

# RegExp 构造函数中的正则表达式无效

我们不希望在`RegExp`构造函数中出现无效的正则表达式。构造函数接受任何字符串，并尝试从中返回正则表达式对象。

无效的正则表达式定义包括如下内容:

```
RegExp(']')
RegExp('-', 'z')
```

我们应该确保传入一个有效的正则表达式字符串或者使用正则表达式文字。

# 结论

JavaScript 解释器允许很多事情，但是它们要么没用，要么会导致错误。例如，应该删除无用的括号和分号。函数声明也不应该被重新赋值给新值。

块中也不允许函数声明，即使 JavaScript 解释器仍然可以运行它。

最后，无效的 regex 字符串不应该传递给`RegExp`构造函数。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****