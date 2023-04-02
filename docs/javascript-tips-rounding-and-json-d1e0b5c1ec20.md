# JavaScript 技巧—舍入和 JSON

> 原文：<https://javascript.plainenglish.io/javascript-tips-rounding-and-json-d1e0b5c1ec20?source=collection_archive---------2----------------------->

![](img/f67cd0112f4b7afe46d5cf8f66de6688.png)

Photo by [Elizeu Dias](https://unsplash.com/@elishavision?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将了解一些在编写 JavaScript 代码时应该遵循的技巧。

# 将数字舍入到 N 位小数

我们可以用`toFixed`方法来舍入数字。

它将舍入到的小数位数作为参数。

例如，我们可以写:

```
let num = 2.575949004696;
num = num.toFixed(3);
```

它返回一个带有舍入数字的字符串。

所以我们得到`“2.576”`作为`num`的新值。

# 浮点问题

我们应该意识到，添加浮点数可能不会得到我们预期的结果。

例如，如果我们有:

```
0.1 + 0.2 === 0.3
```

然后我们得到`false`。

这是因为浮点数是 64 位二进制数。

转换会给我们带来差异。

为了解决这个问题，我们使用了`toFixed`或者`toPrecision`方法。

# 使用 for-in 循环时检查对象的属性

用`hasOwnProperty`方法检查对象的属性。

例如，我们可以写:

```
for (let name in object) {
  if (object.hasOwnProperty(name)) {
    // ...
  }
}
```

我们用`hasOwnProperty`检查属性是否在对象本身，而不是它的原型。

# 逗点算符

逗号运算符总是返回列表中的最后一项。

所以如果我们有:

```
const a = (100, 99);
```

那么`a`就是 99。

# 需要计算或查询的缓存变量

为了加速我们的应用程序，我们应该尽可能缓存变量。

例如，我们写道:

```
const navRight = document.querySelector('#right'); 
const navLeft = document.querySelector('#left'); 
const navUp = document.querySelector('#up'); 
const navDown = document.querySelector('#down');
```

那么我们不会在任何地方使用变量，而是一直查询 DOM。

# 在将参数传递给 isFinite()之前，请对其进行验证

让我们检查一个表达式是否返回一个有限的数字。

例如，我们可以写:

```
isFinite("foo")
```

它返回`false`。

如果我们有:

```
isFinite(10)
```

它返回`true`。

# 用 JSON 进行序列化和反序列化

我们可以使用`JSON`对象来序列化和反序列化 JSON。

为了将对象转换成 JSON，我们使用了`JSON.stringify`。

为了将 JSON 字符串解析成对象，我们使用了`JSON.parse`。

我们可以通过写来使用`JSON.stringify`:

```
const str = JSON.stringify(obj)
```

它返回字符串化的 JSON 对象。

此外，我们可以传入额外的参数，使字符串更具可读性:

```
const str = JSON.stringify(obj, undefined, 2);
```

第三个参数包含要缩进的空格数。

为了将 JSON 字符串解析为对象，我们编写:

```
const obj = JSON.parse(str)
```

其中`str`是 JSON 字符串。

# 避免使用 eval()或函数构造函数

我们应该避免使用`eval`和`Function`构造函数，因为它们都从字符串中运行代码。

这是一个安全风险，因为我们可以向它传递恶意的字符串。

字符串中的代码无法优化，也很难调试。

# 避免使用 with()

不应该使用，因为范围会引起混淆。

变量可以在全局范围内插入，这也不好。

如果两个变量在一个`with`语句中有相同的名字，它可以覆盖这个值。

![](img/1b02e0b7aa17587ea778d17c91e366ba.png)

Photo by [Khachik Simonian](https://unsplash.com/@khachiksimonian?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

JavaScript 中有些东西我们应该避免。

我们可以用标准库来舍入数字和操纵 JSON。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**