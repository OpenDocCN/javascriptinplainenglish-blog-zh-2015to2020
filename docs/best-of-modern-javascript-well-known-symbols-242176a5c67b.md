# 现代 JavaScript 的精华——众所周知的符号

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-well-known-symbols-242176a5c67b?source=collection_archive---------16----------------------->

![](img/433f8957ee7be851fbd2ccb46236c268.png)

Photo by [Waldemar Brandt](https://unsplash.com/@waldemarbrandt67w?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 中新的 OOP 特性。

# `How to use __proto__?`

为了获得对象的原型，我们使用`Object.getPrototypeOf`。

要创建一个具有给定原型的对象，我们可以使用`Object.create`。

被弃用，它阻止了许多浏览器的优化。

`__proto__`可以用来获取和设置一个对象的原型。

# 可数性

for-in 循环遍历自己的和继承的可枚举属性的字符串键。

`Object.keys`返回一个可枚举自身属性的字符串键。

只有字符串化的可枚举对象拥有属性和字符串键。

在 ES6 中，`Object.assign`只复制可枚举的自身字符串和符号属性。

JavaScript 对象中有许多不可枚举的属性。

内置类的所有属性都是不可枚举的。

例如，如果我们有:

```
const desc = Object.getOwnPropertyDescriptor.bind(Array);
console.log(desc(Object.prototype, 'toString').enumerable)
```

我们在`Array.prototype.toString`方法中获得属性描述符。

我们得到了`enumerable`属性，它将记录`false`。

# 将属性标记为不可复制

如果我们将一个属性标记为可枚举的，我们可以将它标记为不可复制。

例如，我们可以写:

```
const obj = Object.defineProperty({}, 'foo', {
  value: 'bar',
  enumerable: false
});
```

我们将`enumerable`属性设置为`false`，这样它就不会被`Object.assign`或扩展操作符拾取。

# `**Object.assign()**`

`Object.assign`可用于将对象源合并到目标中。

源中所有自己的可枚举属性都将被复制到目标中。

它不考虑继承的属性。

# 对`JSON.stringify()`隐藏自己的属性

对`JSON.stringify`隐藏自己的属性可以通过使属性不可枚举来实现。

这将使`JSON.stringify`跳过属性。

我们也可以指定`toJSON`方法来返回我们想要 stringify 的对象。

例如，我们可以写:

```
const obj = {
  foo: 'bar',
  toJSON() {
    return {
      bar: 'baz'
    };
  },
};console.log(JSON.stringify(obj))
```

然后控制台日志会记录`{“bar”:”baz”}`。

# 通过众所周知的符号定制基本语言操作

我们可以用熟知的符号自定义基本的语言操作。

`Symbol.hasInstance`方法让一个对象定制`instanceof`操作符的行为。

`Symbol.toPrimitive`是一种让我们自定义如何将其转换为原始值的方法。

`Symbol.toStringTag`方法让我们调用`Object.prototype.toString`来计算对象的字符串描述。

`Symbol.unscopables`让我们从`with`语句中隐藏一些属性。

`obj instanceof C`通过做一些检查来工作。

如果`C`不是一个对象，那么它抛出一个`TypeError`。

如果该方法存在，那么它调用`C[Symbol.hasInstance](obj)`。

它将结果强制转换为布尔值并返回它。

否则，它根据常规算法通过检查可伸缩性、`C.prototype`在`obj`的原型链中等等来计算并返回结果。

标准库中唯一有`Symbol.hasInstance`的方法是`Function.prototype`。

我们可以通过编写以下代码来检查对象中的值:

```
const ObjType = {
  [Symbol.hasInstance](value) {
    return (value !== null &&
      (typeof value === 'object' ||
        typeof value === 'function'));
  }
};
```

我们用`Symbol.hasInstance`方法创建自己的`ObjType`对象。

它需要我们要检查的`value`。

然后检查是否不是`bull`,`value`的类型是`'object'`还是`'function'`。

然后我们可以通过写来使用它:

```
{} instanceof ObjType
```

返回`true`。

![](img/397225374af819eb26be0d03e9cd1d9f.png)

Photo by [Glen Carrie](https://unsplash.com/@glencarrie?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

可以更改和检查对象的可枚举性。

此外，我们可以通过用众所周知的符号覆盖方法来改变 or `instanceof`和其他操作符的行为。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**