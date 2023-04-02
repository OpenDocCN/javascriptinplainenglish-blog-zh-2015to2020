# 面向对象的 JavaScript —布尔型、数字型和字符串型

> 原文：<https://javascript.plainenglish.io/object-oriented-javascript-boolean-number-and-string-a21aca5493d0?source=collection_archive---------7----------------------->

![](img/547a924f024d04af40cab293671976d4.png)

Photo by [Rodion Kutsaev](https://unsplash.com/@frostroomhead?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将研究原始包装器。

# 布尔代数学体系的

`Boolean`可以用作工厂函数或构造函数。

例如，我们可以写:

```
const b = new Boolean();
```

我们得到一个布尔包装对象。

我们可以用`valueOf`将其转换回原始值:

```
b.valueOf();
```

更好的使用`Boolean`函数的方法是将其作为工厂函数使用。

例如，我们可以写:

```
Boolean("foo");
```

然后我们得到`true`。

如果为真，它将返回`true`，否则返回`false`。

`Boolean`不带`new`返回一个原始值。

# 数字

`Number`也可用作构造函数或工厂函数。

例如，如果我们有:

```
const n = Number('12');
```

然后我们返回数字 12。

如果我们有:

```
const n = new Number('12');
```

然后我们返回一个值为 12 的对象。

所以要不用`new`来使用，以消除混淆。

`Number`也有一些静态属性。

`Number.MAX_VALUE`就是`1.7976931348623157e+308`。

还有就是`Number.POSITIVE_INFINITY`也就是`Infinity`。

而`Number.NEGATIVE_INFINITY`也就是`-Infinity`。

`toFixed`方法将一个数字格式化为具有给定小数位数的十进制数。

例如，我们可以写:

```
(123.456).toFixed(1);
```

我们有`“123.5”`。

`toExponential`方法返回指数记数法中的数字字符串。

例如，如果我们有`(12345).toExponential()`，那么我们得到:

```
"1.2345e+4"
```

`toString`方法让我们将数字转换成不同基数的数字串。

例如，我们可以写:

```
(255).toString(2)
```

然后我们将数字转换成二进制字符串:

```
"11111111"
```

我们可以写:

```
(255).toString(16)
```

并获得:

```
"ff"
```

要将其转换为十进制，我们写:

```
(255).toString(10)
```

我们得到了:

```
"255"
```

# 线

`String`函数用于创建一个字符串。

我们可以将它用作构造函数或工厂函数。

我们不想用它创建一个对象来减少混乱，所以我们应该把它作为一个工厂函数来使用。

例如，我们可以写:

```
const obj = new String('foo');
```

然后我们创建一个字符串包装对象。

要将其转换为原始值，我们调用`valueOf`:

```
const s = obj.valueOf();
```

最好只是创建一个字符串作为字符串文字:

```
const s = 'foo';
```

我们还可以使用函数将非字符串转换为字符串:

```
const s = String(123);
```

我们可以用`length`属性得到一个字符串中的字符数。

所以如果我们有:

```
const s = 'foo';
```

那么`s.length`就是 3。

如果我们向`String`传递一个对象，那么`toString`方法将被调用。

所以:

```
String({
  a: 1
});
```

退货:

```
"[object Object]"
```

并且:

```
String([1, 2, 3]);
```

退货:

```
"1,2,3"
```

![](img/10196f64559fdb1aa22cff670df59005.png)

Photo by [Anton Darius](https://unsplash.com/@thesollers?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

`Boolean`、`Number`和`String`函数让我们创建原始值。

我们应该将它们用作工厂函数，在不同的原始类型之间进行转换。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**