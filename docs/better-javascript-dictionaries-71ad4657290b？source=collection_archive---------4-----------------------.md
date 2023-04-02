# 更好的 JavaScript —字典

> 原文：<https://javascript.plainenglish.io/better-javascript-dictionaries-71ad4657290b?source=collection_archive---------4----------------------->

![](img/1f2da5e73a17d26d18f589a2356e89e4.png)

Photo by [Sandy Millar](https://unsplash.com/@sandym10?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究改进 JavaScript 代码的方法。

# 从对象构建词典

字典是具有键值对的数据结构。

JavaScript 对象可以动态地添加和删除键值对。

所以我们可以添加和删除项目。

例如，我们可以写:

```
const dict = {
  james: 33,
  bob: 22,
  mary: 41
};
```

然后，我们可以通过各种方式循环条目。

我们可以使用 for-in 循环来遍历这些项目:

```
for (const name in dict) {
  console.log(name, dict[name]);
}
```

我们可以通过编写以下代码来获得带有`Object.keys`的密钥:

```
for (const name of Object.keys(dict)) {
  console.log(name, dict[name]);
}
```

我们把`in`换成了`of`。

同样，我们可以获得所有带有`Object.entries()`的条目:

```
for (const [name, age] of Object.entries(dict)) {
  console.log(name, age);
}
```

我们在循环标题中解构了键值对。

每个对象都从`Object.prototype`继承，所以我们从它那里得到类似`toString`的方法和其他属性。

`Object.keys`，`Object.entries`只循环自己的属性，所以这不成问题。

操作符让我们从一个对象中删除属性。

所以我们可以写:

```
delete james
```

移除`james`属性。

我们不应该用数组来表示字典。

数组可以有属性，因为它们也是对象。

但这是使用数组的错误方式。

例如，如果我们有:

```
const dict = new Array();
dict.james = 34;
dict.bob = 24;
dict.mary = 62;
```

那我们就不应该把它当物件。

一些语言，比如 PHP，有关联数组，也就是字典。

# 使用零原型来防止原型污染

默认情况下，对象的原型是`Object.prototype`。

所以如果我们有:

```
Object.getPrototypeOf({})
```

我们得到了返回的`Object.prototype`。

为了创建一个具有`null`原型的对象，我们可以传递一个具有`null`原型的`Object.create`方法。

例如，我们可以写:

```
const obj = Object.create(null);
```

那么`Object.getPrototypeOf(obj)`就是`null`。

我们还可以通过编写以下代码来设置对象的`__proto__`属性:

```
const obj = { __proto__: null };
```

这是自 ES6 以来用于设置原型的标准属性，因此我们可以使用它将原型设置为`null`。

# 使用 hasOwnPropety 防止原型污染

`in`操作员可以检查对象是否具有给定的属性。

它还检查这些属性的原型。

例如，如果我们有:

```
const dict = {
  james: 33,
  bob: 22,
  mary: 41
};
```

那么如果我们有:

```
console.log('james' in dict);
console.log('toString' in dict);
```

两个日志`true`。

`toString`是继承自`Object.prototype`。

我们可以使用`hasOwnProperty`只检查非继承的属性。

例如，我们可以写:

```
dict.hasOwnProperty("james");
```

然后返回`true`。

但是如果我们有:

```
dict.hasOwnProperty("toString");
```

那就返回`false`。

![](img/1505f7afd9b9b785b3823599940f0821.png)

Photo by [Romain Vignes](https://unsplash.com/@rvignes?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

`in`操作符检查对象及其原型中是否有属性。

`hasOwnProperty`只检查非继承的属性。

对象对于创建字典来说很方便。

喜欢这篇文章吗？如果有，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**