# JavaScript 的优点——来自标准库的方法

> 原文：<https://javascript.plainenglish.io/good-parts-of-javascript-methods-from-the-standard-library-3d3991058cfd?source=collection_archive---------8----------------------->

![](img/19ff788cc5b0f6a2b741554cff73554e.png)

Photo by [Gabriel Sollmann](https://unsplash.com/@gabons?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是世界上最流行的编程语言之一。它可以做很多事情，并且有一些领先于许多其他语言的功能。

在本文中，我们将看看 JavaScript 标准库中一些有用的数组方法。

# 排列

JavaScript 标准库有许多有用的数组方法。

## array.unshift(item…)

`unshift`将项目添加到数组的开头。

它返回数组的新长度。

例如，我们可以写:

```
const a = ['a', 'b', 'c'];
const b = a.unshift('d');
```

那么`a`就变成了`[“d”, “a”, “b”, “c”]``b`就是 4。

# 功能

如果我们必须使用传统函数，那么这些方法会对我们有所帮助。

## function.apply(thisArg，argArray)

我们可以使用`apply`调用一个传统的函数，它的值`this`与函数内部的参数数组不同。

例如，我们可以写:

```
function foo(a, b) {
  console.log(`${this.value} ${a} ${b}`);
}foo.apply({
  value: 'foo'
}, ['bar', 'baz']);
```

然后我们得到:

```
foo bar baz
```

从控制台日志输出，因为`this`是:

```
{
  value: 'foo'
}
```

`a`是`'bar'`，`b`是`'baz'`。

# 数字

当我们处理数字时，各种数字方法也使我们的生活变得更容易。

## 数字到指数(小数位数)

`toExponential`方法让我们将数字转换成小数点后有小数位数的指数记数法。

返回值是一个字符串。

例如，我们可以写:

```
console.log((Math.PI.toExponential(2)));
```

我们得到:

```
3.14e+0
```

从控制台日志输出。

## number.toFixed(小数位数)

`toFixed`方法以十进制形式返回一个数字的字符串表示。

它会有我们指定的小数点后的位数。

例如，我们可以写:

```
console.log(Math.PI.toFixed(5));
```

我们得到了:

```
3.14159
```

从控制台日志中。

## number . top precision(精度)

`toPrecision`方法将一个数字转换成十进制形式的字符串。

`precision`参数是可选的，控制精度的位数。

例如，我们可以写:

```
console.log(Math.PI.toPrecision(5));
```

并获得:

```
3.1416
```

## 数字字符串(基数)

`toString`将`number`转换为字符串。

我们可以指定可选的`radix`参数来控制数字的基数。

`radix`应该在 2 到 36 之间。默认`radix`为 10。

例如，我们可以写:

```
console.log(Math.PI.toString(8));
```

将`Math.PI`转换为基数 8。我们得到:

```
3.1103755242102643
```

# 目标

`Object.prototype`有一些我们可以使用的有用方法。

## object.hasOwnProperty(名称)

让我们检查一个对象的属性是继承的还是自己的。

如果`name`是自有财产，则返回`true`。

例如，我们可以写:

```
const obj = {
  foo: 'bar'
};
const child = Object.create(obj);
console.log(child.hasOwnProperty('foo'));
```

然后我们得到了`false`，因为`foo`是`child`的继承属性。

# 正则表达式

`RegExp`构造函数还附带了一些匹配模式的有用方法。

## regexp.exec(字符串)

`exec`方法在给定的字符串中查找正则表达式模式的所有匹配。

例如，我们可以写:

```
console.log(/foo/g.exec('foo foo bar'));
```

然后我们在迭代器中得到匹配。

## regexp.test(字符串)

`test`方法用于检查一个字符串是否匹配给定的正则表达式。

例如，我们可以写:

```
console.log(/foo/g.test('foo foo bar'));
```

并记录`true`，因为`'foo'`在`‘foo foo bar’`中。

# 线

各种字符串方法对于操作字符串也非常有用。

## string.charAt(位置)

`charAt`方法返回给定位置的字符。

例如，我们可以写:

```
const name = 'joe';
const initial = name.charAt(0);
```

并得到`'j'`作为`initial`的值。

## string.charCodeAt(pos)

`charCodeAt`与`charAt`类似，只是返回的是字符码位的整数表示，而不是字符本身。

例如，我们可以写:

```
const name = 'joe';
const code = name.charCodeAt(0);
```

然后我们得到字符`'j'`的代码 106。

如果`pos`小于 0 或大于等于`string.length`，则返回`NaN`。

![](img/4f3df43dbdb46a97f9446bfe9dd98f02.png)

Photo by [Paul Melki](https://unsplash.com/@paulmelki?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## string.concat(string…)

我们可以使用`concat`方法将一个或多个字符串连接在一起。

例如，我们可以写:

```
console.log('foo'.concat('bar', 'baz'));
```

并从控制台日志输出中获取`foobarbaz`。

# 结论

我们不能忽略许多数组、字符串和正则表达式方法。

它们非常有用，让我们可以轻松处理任何类型的数据。

用`concat`组合字符串很有用。

我们可以使用 regex `test`方法来检查一个字符串是否匹配 regex 模式，并做更多的事情。

## **说白了就是**

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**