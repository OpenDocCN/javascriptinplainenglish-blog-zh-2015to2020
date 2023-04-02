# JavaScript 能做到吗？(速记技巧)

> 原文：<https://javascript.plainenglish.io/javascript-can-do-that-ca59c00738cd?source=collection_archive---------1----------------------->

## 用 JavaScript 速记技术做更多的事情。

![](img/fb558bf7daaaeefbf629d00fee2cbd84.png)

Image by [Alltechbuzz](https://pixabay.com/users/Alltechbuzz-13671689/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4523100) from [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4523100)

JavaScript 是最流行的编程语言之一，有时了解 JavaScript 中的速记技术可以改进您的代码，做到事半功倍。在这篇文章中，我将谈论一些速记技巧。

*这些技术可能会让你的代码看起来整洁，但它可能会损害一些用户的可读性，特别是初级开发人员可能很难理解发生了什么，所以明智地使用它们*，[亲亲](https://en.wikipedia.org/wiki/KISS_principle)。

所以让我们开始吧。

# 将值转换为布尔值

## 双非(！！)

这种转换是基于值的“[](https://developer.mozilla.org/en-US/docs/Glossary/truthy)**”或“[](https://developer.mozilla.org/en-US/docs/Glossary/falsy)**”的真值。****

## ****布尔函数****

****这很像上面显示的 **DoubleNot** 操作符。但是我觉得更干净，更容易理解。跟**一样快(！！)**。****

# ****将数字字符串和 null 转换为数字****

****最简单的方法是在字符串前面添加**一元运算符** `+`。如果格式正确，`string`将被转换成`integer`。这不是一个广为人知的技巧，也许是因为它会让读者感到困惑。****

******数字()👍🏽**可读性最强，更容易理解。也可以用( ***1)** 但是，我感觉就是有点乱。****

# ****将值转换为字符串****

****进行字符串转换最流行的方法是使用`toString()`，但是现在我们可以使用模板文字。****

# ****串联****

****我们可以使用模板文字进行连接。它使您能够创建多行字符串和表达式插值。****

# ****使用 Spread 运算符的数组和对象****

## ****复制/克隆阵列****

****当你想要复制一个数组或一个对象时，你可以使用 [spread 运算符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax) (…)。这创建了一个数组/对象的浅拷贝，就像`Array.prototype.slice() / Object.assign()`。然而，当处理多维数组时，不推荐这样做。****

## ****复制/克隆对象****

## ****将数组连接在一起****

## ****删除数组中的重复项****

****`Set`是 ES6 中引入的[新数据对象。set 对象允许您存储任何类型的唯一值。我从](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)[萨曼莎·明](https://medium.com/u/829a804ea5da?source=post_page-----ca59c00738cd--------------------------------)的[博客](https://www.samanthaming.com/tidbits/43-3-ways-to-remove-array-duplicates)上读到了`Set`。我觉得它真的很棒😀。****

# ****IndexOf()或 includes()****

****当在一个数组中寻找一个项目时，我们使用`indexOf()`来获得我们所寻找的那个项目的位置。如果该值是一个数字(+或-)，而不是一个`0`，那么它被认为是**真值。**我们也可以使用 **includes()，**来适当地返回一个布尔值。****

****还有`~` ( **波浪号**[**til**-*duh*)可以和`indexOf()`一起使用的按位运算符，不过为了简单起见我宁愿用`includes()`。除了`-1`之外，按位运算符将返回任何值的真值。****

# ****如果(👽)条件****

## ****真实性检查****

****我在代码中多次看到下面的 truthy check。****

****在 JavaScript 中，如果值不是`undefined, null, false, 0, "", NaN` [，那么它将计算为 the。](https://www.ecma-international.org/ecma-262/5.1/#sec-9.2) 因此，你可以简单地使用变量本身作为条件，而不是像上面的例子那样。****

****进行真实性检查的一种简便方法:****

# ****实验性和有限的可用性****

****在工作中，我几乎一直都在使用 TypeScript，关于 TypeScript 的最好的事情之一是你现在就可以使用 ECMAScript 即将到来的特性。最近发布的 [TypeScript v3.7](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-7.html) 允许您使用[可选链接](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining)和[无效合并](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining)。****

## ****可选链接****

****这允许我们安全地读取一个属性的值，这个值在一个对象的深处。我们不再需要明确检查对象链的每个属性是否有效。这使得我们的代码更短、更简单、更容易理解。****

```
**// What we current use:
let y = (foo === null || foo === undefined) ? undefined : foo.bar;**
```

****不用做上面的，你可以简单的做这个:`let y = foo?.bar;`，看看有多简单优雅🙂。****

****由于可选的链接，表达式将只返回`undefined`。而不是我们都熟悉的例外`TypeError: Cannot read property 'bar' of undefined`。****

## ****无效合并****

****在 JavaScript 中，对于 nullish 合并，我们使用逻辑**或||** 操作符。但是这伴随着一些意想不到的行为。****

```
**const foo = bar.baz || "qux";**
```

******或(||)** 是一个布尔运算符，如果`bar.baz`定义了一个值并且是`0`，那么`foo`将是`"qux",`，因为`0`是一个伪值。这是因为左边的值被强制转换为布尔值进行评估，并且*任何假值都不会返回*。这就是 nullish 合并运算符派上用场的地方。****

```
**const foo = bar.baz ?? "qux";**
```

****现在，如果`bar.baz`的值是`0`，那么`foo`将是`0`。如果`bar.baz`是`undefined`或`null`，那么`foo`将是`"qux"`。无效合并运算符和可选链接运算符都将`undefined`和`null`视为特定值。****

****这些是我认为值得分享的一些技巧，还有很多；我们改天再看。****

## ****感谢阅读😀****