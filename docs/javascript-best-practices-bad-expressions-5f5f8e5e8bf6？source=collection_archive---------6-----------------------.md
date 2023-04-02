# JavaScript 最佳实践——糟糕的表达式

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-bad-expressions-5f5f8e5e8bf6?source=collection_archive---------6----------------------->

![](img/9c30de25f78aa74ffdf833e2190fcb8e.png)

Photo by [Robert Bye](https://unsplash.com/@robertbye?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究一些在编写 JavaScript 代码时应该遵循的最佳实践。

# 扩展运算符和它们的表达式之间没有空格

我们不应该在扩展操作符和它们的表达式之间有任何空白。

例如，不写:

```
fn(... args)
```

我们写道:

```
fn(...args)
```

# 分号后面必须有一个空格，前面不能有空格

分号后面应该有一个空格，前面没有空格，这是 JavaScript 中普遍接受的。

例如，不写:

```
for (let i = 0 ;i < arr.length ;i++) {...}
```

我们写道:

```
for (let i = 0; i < arr.length; i++) {...}
```

# 块之间有一个空格字符

如果我们正在创建一个块，那么我们应该在它前面有一个空格字符。

例如，不写:

```
if (cond){...}
```

我们写道:

```
if (cond) {...}
```

# 括号内没有空格

括号里不应该有空格。

例如，不写:

```
getName( person )
```

我们写道:

```
getName(person)
```

# 一元运算符后面必须有空格

我们应该在一元操作符后面有一个空格，这样我们就可以阅读它们了。

还有，我们不想把单词连在一起。

例如，不写:

```
typeof!cond
```

我们写道:

```
typeof !cond
```

# 在注释中使用空格

JavaScript 注释中应该有空格。

例如，不写:

```
//comment
```

我们写道:

```
// comment
```

对于多行注释，我们不应该写:

```
/*comment*/
```

相反，我们写道:

```
/* comment */
```

# 模板字符串中没有空格

在模板字符串中，表达式的开头和结尾不应该有空格。

例如，不写:

```
const message = `hi, ${ name }`
```

我们写道:

```
const message = `hi, ${name}`
```

# 要检查 NaN，我们应该使用 isNaN

我们不能用`===`来比较`NaN`,因为`NaN`并不被认为与它本身相同。

相反，我们需要使用`isNaN`函数来检查`NaN`。

所以与其写作；

```
if (price === NaN) { }
```

我们写道:

```
if (isNaN(price)) { }
```

# typeof 必须与字符串进行比较

`typeof`必须与有效字符串进行比较。

确保字符串中没有错别字。

例如，我们不应该写:

```
typeof name === 'undefimed'
```

当我们实际上指的是:

```
typeof name === 'undefined'
```

# 必须包装立即调用的函数表达式(IIFEs)

我们应该用圆括号把函数括起来，这样我们就清楚了，我们在函数定义后就立即调用它。

例如，我们应该写:

```
const getName = (function () { }())
```

或者:

```
const getName = (function () { })()
```

或者:

```
const getName = (() => { })()
```

# 产出表达式前后必须有一个空格

当我们从一个生成器函数调用另一个生成器函数时，我们需要`yield *`关键字。

为了与 JavaScript 约定保持一致，我们应该这样写。

例如，我们写道:

```
yield * foo();
```

# 没有尤达表情

我们应该将正在比较的值作为正确的运算符，这样我们可以更容易地比较它们。

这更像是正常的人类语言

例如，我们写道:

```
if (age === 2) { }
```

而不是:

```
if (2 === age) { }
```

# 分号

我们应该有分号，以避免在意想不到的地方自动插入分号。

例如，我们写道:

```
window.alert('hi');
```

# 不要用我们不希望开始一行的字符开始新的一行

我们不应该有像圆括号，方括号，分号，逗号等字符。即使它们是有效的。

例如，我们不应该编写这样的代码:

```
;(function () {
  window.alert('ok')
}())
```

相反，我们写道:

```
(function () {
  window.alert('ok')
}())
```

![](img/e52429e8f3c5ff5afe23a2d1a50c9c07.png)

Photo by [Victor Malyushev](https://unsplash.com/@malyushev?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们不应该写大多数人不期望的代码。

此外，我们应该编写像程序员一样自然阅读的代码。

我们也不应该在代码中有无用的空白。

## **说白了**

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**