# 更好的 JavaScript——原语

> 原文：<https://javascript.plainenglish.io/better-javascript-primitives-d05e41a5dec4?source=collection_archive---------8----------------------->

![](img/f29c281ffed1052cd8f48bcfb369e0b6.png)

Photo by [Virginia Choy](https://unsplash.com/@veechoy?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究改进 JavaScript 代码的方法。

# 将对象转换为图元

可以用`valueOf`方法将对象转换成数字。

例如，我们可以写:

```
const obj = {
  valueOf() {
    return 3;
  }
};console.log(2 + obj);
```

然后我们看到 5 个日志。

我们可以这样把一个对象转换成一个字符串:

```
const obj = {
  toString() {
    return 'bar';
  }
};console.log('foo' + obj);
```

然后我们得到:

```
'foobar'
```

记录在案。

如果一个对象同时有`valueOf`和`toString`方法，那么不清楚哪个会被运行。

所以如果我们有:

```
const obj = {
  toString() {
    return "[object obj]";
  },
  valueOf() {
    return 'bar';
  }
};console.log('foo' + obj);
```

然后我们看到`'foobar'`被记录。

所以我们知道，如果`valueOf`和`toString`都存在，那么`valueOf`将被运行。

`valueOf`设计用于表示数值的对象，如`Number`对象。

对于这些对象，`toString`和`valueOf`是一致的。

它们都返回数字的字符串表示形式。

对字符串的强制远比对数字的强制更常见。

最好避免`valueOf`，除非对象是数字的抽象。

另一种胁迫被称为真实。

`||`和`&&`使用布尔值，但是它们可以接受任何东西。

如果一个值是真的，那么它们会被强制执行`true`。

否则，他们就是假的，他们被迫`false`。

`false`，0，-0，“”，`NaN`，`null`，`undefined`为假。

其他价值观都是真理。

如果第一个操作数是 falsy，那么`||`操作符让我们返回一个默认值。

例如，不写:

```
function point(x, y) {
  if (!x) {
    x = 10;
  }
  if (!y) {
    y = 20;
  }
  return {
    x,
    y
  };
}
```

或者:

```
function point(x, y) {
  if (typeof x === "undefined") {
    x = 10;
  }
  if (typeof y === "undefined") {
    y = 20;
  } return {
    x,
    y
  };
}
```

我们可以写:

```
function point(x, y) {
  x = x || 10;
  y = y || 20;
  return {
    x,
    y
  };
}
```

`||`操作符接受所有的 falsy 值，如果第一个操作数是 falsy，则返回第二个操作数。

# 原语比对象包装要好

原语比对象包装要好。

JavaScript 有各种原始值。

它们包括布尔值、数字、字符串、`null`和`undefined`。

`null`用`typeof`运算符报告为`'object'`，但在 ES 标准中被定义为一个单独的类型。

JavaScript 标准库也为这些对象提供了构造函数。

所以，我们可以写:

```
const s = new String("hello");
```

或者:

```
const str = "hello";
```

我们可以用方括号提取一个字符。

例如，我们可以写:

```
s[4]
```

我们得到了`'o'`。

但是如果有:

```
typeof s
```

它返回`'object'`。

但是如果我们有:

```
typeof str
```

然后我们得到`'string'`。

如果我们有:

```
const s1 = new String("foo");
const s2 = new String("foo");
```

如果我们写下:

```
s1 === s2
```

由于引用不同，它返回`false`。

它只能等于它自己。

正如我们所看到的，这是字符串包装器对象和其他原始包装器的问题。

![](img/dfaa484e7d1e614af59059763d99b11f.png)

Photo by [nima hatami](https://unsplash.com/@thegreatnima?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们不应该使用原始的包装对象。

它们在我们的代码中没有用。

![](img/787be6c671be8d345dc786dad8729ce5.png)

[Subscribe to Decoded, our official YouTube channel!](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)