# 可维护的 JavaScript —处理错误

> 原文：<https://javascript.plainenglish.io/maintainable-javascript-handling-errors-752a1f943d2d?source=collection_archive---------11----------------------->

![](img/8a08188446ac0f5170cd5380c92d65a1.png)

Photo by [Michael Geiger](https://unsplash.com/@jackson_893?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果想继续使用代码，创建可维护的 JavaScript 代码很重要。

在本文中，我们将通过查看处理错误的方法来了解创建可维护的 JavaScript 代码的基础。

# 投掷失误的优势

抛出我们自己的错误允许我们提供浏览器显示的准确文本。

除了行号和列号之外，我们还可以包含任何我们喜欢的信息。

提供的信息越多，我们就越容易找到问题。

例如，如果我们有:

```
function getDivs(element) {
  if (element && element.getElementsByTagName) {
    return element.getElementsByTagName("div");
  } else {
    throw new Error("divs not found");
  }
}
```

如果在元素中没有发现 div，我们就会抛出一个错误。

知道了这一点，问题就更容易发现了。

# 何时抛出错误

到处抛出错误是不切实际的，会降低我们应用程序的性能。

例如，如果我们有:

```
function addClass(element, className) {
  if (!element || typeof element.className !== "string") {
    throw new Error("element class name must be a string");
  }
  if (typeof className !== "string") {
    throw new Error("class name must be a string");
  }
  element.className += ` ${className}`;
}
```

那么大部分功能都被错误抛出代码占用了。

这在 JavaScript 中是多余的。

我们应该只为最有可能抛出错误的代码抛出错误。

所以我们不需要为`className`参数抛出错误。

相反，我们写道:

```
function addClass(element, className) {
  if (!element || typeof element.className !== "string") {
    throw new Error("element class name must be a string");
  }
  element.className += ` ${className}`;
}
```

`element.className`很可能不是一个字符串，所以我们检查它。

已知实体不需要错误检查。

如果我们不能提前确定函数将被调用的位置，那么在出现意外情况时抛出错误是一个好主意。

对于已知的错误情况，JavaScript 库应该从它们的公共接口抛出错误。

这样，如果程序员使用不当，他们可以知道问题出在哪里。

调用堆栈应该在库的接口中终止。

这也适用于私人图书馆。

一旦我们修复了难以调试的错误，我们应该抛出一些让我们更容易识别问题的错误。

如果我们正在编写代码，如果它做了一些意想不到的事情，可能会把事情弄糟，那么我们应该检查这些条件并抛出错误。

如果代码将被我们不认识的人使用，那么代码的公共接口应该为意料之外的事情抛出错误。

# try-catch 语句

JavaScript 为我们提供了 try-catch 语句，让我们在浏览器处理抛出的错误之前拦截它们。

可能导致错误的代码在 try 块中，错误处理代码在 catch 块中。

例如，我们可以写:

```
try {
  errorFunc();
} catch (ex) {
  handleError(ex);
}
```

我们还可以在`catch`块下面添加一个`finally`子句。

`finally`块中的代码不管是否发生错误都会运行。

例如，我们可以写:

```
try {
  errorFunc();
} catch (ex) {
  handleError(ex);
} finally {
  cleanUp();
}
```

`finally`条款可能很棘手。

如果`try`子句有一个`return`语句，那么在`finally`运行之前它不会返回。

![](img/e3a2f4203fc2eed66b52cc36f7ebe545.png)

Photo by [傅甬 华](https://unsplash.com/@hhh13?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该只在接口级别抛出错误，并且是针对最经常发生的错误。

此外，我们可以使用`try-catch-finally`子句来处理错误。

![](img/787be6c671be8d345dc786dad8729ce5.png)

[Subscribe to Decoded, our official YouTube channel!](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)