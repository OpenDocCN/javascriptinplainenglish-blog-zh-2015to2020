# 更好的 JavaScript——没有全局变量或有

> 原文：<https://javascript.plainenglish.io/better-javascript-no-globals-or-with-be8f82ff4d2?source=collection_archive---------17----------------------->

![](img/64b48da1bbaa24e27379237d0fbcab71.png)

Photo by [Marco Zoppi](https://unsplash.com/@mzoe?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究改进 JavaScript 代码的方法。

# 最小化全局对象的用户

全局对象违背了模块化原则。

所有的变量在任何地方都是可用的。

任何东西都可以改变它们。

这是我们与他们合作时遇到的一个问题。

很难追踪它们的价值，因为它们可以被任何东西改变。

当我们使用全局变量时，所有的组件都耦合在一起。

当我们有大的应用程序时，这肯定是不可接受的。

在我们拥有模块之前，全局名称空间是 JavaScript 应用程序的独立部分进行交互的唯一方式。

因此，我们应该使用模块来分离各个部分，让我们使用我们想要的部分。

此外，我们应该小心不要意外地创建全局变量。

如果我们在顶层创建一个带有`var`的变量，我们就可以做到这一点。

例如，我们可以写:

```
var x = 1;
```

我们不应该在顶层创建函数声明。

所以我们不应该:

```
function foo() {}
```

此外，我们不应该通过给没有定义的东西赋值而意外地创建全局变量。

所以不要写这样的话:

```
foo = 1;
```

或者:

```
this.foo = 1;
```

在最高层。

这些语句都更新全局对象。

但是我们可以防止最后两项用 struct 模式创建全局变量。

我们仍然可以使用 JavaScript 标准库中的全局对象。

例如，我们可以使用像`Array`、`Number`、`JSON`等这样的对象。

但是我们绝对不应该创造新的。

如果我们想让应用程序的其他部分可以访问代码，我们应该使用模块。

浏览器支持开箱即用的模块，我们可以使用模块捆绑器将它们构建到一个应用程序中。

# 总是声明局部变量

我们应该总是声明局部变量。

为此，我们可以使用`let`和`const`关键字。

它们都是块范围的，所以变量留在块内。

例如，不要写:

```
function swap(arr, i, j) {
  temp = arr[i];
  arr[i] = arr[j];
  arr[j] = temp;
}
```

相反，我们写道:

```
function swap(arr, i, j) {
  const temp = arr[i];
  arr[i] = arr[j];
  arr[j] = temp;
}
```

我们在变量名前添加了`const`,这样我们就不会声明一个全局变量。

创建全局变量绝对是一种不好的风格。

意外地创造它们更糟糕。

我们可以使用 linter 来自动检查这类错误。

这是像 ESLint 这样的 linters 会检查的事情之一。

# 避免 with 语句

我们不应该在代码中使用`with`语句。

它没有提供很多便利，但是它确实提供了很多不可靠性和低效率。

低效来自于不能通过检查代码来确定范围。

所以，优化是不可能的。

范围的不确定性也意味着处理内部的东西是令人困惑的。

如果我们有这样的东西:

```
function foo(x, y) {
  with(Math) {
    return min(round(x), sqrt(y)); 
  }
}
```

然后我们不知道`x`和`y`是来自参数还是来自`Math`物体。

因此，我们不应该使用它。

![](img/5bae9a6c185c66c142ddf500d013d7d3.png)

Photo by [Gigi](https://unsplash.com/@ling_gigi?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该总是避免创建全局变量，避免使用`with`语句。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**