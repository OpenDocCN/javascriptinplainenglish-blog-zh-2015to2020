# 现代 JavaScript 精华—属性键和比较

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-property-keys-and-comparisons-d5b6634d273a?source=collection_archive---------10----------------------->

![](img/d6b09849ec8b84050472bf32dbcadca3.png)

Photo by [Free To Use Sounds](https://unsplash.com/@freetousesoundscom?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 中新的 OOP 特性。

# 使用`Object.is()`查找数组元素

我们可以使用`Object.is`来寻找数组元素。

有助于在我阵中找到`NaN`。

例如，我们可以写:

```
const arr = [0, NaN, 2];
const index = arr.findIndex(x => Object.is(x, NaN));
```

我们可以用`findIndex`方法找到`NaN`的指数。

我们用`findIndex`回调函数返回`Object.is`的结果。

# `Object.setPrototypeOf(obj, proto)`

`Object.setPrototyeOf`方法让我们将对象的原型设置为我们想要的对象。

例如，我们可以写:

```
const proto = {
  foo: 'bar'
}
const obj = {
  baz: 'qux'
};
Object.setPrototypeOf(obj, proto);
```

我们调用了`Object.setPrototype`，将我们想要设置原型的对象作为第一个参数。

第二个参数是我们想要为`obj`设置的原型。

现在，当我们记录`__proto__`属性时:

```
console.log(obj.__proto__)
```

我们看到了`proto`物体。

# ES6 中的遍历属性

有几种方法可以用 ES6 遍历对象。

有几种方法可以做到这一点。

`Object.keys`方法接受一个对象并返回一个可枚举的字符串键数组。

`Object.getOwnPropertyNames`方法接受一个对象并返回一个对象所有自己的字符串键的数组。

`Object.getOwnPropertySymbols`方法接受一个对象并返回一个符号自身键的数组。

`Reflect.ownKeys`是一个方法，它接受一个对象并返回一个包含所有字符串和符号自身键的数组。

for-in 循环在 ES6 之前是可用的，它让我们遍历对象拥有的和继承的键。

# 属性的遍历顺序

对于自己的属性键和可枚举的自己的名称，属性的移动顺序是分开的。

own 属性键检索对象中所有 own 属性的键。

字符串 kets 是整数，按数字升序遍历。

然后遍历其他字符串键。

最后，遍历符号键。

这是由`Object.assign()`、`Object.defineProperties()`、`Object.getOwnPropertyNames()`、`Object.getOwnPropertySymbols()`、`Reflect.ownKeys()`方法使用的。

可枚举所有者名称是从对象的所有可枚举所有者属性的字符串关键字中检索的。

ES6 没有定义它，但是它与 for-in 循环遍历属性的顺序相同。

这种方式被`JSON.parse()`、`JSON.stringify()`、`Object.keys()`方法所采用。

# 整数索引

数组元素有整数索引，但它们被视为普通的字符串属性键。

例如，如果我们有:

```
const arr = ['a', 'b', 'c'];
```

然后:

```
console.log(arr['0'])
```

日志`'a'`。

整数索引是特殊的，因为它们影响数组的`length`，并且在遍历属性键时排在第一位。

任何以`‘0’`开头的都不是整数索引。

# 分配与定义属性

向对象添加属性有两种方式。

一种方法是分配它:

```
obj.prop = 'foo';
```

另一种方法是用`Object.defineProperty`方法定义它:

```
Object.defineProperty(obj, 'prop', { value: 'foo' });
```

第一个参数是要添加属性的对象。

第二个是属性名。

第三个包含属性描述符，其中包含值。

![](img/de21f1567586560f83bca1ef1e5b210e.png)

Photo by [Ochir-Erdene Oyunmedeg](https://unsplash.com/@chiklad?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用`Object`方法来获取键并比较值和对象。

还有一种遍历属性的固定方式。

向对象添加属性的方法不止一种。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**