# JavaScript 最佳实践——子字符串、修整、生成器和生命

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-substrings-trimming-generators-and-aae4b3c5edd7?source=collection_archive---------9----------------------->

![](img/b2680d9ac0b58144ba759d21202af4d5.png)

Photo by [Jessica Knowlden](https://unsplash.com/@mybibimbaplife?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了使代码易于阅读和维护，我们应该遵循一些最佳实践。

在这篇文章中，我们将看看我们应该遵循的一些最佳实践，以使每个人的生活更轻松。

# 用`String#slice()`代替`String#substr()`和`String#substring()`

`slice`方法可以做`substr`或`substring`方法做的事情，除了它更短。

它也更符合数组。

例如，不写:

```
foo.substr(start, end);
foo.substring(start, end);
```

我们写道:

```
foo.slice(start, end);
```

# `Use .textContent`超过`.innerText`

`textContent`比`innerText`更快，也更容易预测，所以我们应该用它来代替。

`textContent`也可以有 CDATA 和处理指令。

它还将连接所有节点的`textContent`。

所以与其写:

```
foo.innerText = 'foo';
```

我们写道:

```
foo.textContent = 'foo';
```

# `Use String#trimStart()` / `String#trimEnd()`代替`String#trimLeft()` / `String#trimRight()`

`trimStart`和`trimEnd`更符合其他方法如`padStart`或`padEnd`。

从字符串的开头开始修剪空白。

而`trimEnd`从字符串末尾开始修剪空白。

所以我们应该使用`trimStart`或`trimEnd`

例如，不写:

```
str.trimLeft();
str.trimRight();
```

我们写道:

```
str.trimStart();
str.trimEnd();
```

# 投入`TypeError`进行型式检验

如果我们可能得到无效数据类型的数据，那么我们可能希望抛出类型错误，使它们变得非常明显。

例如，不写:

```
if (!Array.isArray(bar)) {
  throw new Error('not an array');
}
```

我们写道:

```
if (!Array.isArray(bar)) {
  throw new TypeError('not an array');
}
```

`TypeError`是数据类型不匹配时专门抛出的。

# 没有缩写

如果全名不太长，那么我们不应该缩写它们。

例如，代替写作:

```
class Btn {}
```

我们写道:

```
class Button {}
```

而不是写:

```
const err = new Error();
```

我们写道:

```
const err = new Error();
```

# `Use new`抛出错误时

`Error`是一个构造函数，所以我们应该用`new`来调用它。

例如，不写:

```
throw Error();
```

我们写道:

```
throw new Error();
```

# 使用`isNaN()`检查`NaN`

`isNaN`是一个专门用来检查`NaN`的函数。

我们需要这个，因为我们不能使用`===`或`!==`来检查它。

例如:

```
if (bar === NaN) {
  // ...
}
```

行不通，因为`NaN`不等于它自己。

相反，我们写道:

```
if (isNaN(bar)) {
  // ...
}
```

做正确的比较。

其他比较操作符也不能像我们期望的那样使用`NaN`。

# 将`typeof`表达式与有效字符串进行比较

`typeof`操作符返回一个带有数据类型名称的字符串，所以我们应该与字符串进行比较。

例如，不要写像这样的东西

```
typeof bar === "strnig"
typeof bar === "undefibed"
```

我们写道:

```
typeof bar === "string"
typeof bar === "undefined"
```

# 变量声明位于其作用域的顶部

我们可以把变量声明放在它们作用域的顶部，这样我们就可以更容易地找到它们。

例如，不写:

```
function doWork() {
  var foo;
  if (true) {
    foo = 'abc';
  }
  var bar;
}
```

我们写道:

```
function doWork() {
  var foo;
  var bar;
  if (true) {
    foo = 'abc';
  }
}
```

这样我们就可以在一个地方找到它们。

# 生活应该被包装

我们需要包装 IIFEs，让 JavaScript 解释器将它解析为表达式而不是声明。

例如，不写:

```
let z = function() {
  return {
    y: 'abc'
  };
}();
```

我们写道:

```
let z = (function() {
  return {
    y: 'abc'
  };
})();
```

第二个示例将被声明为表达式。

# 正则表达式应该换行

我们可以包装正则表达式文字，使它们更容易阅读。

例如，不写:

```
/baz/.test("bar");
```

我们写道:

```
(/baz/).test("bar");
```

# `yield*`表达式中`*`周围的间距

当我们在另一个生成器函数中调用生成器函数时，我们通常不在`yield`和`*`之间加空格。

所以与其写:

```
function* generator() {
  yield * foo();
}
```

我们写道:

```
function* generator() {
  yield* foo();
}
```

![](img/f47c66cd1aa29252454ffc8d33caa598.png)

Photo by [Boris YUE](https://unsplash.com/@ybs9641?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该使用`slice`而不是`substr`或`substring`。

此外，我们应该使用`trimStart`和`trimEnd`，以便与`padStart`或`padEnd`保持一致。

当我们遇到我们不期望的类型时，我们应该抛出`TypeError` s，而不是一般错误。

# 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**