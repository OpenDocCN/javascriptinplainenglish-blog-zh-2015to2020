# 现代 JavaScript 的精华— __proto__ 和描述符

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-proto-and-descriptors-1c5eda001226?source=collection_archive---------15----------------------->

![](img/07dc7a69365d0f3f7c5ec3c4de8968dd.png)

Photo by [Free To Use Sounds](https://unsplash.com/@freetousesoundscom?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 中新的 OOP 特性。

# `__proto__`属性

我们可以使用`__proto__`属性轻松地获取和设置对象的原型。

例如，我们可以写:

```
Object.getPrototypeOf({ __proto__: null })
```

然后它将返回`null`，因为我们将`__proto__`属性设置为`null`。

这不适用于密钥的字符串版本，因为它会创建自己的属性:

```
Object.getPrototypeOf({ ['__proto__']: null })
```

它会返回`Object.prototype`。

并且`Object.keys({ [‘__proto__’]: null })`返回数组`[ ‘__proto__’ ]`。

此外，如果我们定义自己的属性名为`__proto__`，它也不会工作。

例如，我们可以写:

```
const obj = {};
Object.defineProperty(obj, '__proto__', { value: 'bar' })
```

# 没有`Object.prototype`作为原型的对象

我们可以使用带有参数`null`的`Object.create`方法来创建一个没有原型的对象。

例如，我们可以写:

```
const obj = Object.create(null);
```

那么`obj`就没有`__proto__`属性。

这个方法使得创建 dictionary 对象变得容易，我们不希望从它那里继承任何属性。

# `__proto__`和 JSON

`__proto__`可以用`JSON.stringify`字符串化。

例如，我们可以写:

```
JSON.stringify({['__proto__']: 'foo' })
```

然后返回:

```
"{"__proto__":"foo"}"
```

# 检测对 ES6 风格的支持`__proto__`

有几种方法可以检测 ES6 style `__proto__`属性。

我们可以用对象文字中的`Object.prototype.__proto__`属性和`__proto__`来检查它。

例如，我们可以写:

```
const supported = ({}).hasOwnProperty.call(Object.prototype, '__proto__');
```

我们检查`Object.prototype.__proto__`属性是否存在。

我们还可以检查`__proto__`的 getter 和 setter 是函数，以确保我们可以获取和设置该属性的值。

例如，我们可以写:

```
const desc = Object.getOwnPropertyDescriptor(Object.prototype, '__proto__');
const supported = (
  typeof desc.get === 'function' && typeof desc.set === 'function'
);
```

我们还可以使用`Object.getPrototypeOf`方法通过传入一个属性设置为`null`的对象来进行检查。

然后我们可以用它来检查返回的原型是否是`null`。

例如，我们可以写:

```
const supported = Object.getPrototypeOf({__proto__: null}) === null;
```

# ES6 中的可枚举性

在 JavaScript 中，每个对象可以有零个或多个属性。

每个属性都有一个键和 3 个或更多的描述符。

所有属性都有`enumerable`和`configurable` 属性

`enumerable`意味着我们是否到处展示财产。

`configurable`设置为`false`禁用属性值更改并防止删除。

属性有`value`和`writable`属性。

`value`有属性值。

而`writable`控制我们是否可以改变属性的值。

属性也有 getter `get`访问器和 setter `set`访问器。

我们可以使用`Object.getOwnPropertyDescriptor`方法来获取属性描述符。

例如，我们可以写:

```
const obj = { foo: 'bar' };
consr desc = Object.getOwnPropertyDescriptor(obj, 'foo')
```

那么`desc`就是:

```
{
  value: "bar",
  writable: true,
  enumerable: true,
  configurable: true
}
```

![](img/54faba8511f8c7458fbc12b77329f3de.png)

Photo by [Alexander Popov](https://unsplash.com/@5tep5?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

对象属性有属性描述符。

此外，`__proto__`属性在不同的上下文中有不同的行为。

我们可以通过不同的方式来检查该属性是否可用。

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**