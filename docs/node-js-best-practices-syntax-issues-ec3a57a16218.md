# Node.js 最佳实践—语法问题

> 原文：<https://javascript.plainenglish.io/node-js-best-practices-syntax-issues-ec3a57a16218?source=collection_archive---------6----------------------->

![](img/fb54677f553040d70bda4d3b36227b14.png)

Photo by [Cristina Anne Costello](https://unsplash.com/@lightupphotos?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

与任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将探讨在编写 Node 应用程序时应该遵循的一些最佳实践。

# 在同一行上启动代码块的花括号

大括号应该与开始语句在同一行。

例如，不写:

```
function foo() 
{
  // code block
}
```

我们写道:

```
function foo() {
  // code block
}
```

这有助于我们避免意外结果。

如果我们有:

```
function foo()
{
  return;
  { 
    bar: "fantastic"
  };
}
```

那么`return`和对象被认为是分开的。

如果我们把开头的花括号放在`return`旁边，那么它们将被认为是一个陈述。

# 正确区分报表

我们应该适当地分开陈述。

例如，我们可以写:

```
function doThing() {
  // ...
}

doThing()const items = [1, 2, 3]
items.forEach(console.log)
```

另一方面，我们应该避免拼写错误，比如:

```
const m = new Map()
const a = [1,2,3]
[...m.values()].forEach(console.log)
```

最后两行被认为是相同的语句，将引发语法错误。

另一个例子是:

```
const count = 2
(function foo() {
  // do something
}())
```

2 被认为是一个新行上带有括号的函数。

为了避免所有这些问题，我们应该用分号把它们分开。

# 说出我们的职能

我们应该给函数命名，以便在调试时可以按名称跟踪函数。

如果我们发现匿名函数的内存消耗存在严重问题，则使用核心转储进行调试可能是一个挑战。

# 对变量、常量、函数和类使用命名约定

变量、常量、函数和类的命名约定应遵循通用约定。

命名常量、变量和函数时应使用小写 camel。

上 camel 大小写应用于类。

这有助于我们区分普通变量或函数和类。

此外，我们应该使用描述性名称，但要保持简短。

# 比起 let，更喜欢 const，抛弃 var

`var`不能再用来声明变量了。

它们的范围很微妙。

`let`和`const`都是区块划分的，所以能找到它们的地方很清楚。

`const`比`let`更好，因为我们不能将它们重新分配给新值。

# 首先需要模块，而不是内部函数

模块应该在模块的顶部是必需的，以便我们可以在模块加载时发现错误和其他问题。

如果它们是内部函数，那么只有当我们运行代码时，才能看到`require`的问题。

所以这不是个好主意。

此外，`require` s 由 Node 同步运行，所以如果它们花费很长时间，那么它们可能会阻塞`require`之后的代码。

# 按文件夹要求模块，而不是直接按文件

我们应该放置一个`index.js`文件，它公开了模块的间隔，这样消费者就可以通过它。

这让我们可以创建一个界面，在不违反合约的情况下使未来的更改变得更容易。

例如，我们可以写:

```
module.exports.foo = require("./foo");
module.exports.bar = require("./bar");
```

而不是:

```
module.exports.foo = require("./foo/foo.js");
module.exports.bar = require("./bar/bar.js");
```

以避免直接在文件夹中导入 JavaScript 模块。

![](img/70b679ee0b20fdd75b4d062b6f4b3a14.png)

Photo by [Xiang Gao](https://unsplash.com/@xianggao?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该考虑语法变化，使我们的生活更容易，并避免错误。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**