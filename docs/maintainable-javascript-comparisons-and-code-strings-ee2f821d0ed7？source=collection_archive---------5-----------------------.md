# 可维护的 JavaScript —比较和代码字符串

> 原文：<https://javascript.plainenglish.io/maintainable-javascript-comparisons-and-code-strings-ee2f821d0ed7?source=collection_archive---------5----------------------->

![](img/e91a3c7161243b81fff85aad4d7253c5.png)

Photo by [Rajiv Perera](https://unsplash.com/@rajivperera?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果想继续使用代码，创建可维护的 JavaScript 代码很重要。

在本文中，我们将通过比较和`eval`来了解创建可维护的 JavaScript 代码的基础。

# 布尔比较

与`==`和`!=`的布尔比较也会做一些我们意想不到的事情。

这是因为在比较完成之前，任何操作数都被转换为`true`或`false`。

例如，如果我们有:

```
console.log(1 == true);
```

然后我们得到`true`，因为 1 被转换为`true`。

另一方面，如果我们有:

```
console.log(0 == false);
```

我们也 long `true`，因为 0 是 falsy，它被转换为`false`。

# 对象比较

如果对象被比较，就用`valueOf`方法将其转换成原始值。

如果`valueOf`不存在，则调用`toString`。

然后比较继续进行，就像其他类型的比较一样。

因为如果我们有:

```
const object = {
  toString() {
    return "0xFF";
  }
};
console.log(object == 255);
```

然后控制台日志记录`true`，因为`object`用`toString`方法转换成字符串。

然后`Number`函数将其转换成十进制数。

# 空值和未定义的值

此外，类型强制发生在`null`和`undefined`之间。

它们被`==`操作符认为是等价的。

例如，如果我们有:

```
console.log(null == undefined);
```

然后那个日志`true`。

# ===而且！==

类型强制是用`==`和`!=`完成的，所以我们不应该用它们来做比较。

强制规则太复杂了。

相反，我们应该使用`===`和`!==`操作符，它们不做任何类型强制。

所以如果我们有:

```
console.log(5 === "5");
```

我们记录`false`。

并且:

```
console.log(25 === "0x19");
```

也记录了`false`。

如果我们有:

```
const object = {
  toString() {
    return "0xFF";
  }
};
console.log(object == 255);
```

这也记录了`false`,因为没有进行类型强制。

使用`===`和`!==`是道格·克罗克福德的风格指南、谷歌风格指南等推荐的。

这几乎是所有风格指南之间的普遍推荐。

像 ESLint 和 JSHint 这样的 Linters 都警告我们使用了`==`和`!=`操作符。

# eval()

`eval`是一个获取一串 JavaScript 代码并运行它的函数。

例如，我们可以写:

```
eval("console.log('hello')");
```

在控制台日志中记录`'hello'`。

`eval`函数不是唯一一个从字符串运行 JavaScript 代码的函数。

`setTimeout`和`setInterval`函数也接受一个函数并运行该代码。

`Function`也接受带有参数和代码的字符串，并返回一个函数。

从 JavaScript 字符串运行代码被普遍认为是一种不好的做法，因为运行可能从任何来源运行的代码会带来安全问题。

此外，字符串中的 JavaScript 代码无法通过 JavaScript 引擎进行性能优化。

因此，我们不应该使用`eval`函数或`Function`构造函数。

同样，我们应该在实际代码中使用`setTimeout`和`setInterval`，而不是字符串。

例如，我们通过写来恰当地使用它:

```
setTimeout(() => {
  document.body.style.background = 'red';
}, 50);setInterval(() => {
  document.title = `the tiome is ${new Date()}`;
}, 1000);
```

我们在回调中运行代码，而不是从字符串中运行代码。

![](img/5f7c1e62b9c7b4aee097d515d97d8a0a.png)

Photo by [Richard Bagan](https://unsplash.com/@richard_bagan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们要用`===`和`!==`进行比较。

此外，我们不应该从字符串中运行代码。

## **简单英语的 JavaScript**

你知道我们有三份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)找到所有内容的链接！