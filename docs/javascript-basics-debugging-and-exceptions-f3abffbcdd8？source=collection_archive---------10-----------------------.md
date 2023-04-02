# JavaScript 基础—调试和异常

> 原文：<https://javascript.plainenglish.io/javascript-basics-debugging-and-exceptions-f3abffbcdd8?source=collection_archive---------10----------------------->

![](img/98e81b996287ef6925b51c4beb27862b.png)

Photo by [Quino Al](https://unsplash.com/@quinoal?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是世界上最流行的编程语言之一。为了有效地使用它，我们必须了解它的基本知识。

在本文中，我们将研究如何调试 JavaScript 程序和错误传播。

# 排除故障

我们可以用`console.log`调试我们的程序。

它记录表达式返回的值。

它可以接受多个参数。

例如，我们可以写:

```
console.log(foo);
```

或者:

```
console.log(foo, bar);
```

它可能需要更多的参数。

所有参数的值都将显示在开发人员控制台中。

我们可以通过记录我们认为会引起问题的表达式来战略性地使用`console.log`。

一个替代`console.log`的方法是使用`debugger`关键字。

这将在浏览器中设置一个断点。

它被到达的`debugger`语句暂停，我们可以在那时检查变量的值。

`debugger`的实现因浏览器而异。

# 错误传播

不是所有的错误都能被程序员避免。

如果一个程序与外界通信，那么它们可能有问题。

我们希望优雅地处理这些情况，而不是让程序崩溃。

例如，如果我们用`prompt`函数提示输入一个数字，而用户输入的不是数字。那我们就要防止这种情况。

我们可以写:

```
let result = Number(prompt('How many cats do you have?'));
if (Number.isNaN(result)) {
  console.log('please input a number')
} else {
  console.log(`You have ${result} cats`)
}
```

现在，如果用户没有输入数字，他们会得到`'please input a number'`。

否则，``You have ${result} cats``，其中`result`是用户输入的。

我们应该检查程序不期望的值，并处理它们。

许多错误是常见的，我们的代码应该考虑到它们。

# 例外

当一个功能不能正常运行时，我们可能想停下来处理这个问题。

这就是异常处理的作用。

异常是一种机制，使遇到问题的代码有可能引发或抛出异常。

例外可以是任何值。

错误对象具有导致错误的调用堆栈。

如果我们有:

```
let result = Number(prompt('How many cats do you have?'));
if (Number.isNaN(result)) {
  throw new Error('invalid number');
} else {
  console.log(`You have ${result} cats`)
}
```

然后，我们会在控制台日志中显示一个错误，并在控制台日志中显示调用堆栈跟踪。

`throw`关键字用于引发异常。

捕捉一个是用一个`try`块包装代码，然后是`catch`。

在`catch`程序块完成后，或者如果`try`程序块运行没有问题，那么程序运行`try/catch`语句下的代码。

例如，我们写道:

```
const promptCats = () => {
  let result = Number(prompt('How many cats do you have?'));
  if (Number.isNaN(result)) {
    throw new Error('invalid number');
  } else {
    console.log(`You have ${result} cats`)
  }
}try {
  promptCats();
} catch (err) {
  console.log(err)
}
```

然后，如果抛出一个错误，我们捕获由`promptCats`抛出的错误。

我们使用`Error`构造函数来创建我们自己的错误对象。

这样，我们可以获得所有的错误信息，包括堆栈跟踪

如果我们需要使用堆栈跟踪来调试问题，它位于`stack`属性中。

它告诉我们问题发生在哪里，哪些函数导致了调用失败。

# 清理

如果需要，我们必须在抛出异常后进行清理。

为此，我们使用`finally`子句并在其中运行代码。

例如，我们写道:

```
try {
  errorCode();
} catch (ex) {
  //...
} finally {
  //...
}
```

我们有`finally`来运行任何代码，不管是否抛出异常。

这是运行清理代码的一个方便的地方。

![](img/75d6dbcbd9ed3ab1183baf0e1f28b615.png)

Photo by [MIO ITO](https://unsplash.com/@mioitophotography?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

要调试，我们可以使用`debugger`语句或者`console.log`。

我们应该检查代码中的错误情况。

如果我们遇到错误情况，异常对于抛出是有用的。

我们可以通过使用`throw`关键字和将代码分别放在`try/catch`块中来抛出和捕捉异常。

为了不管结果如何都运行代码，我们可以使用`finally`子句。