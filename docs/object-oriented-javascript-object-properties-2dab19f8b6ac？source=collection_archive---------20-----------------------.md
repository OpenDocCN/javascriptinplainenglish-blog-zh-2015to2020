# 面向对象的 JavaScript —对象属性

> 原文：<https://javascript.plainenglish.io/object-oriented-javascript-object-properties-2dab19f8b6ac?source=collection_archive---------20----------------------->

![](img/36c8b60f25cf2937f88145c7ab878b45.png)

Photo by [Tierra Mallorca](https://unsplash.com/@tierramallorca?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将研究对象属性。

# 对象属性和特性

对象属性有自己的属性。

它们包括可枚举和可配置的属性。

他们都是布尔人。

可枚举意味着我们是否可以枚举属性。

可配置意味着如果属性为`false`，则不能删除或更改任何属性。

我们可以通过编写来使用`Object.getOwnPropertyDescriptor`方法:

```
let obj = {
  name: 'james'
}
console.log(Object.getOwnPropertyDescriptor(obj, 'name'));
```

我们得到了:

```
{value: "james", writable: true, enumerable: true, configurable: true}
```

从控制台日志中。

# 对象方法

ES6 带有各种对象方法。

# 使用 Object.assign 复制属性

我们可以使用`Object.assign`方法来复制属性。

例如，我们可以写:

```
let a = {}
Object.assign(a, {
  age: 25
})
```

那么`a`就是:

```
{age: 25}
```

我们将`age`属性复制到`a`对象，这就是我们得到的结果。

`Object.assign`可以带多个源对象。

例如，我们可以写:

```
let a = {}
Object.assign(a, {
  a: 2
}, {
  c: 4
}, {
  b: 5
})
```

那么`a`就是:

```
{a: 2, c: 4, b: 5}
```

将复制每个对象的所有属性。

如果有任何冲突:

```
let a = {
  a: 1,
  b: 2
}
Object.assign(a, {
  a: 2
}, {
  c: 4
}, {
  b: 5
})console.log(a)
```

然后后面的人会优先考虑。

所以`a`也是一样。

# 将值与 Object.is 进行比较

我们可以用`Object.is`来比较数值。

和`===`差不多，除了`NaN`等于自己。

而且`+0`和`-0`不一样。

例如，如果我们有:

```
console.log(NaN === NaN) 
console.log(-0 === +0)
```

然后我们得到:

```
false
true
```

如果我们有:

```
console.log(Object.is(NaN, NaN))
console.log(Object.is(-0, +0))
```

我们得到:

```
true
false
```

# 解构

析构让我们将对象属性分解成变量。

例如，不写:

```
const config = {
  server: 'localhost',
  port: '8080'
}
const server = config.server;
const port = config.port;
```

我们可以写:

```
const config = {
  server: 'localhost',
  port: '8080'
}
const {
  server,
  port
} = config
```

它比第一个例子短得多。

析构也适用于数组。

例如，我们可以写:

```
const arr = ['a', 'b'];
const [a, b] = arr;
```

那么`a`就是`'a'`而`b`就是`'b'`。

它对于交换变量值也很方便。

例如，我们可以写:

```
let a = 1,
  b = 2;
[b, a] = [a, b];
```

那么`b`为 1`a`为 2。

# 内置对象

JavaScript 附带了各种构造函数。

包括`Object`、`Array`、`Boolean`、`Function`、`Number`、`String`等。

这些可以创建各种各样的对象。

实用对象包括`Math`、`Date`和`RegExp`。

可以用`Error`构造函数创建错误对象。

# 目标

`Object`构造函数可以用来创建对象。

所以这些是等价的:

```
const o = {}; 
const o = new Object();
```

第二个更长。

它们都从`Object.prototype`继承而来，后者有各种内置属性。

例如，有一个将对象转换成字符串的`toString`方法。

还有返回对象表示的`valueOf`方法。

对于简单对象，`valueOf`只是返回对象。所以:

```
console.log(o.valueOf() === o)
```

我们得到了`true`。

![](img/aa956d83c46f63362e2077eef68afa41.png)

Photo by [Alex D'Alessio](https://unsplash.com/@alexdalessio22?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

对象有不同的属性和方法。

它们继承了各种方法，让我们转换成不同的形式。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**