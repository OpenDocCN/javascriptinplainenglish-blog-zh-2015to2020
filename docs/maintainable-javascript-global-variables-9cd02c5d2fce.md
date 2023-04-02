# 可维护的 JavaScript —全局变量

> 原文：<https://javascript.plainenglish.io/maintainable-javascript-global-variables-9cd02c5d2fce?source=collection_archive---------6----------------------->

![](img/8474cec48907ceb1c2e9e5ad0b0ad71f.png)

Photo by [Kyle Glenn](https://unsplash.com/@kylejglenn?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果想继续使用代码，创建可维护的 JavaScript 代码很重要。

在本文中，我们将通过避免全局变量来了解创建可维护 JavaScript 代码的基础。

# 避免全局

JavaScript 的执行环境是独一无二的，因为我们过去常常使用大量的全局变量和函数。

JavaScript 的默认执行环境是处处使用全局变量。

我们拥有的一切都被定义为全局对象的属性。

它是一个表示脚本最外层上下文的对象。

`window`是浏览器中的全局对象。

因此，在全局范围内声明的任何变量或函数都成为了`window`对象的属性。

例如，我们有:

```
var color = "red"
```

或者

```
function getColor() {
  console.log(color);
}
```

然后，我们可以通过使用以下公式获得这些值:

```
console.log(window.color)
```

或者:

```
window.getColor();
```

然而，到处创建全局变量是一种不好的做法，因为它们有很多问题。

它的一个问题是命名冲突。

因为所有东西都在同一个范围内，所以很可能我们在全局范围内有命名冲突。

我们可能已经在别的地方定义了`color`全局变量。

这将覆盖先前定义的值。

`getColor`变量取决于`color`，所以很难追踪到`color`的实际值。

我们不知道`color`从何而来。

此外，我们还面临着这样的风险:这个全局变量以后可能会变成内置的浏览器全局变量。

# 代码脆弱性

拥有全局变量使得代码与环境紧密耦合。

如果环境改变了，那么这个功能就有可能中断。

如果`color`变量不再存在，则`getColor`方法记录`undefined`。

这意味着对全局环境的任何更改都可能导致整个代码出错。

任何函数都可以随时更改全局变量。

这意味着全局变量的可靠性也值得怀疑。

为了使我们的代码更健壮，我们应该避免全局变量。

所以我们不直接在我们的`getColor`函数中记录`color`，而是从一个参数中记录`color`。

例如，我们可以写:

```
function getColor(color) {
  console.log(color);
}
```

改为从参数中获取`color`。

这样，我们就知道它从哪里来了。

# 测试难度

拥有全局变量也使得我们的代码难以测试。

如果测试依赖于全局变量，那么创建测试是非常困难的，因为它们可以被任何东西改变。

由全局变量引起的代码不同部分之间的紧密耦合会导致不可预知的结果。

因此，我们必须通过消除对全局变量的依赖来解决这个问题。

我们不应该创建自己的全局变量。

但是我们可以依赖 JavaScript 自带的全局变量，比如`Array`或`Date`。

![](img/11bf53f19991746e1de4a49a9b5ebe50.png)

Photo by [Adrian Dascal](https://unsplash.com/@dascal?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该尽可能避免全局变量。

它们使得我们的代码难以测试。

我们创建的代码是脆弱的，因为全局变量可以在任何地方被任何东西改变。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**