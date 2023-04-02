# 现代 JavaScript 的精华——Regex u 标志和计时器

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-regex-u-flag-and-timers-9627d7e51a7b?source=collection_archive---------6----------------------->

![](img/2942b9765688f99fdb651abb5015925c.png)

Photo by [Bonneval Sebastien](https://unsplash.com/@gentlestache?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究新的 JavaScript regex 特性和计时器。

# u 形旗

`u`标志是 ES6 引入的另一个标志。

它为正则表达式打开 Unicode 模式。

在我们的 regex 中，大多数可以识别像`\u{094F}`这样的 Unicode 点转义序列。

此外，regex 模式和字符串中的字符也被识别为代码点。

代码单元被转换成代码点。

在非 Unicode 模式下，单个代理匹配代理对码位。

例如，如果我们有:

```
/\uD83D/.test("\uD83D\uDC2B")
```

我们让`true`回来了。

在 Unicode 模式下，这将返回`false`,因为这两个代码点应该放在一起以创建一个字符。

于是`/\uD83D/u.test(“\uD83D\uDC2B”)`返回`false`。

实际的单独代理仍然与`u`标志匹配:

```
/\uD83D/u.test("\uD83D \uD83D\uDC2B")
```

所以上面的表达式返回`true`。

这意味着在 Unicode 模式下，两个码位一起将被解释为一个正确的字符。

dor 运算符将匹配代码点，而不是代码单元。

例如:

```
'\uD83D\uDC2B'.match(/./gu).length
```

返回`.`

但是:

```
'\uD83D\uDC2B'.match(/./g).length
```

返回 2。

在 Unicode 模式下，量词适用于代码点。

但是在非 Unicode 模式下，它们适用于单个代码单元。

所以如果我们有:

```
/\uD83D\uDC2B{2}/u.test("\uD83D\uDC2B\uD83D\uDC2B")
```

这将返回`true`。

但是:

```
/\uD83D\uDC2B{2}/.test("\uD83D\uDC2B\uD83D\uDC2B")
```

返回`false`。

# `flags Property`

`flags`属性是 regex 对象的新属性。

它返回添加到正则表达式中的标志。

例如，如果我们有:

```
/foo/gy.flags
```

我们得到`“gy”`。

我们不能更改现有正则表达式的标志。

但是我们可以使用`flags`属性复制一个带有标志的正则表达式。

例如，我们可以写:

```
const regex = /foo/gy
const copy = new RegExp(regex.source, regex.flags);
```

`source`属性具有正则表达式模式。

并且`flags`具有标志。

# `RegExp Constructor`

正如我们所见，`RegExp`压缩函数对于复制正则表达式很有用。

它采用模式和标志。

# 委托给 Regex 方法的字符串方法

一些字符串实例方法委托给 regex 实例方法。

他们是:

*   `String.prototype.match`来电`RegExp.prototype[Symbol.match]`
*   `String.prototype.replace`来电`RegExp.prototype[Symbol.replace]`
*   `String.prototype.search`来电`RegExp.prototype[Symbol.search]`
*   `String.prototype.split`来电`RegExp.prototype[Symbol.split]`

# 异步编程

JavaScript 具有广泛的异步编程特性。

每个浏览器选项卡都在一个称为事件循环的进程中运行。

该循环运行通过任务队列提供的浏览器相关的东西。

任务包括解析 HTML、运行 JavaScript 代码、监听用户输入等等。

# 定时器

JavaScript 有`setTimeout`来延迟一段代码的执行。

在给定的延迟之后，需要一个回调来运行代码。

第一个参数是要运行的回调。

第二个是将回调添加到队列中的毫秒数。

一旦被调用，回调就被添加到任务队列中。

延迟指定了添加回调的时间，而不是实际运行的时间。

这可能以后会发生。

# DOM 更改

DOM 变化不会立即发生。

每 16 毫秒发生一次。

![](img/c922f3076ec1b0aaa9bf36aa679d7e0c.png)

Photo by [Agê Barros](https://unsplash.com/@agebarros?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

`u`标志允许我们启用 Unicode 模式，以便正确地搜索 Unicode 字符串。

让我们延迟运行代码。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**