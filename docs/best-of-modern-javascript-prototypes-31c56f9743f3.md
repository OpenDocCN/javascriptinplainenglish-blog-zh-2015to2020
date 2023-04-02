# 现代 JavaScript 的精华——原型

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-prototypes-31c56f9743f3?source=collection_archive---------8----------------------->

![](img/4bc516c46577b9da90ca7a29ad0f14bf.png)

Photo by [Jasmin Schreiber](https://unsplash.com/@lavievagabonde?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 中新的 OOP 特性。

# 重写继承的只读属性

我们可以用`Object.defineProperty`方法覆盖继承的只读属性。

我们不能直接给只读属性赋值。

例如，如果我们有:

```
const proto = Object.defineProperty({}, 'prop', {
  writable: false,
  configurable: true,
  value: 'foo',
});const obj = Object.create(proto);
obj.prop = 'bar';
```

然后我们`obj.prop`保持`'foo'`在松散模式下，我们在严格模式下得到一个错误。

相反，我们可以调用`Object.defineProperty`来覆盖`obj.prop`的属性值。

例如，我们可以写:

```
const proto = Object.defineProperty({}, 'prop', {
  writable: false,
  configurable: true,
  value: 'foo',
});const obj = Object.create(proto);
Object.defineProperty(obj, 'prop', {
  value: 'bar'
});
```

我们在`obj`对象上定义了`prop`属性，以将值添加到`obj`对象中。

这覆盖了`prop`属性，从`obj`的原型到`obj`对象本身。

# `__proto__`在 ES6 中

`__proto__`是 ES6 中的官方隐藏属性。

在 ES5 中，我们可以使用`Object.getPrototypeOf`方法来获得对象的原型。

例如，我们可以写:

```
var obj = {};
var p = Object.getPrototypeOf(obj);
```

用这种方法可以得到`obj`的原型。

要用给定的原型创建一个对象，我们可以使用`Object.create`方法:

```
var obj = Object.create(p);
```

其中`p`为原型。

Firefox 是第一个让曾经非标准的`__proto__`属性允许我们用 Firefox 获取和设置对象原型的浏览器。

例如，我们可以写:

```
var obj = {};
var p = {};
obj.__proto__ = p;
```

然后:

```
console.log(obj.__proto__ === p);
```

日志`true`。

但是当我们试图用以下方法检查该属性是否是`obj`的实际属性时:

```
'__proto__' in obj
```

这将返回`false`。

`__proto__`属性对于创建数组的子类很有用。

例如，我们可以写:

```
function FancyArray() {
  var instance = new Array();
  instance.__proto__ = FancyArray.prototype;
  return instance;
}FancyArray.prototype = Object.create(Array.prototype);
FancyArray.prototype.customMethod = function() {
  //...
};
```

我们通过创建`Array`构造函数的实例来创建`FancyArray`构造函数，

然后，我们将实例的原型设置为我们的`FancyArray`构造器的`prototype`。

我们返回数组的实例。

然后我们用`Object.create`创建`FanctArray`的原型，这样我们就可以从`Array`的原型继承方法。

然后我们可以在原型中创建一个自定义方法。

属性很容易与对象的常规属性混淆。

因此，使用`__proto__`属性来获取和设置对象的属性并不是一个好主意。

`Object.getPrototypeOf()`是获得原型的最好方法。

ES6 标准化了`__proto__`属性，使其广泛可用。

这是一个 getter 和 setter 属性。

ES6 让我们获取并设置`__proto__`属性。

![](img/9dcfe01dd4fc0865ec27ec37c2e322a8.png)

Photo by [Mark König](https://unsplash.com/@markkoenig?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以通过各种方式获得和设置原型。

`__proto__`属性是 ES6 的标配。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**