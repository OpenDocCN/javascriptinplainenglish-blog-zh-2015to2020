# JavaScript 反模式——包装器和函数

> 原文：<https://javascript.plainenglish.io/javascript-antipatterns-wrappers-and-functions-b8580c92dec8?source=collection_archive---------4----------------------->

![](img/f3908e9305e24b8c3b2f9834700fe08d.png)

Photo by [Scott Webb](https://unsplash.com/@scottwebb?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 让我们可以做很多事情。它的语法有时过于宽容。

在本文中，我们将研究一些在定义和使用包装器和函数时应该避免的反模式。

# 原始包装

JavaScript 附带了一组原始包装器。

他们包括`Number()`、`String()`、`Boolean()`构造者。

使用它们创建原始值不同于使用文字。

如果我们使用文字，我们得到:

```
console.log(typeof 100);
```

然后我们看到`'number'`登录到控制台。

另一方面，如果我们使用`Number`构造函数，我们得到:

```
console.log(typeof new Number(100));
```

我们得到`'object'`日志。

JavaScript 解释器内部使用时最需要包装器。

原始文字在调用类似数字上的`toFixed()`的方法之前被转换成对象。

字符串在包装器上也有一些可用的方法，如`toLowerCase()`。

例如，如果我们写:

```
(1 / 7).toPrecision(3);
```

然后`1 / 7`转换成包装器对象，就可以调用`toPrecision`了。

然而，我们不应该用它们来创建对象。

这是因为它们对我们毫无用处。

所以与其写:

```
const s = new String("string");
```

我们写道:

```
const s = "string";
```

它们应该仅隐式用于调用方法:

```
'hello world'.split(' ')[0]
```

我们不想显式地调用构造函数。

# 错误对象

错误构造函数是 JavaScript 标准库的一部分。

它有`Error()`、`SyntaxError()`、`TypeError()`等构造函数。

它们具有以下属性:

*   `name` —错误的名称
*   `message` —创建对象时传递给构造函数的字符串。

它还有其他属性，比如行号和发生错误的文件名。

`throw`适用于任何对象。只要对象有`name`和`message`属性，就可以用`catch`语句捕获。

例如，我们可以写:

```
try {
  throw {
    name: 'error',
    message: 'error occurred'
  }
} catch (ex) {
 console.log(ex)
}
```

然后我们得到:

```
{name: "error", message: "error occurred"}
```

从控制台日志输出。

# 功能

JavaScript 函数是对象。

它们是头等物品。这意味着它们可以动态创建。

此外，还可以将它们赋给变量，复制它们，并向它们添加属性。可以删除它们的属性。

函数也可以作为参数传递给其他函数，这些参数可以由其他函数返回。

他们也可以有自己的方法。

通过`Function`构造函数的存在，我们可以看出它们是对象。

我们不应该用它来创建函数，但我们知道它在那里，所以我们可以看到函数是对象。

函数有自己的作用域。块创建它们自己的变量范围，常量用`let`或`const`声明。

如果我们声明一个函数，那么我们应该使用`let`或`const`来声明块范围的数据。

这将为我们省去许多使用示波器的麻烦。

例如，我们写道:

```
const foo = () => {
  let x = 1;
  const y = 2;
  //...
}
```

![](img/88c940dbbb7a61c0a0a439ffb4a91d53.png)

Photo by [British Library](https://unsplash.com/@britishlibrary?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 功能类型

JavaScript 中有两种函数。

它们是函数表达式和函数声明。

当我们把函数赋给一个变量或常数时，我们就创建了一个函数表达式。

例如，我们可以写:

```
const foo = () => {
  let x = 1;
  const y = 2;
  //...
};
```

或者:

```
const foo = function() {
  let x = 1;
  const y = 2;
  //...
};
```

函数表达式也可以命名为:

```
const foo = function bar() {
  let x = 1;
  const y = 2;
  //...
};
```

但我们还是用`foo`来称呼它。

然而，如果我们不需要引用`this`，箭头函数是首选，如果我们不创建构造函数，我们很可能不需要引用。

函数声明是通过定义传统函数而创建的，无需将其赋给变量或常数。

例如，我们写道:

```
function foo() {
  // function body
}
```

它们被提升起来，以便可以在脚本中的任何地方引用它们。

函数声明不需要分号。

# 结论

我们不应该使用原始包装器来定义原始值。

它们应该只在 JavaScript 解释器内部使用，这样我们就可以用原始值调用方法。

函数可以用几种方法创建。如果我们使用函数表达式，我们应该在末尾加上分号。

我们应该在函数中使用块范围的变量和常量，以避免与范围混淆。

# **用简单英语写的便条**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页[**plain English . io**](https://plainenglish.io/)找到所有这些内容——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**